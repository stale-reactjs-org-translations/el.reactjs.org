---
id: composition-vs-inheritance
title: Σύνθεση έναντι Κληρονομικότητας
permalink: docs/composition-vs-inheritance.html
redirect_from:
  - "docs/multiple-components.html"
prev: lifting-state-up.html
next: thinking-in-react.html
---

Το React έχει ένα ισχυρό μοντέλο σύνθεσης και σας συνιστούμε να χρησιμοποιήσετε τη σύνθεση αντί της κληρονομικότητας για να επαναχρησιμοποιήσετε τον κώδικα μεταξύ των components.

Σε αυτή την ενότητα θα εξετάσουμε μερικά προβλήματα στα οποία οι developers νέοι στο React συχνά φτάνουν για κληρονομίκότητα και θα δείξουμε πώς μπορούμε να τα λύσουμε με τη σύνθεση.

## Περιορισμός {#containment}

Μερικά components δεν γνωρίζουν από πριν τα children τους. Αυτό είναι ιδιαίτερα κοινό για τα components όπως `Sidebar` ή `Dialog` που αντιπροσωπεύουν γενικά "κουτιά".

Συνιστούμε τα components αυτά να χρησιμοποιούν το ειδικό `children` prop για να μεταφέρουν τα children elements απευθείας στην έξοδο τους:

```js{4}
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

Αυτό επιτρέπει σε άλλα components να περάσουν αυθαίρετα children σε αυτά, εμφωλιάζοντας το JSX:

```js{4-9}
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

**[Δοκιμάστε το στο CodePen](https://codepen.io/gaearon/pen/ozqNOV?editors=0010)**

Οτιδήποτε μέσα στο `<FancyBorder>` JSX tag περνάει στο `FancyBorder` component ως `children` prop. Επειδή το `FancyBorder` κάνει render το `{props.children}` μέσα σε ένα `<div>`, τα περασμένα elements εμφανίζονται στην τελική έξοδο.

Ενώ αυτό είναι λιγότερο κοινό, μερικές φορές μπορεί να χρειαστείτε πολλαπλές "τρύπες" σε ένα component. Σε τέτοιες περιπτώσεις μπορείτε να καταλήξετε στη δική σας σύμβαση αντί να χρησιμοποιήσετε το `children`:

```js{5,8,18,21}
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/gwZOJp?editors=0010)

Τα React elements όπως τα `<Contacts/>` και `<Chat />` είναι απλά objects, επομένως μπορείτε να τα περάσετε ως props όπως οποιαδήποτε άλλα δεδομένα. Αυτή η προσέγγιση μπορεί να σας υπενθυμίσει τα "slots" σε άλλες βιβλιοθήκες, αλλά δεν υπάρχουν περιορισμοί σε αυτό που μπορείτε να περάσετε ως props στο React.

## Ειδίκευση {#specialization}

Μερικές φορές σκεφτόμαστε τα components ως "ειδικές περιπτώσεις" άλλων components. Για παράδειγμα, θα μπορούσαμε να πούμε ότι ένα `WelcomeDialog` είναι μια ειδική περίπτωση του `Dialog`.

Στο React, αυτό επιτυγχάνεται επίσης με τη σύνθεση, όπου ένα πιο "ειδικό" component κάνει render ένα πιο "γενικό" και το διαμορφώνει με props:


```js{5,8,16-18}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/kkEaOZ?editors=0010)

Η σύνθεση λειτουργεί εξίσου καλά για τα components που ορίζονται ως κλάσεις:

```js{10,27-31}
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
      {props.children}
    </FancyBorder>
  );
}

class SignUpDialog extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.handleSignUp = this.handleSignUp.bind(this);
    this.state = {login: ''};
  }

  render() {
    return (
      <Dialog title="Mars Exploration Program"
              message="How should we refer to you?">
        <input value={this.state.login}
               onChange={this.handleChange} />
        <button onClick={this.handleSignUp}>
          Sign Me Up!
        </button>
      </Dialog>
    );
  }

  handleChange(e) {
    this.setState({login: e.target.value});
  }

  handleSignUp() {
    alert(`Welcome aboard, ${this.state.login}!`);
  }
}
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/gwZbYa?editors=0010)

## Τι Γίνεται Με Την Κληρονομικότητα? {#so-what-about-inheritance}

Στο Facebook, χρησιμοποιούμε το React σε χιλιάδες components και δεν βρήκαμε καμιά περίπτωση χρήσης όπου θα συνιστούσαμε τη δημιουργία ιεραρχιών κληρονομικότητας component.

Τα props και η σύνθεση σας δίνουν όλη την ευελιξία που χρειάζεστε για να προσαρμόσετε την εμφάνιση και τη συμπεριφορά ενός component με έναν σαφή και ασφαλή τρόπο. Θυμηθείτε ότι τα components ενδέχεται να δέχονται αυθαίρετα props, συμπεριλαμβανομένων των primitive values, React elements ή συναρτήσεις.

<<<<<<< HEAD
Εάν θέλετε να επαναχρησιμοποιήσετε κάποια λειτουργία εκτός του UI μεταξύ των components, σας συνιστούμε να την εξαγάγετε σε ένα ξεχωριστό JavaScript module. Τα components μπορούν να το κάνουν import και να χρησιμοποιούν αυτή τη συνάρτηση, object ή κλάση, χωρίς να το επεκτείνουν.
=======
If you want to reuse non-UI functionality between components, we suggest extracting it into a separate JavaScript module. The components may import it and use that function, object, or class, without extending it.
>>>>>>> 38bf76a4a7bec6072d086ce8efdeef9ebb7af227
