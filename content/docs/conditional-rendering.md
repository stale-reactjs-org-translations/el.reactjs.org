---
id: conditional-rendering
title: Υπό συνθήκες Rendering
permalink: docs/conditional-rendering.html
prev: handling-events.html
next: lists-and-keys.html
redirect_from:
  - "tips/false-in-jsx.html"
---

Στο React, μπορείτε να δημιουργήσετε ξεχωριστά components που ενσωματώνουν τη συμπεριφορά που χρειάζεστε. Στη συνέχεια, μπορείτε να κάνετε render μόνο μαερικά από αυτά, ανάλογα με το state της εφαρμογής σας.

Το rendering υπό συνθήκες στο React λειτουργεί με τον ίδιο τρόπο που λειτουργούν οι συνθήκες στη JavaScript. Χρησιμοποιήστε τα JavaScript operators όπως το [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) ή το [conditional operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) για να δημιουργήσετε components που αντιπροσωπεύουν το τρέχον state και αφήστε το React να ενημερώσει το UI για να τα ταιριάξει.

Εξετάστε αυτά τα δύο components:

```js
function UserGreeting(props) {
  return <h1>Welcome back!</h1>;
}

function GuestGreeting(props) {
  return <h1>Please sign up.</h1>;
}
```

Θα δημιουργήσουμε ένα `Greeting` component που εμφανίζει ένα από τα παραπάνω components ανάλογα με το αν ένας χρήστης είναι συνδεδεμένος ή οχι:

```javascript{3-7,11,12}
function Greeting(props) {
  const isLoggedIn = props.isLoggedIn;
  if (isLoggedIn) {
    return <UserGreeting />;
  }
  return <GuestGreeting />;
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
// Try changing to isLoggedIn={true}:
root.render(<Greeting isLoggedIn={false} />);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/ZpVxNq?editors=0011)

Αυτό το παράδειγμα εμφανίζει διαφορετικό χαιρετισμό ανάλογα με την τιμή του `isLoggedIn` prop.

### Μεταβλητές στοιχείων {#element-variables}

Μπορείτε να χρησιμοποιήσετε μεταβλητές για την αποθήκευση στοιχείων. Αυτό μπορεί να σας βοηθήσει να κάνετε render υπό συνθήκες ένα μέρος του component ενώ το υπόλοιπο output δεν αλλάζει.

Εξετάστε αυτά τα δύο νέα components που αντιπροσωπεύουν τα κουμπιά Logout και Login:

```js
function LoginButton(props) {
  return (
    <button onClick={props.onClick}>
      Login
    </button>
  );
}

function LogoutButton(props) {
  return (
    <button onClick={props.onClick}>
      Logout
    </button>
  );
}
```

Στο παρακάτω παράδειγμα, θα δημιουργήσουμε ένα [stateful component](/docs/state-and-lifecycle.html#adding-local-state-to-a-class) που ονομάζεται `LoginControl`.

Αυτό θα κάνει render είτε το `<LoginButton />` είτε το `<LogoutButton />` ανάλογα με το τρέχον state του. Επίσης θα κάνει render ένα `<Greeting />` από το προηγούμενο παράδειγμα:

```javascript{20-25,29,30}
class LoginControl extends React.Component {
  constructor(props) {
    super(props);
    this.handleLoginClick = this.handleLoginClick.bind(this);
    this.handleLogoutClick = this.handleLogoutClick.bind(this);
    this.state = {isLoggedIn: false};
  }

  handleLoginClick() {
    this.setState({isLoggedIn: true});
  }

  handleLogoutClick() {
    this.setState({isLoggedIn: false});
  }

  render() {
    const isLoggedIn = this.state.isLoggedIn;
    let button;

    if (isLoggedIn) {
      button = <LogoutButton onClick={this.handleLogoutClick} />;
    } else {
      button = <LoginButton onClick={this.handleLoginClick} />;
    }

    return (
      <div>
        <Greeting isLoggedIn={isLoggedIn} />
        {button}
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<LoginControl />);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/QKzAgB?editors=0010)

Ενώ δηλώνεται μια μεταβλητή και χρησιμοποιείτε μια εντολή `if` είναι ένας καλός τρόπος για την υπό συνθήκη render ενός component, μερικές φορές μπορεί να θέλετε να χρησιμοποιήσετε μια σύντομη σύνταξη. Υπάρχουν μερικοί τρόποι για να εισάγετε τις συνθήκες στο JSX, που εξηγείται παρακάτω.

### If σε μια γραμμή με το λογικό Οperator && {#inline-if-with-logical--operator}

<<<<<<< HEAD
Μπορείτε να [ενσωματώσετε οποιεσδήποτε εκφράσεις μέσα στο JSX](/docs/introducing-jsx.html#embedding-expressions-in-jsx) περικλείοντας τα σε άγκιστρα. Αυτό περιλαμβάνει τον λογικό operator της JavaScript `&&`. Μπορεί να είναι βολικό για να συμπεριλάβει υπό συνθήκη ένα component:
=======
You may [embed expressions in JSX](/docs/introducing-jsx.html#embedding-expressions-in-jsx) by wrapping them in curly braces. This includes the JavaScript logical `&&` operator. It can be handy for conditionally including an element:
>>>>>>> d483aebbac6d3c8f059b52abf21240bc91d0b96e

```js{6-10}
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

const messages = ['React', 'Re: React', 'Re:Re: React'];

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<Mailbox unreadMessages={messages} />);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/ozJddz?editors=0010)

Λειτουργεί επειδή στην JavaScript, το `true && expression` πάντα αποτιμάται σε `expression`, και το `false && expression` πάντα αποτιμάται σε `false`.

Επομένως, αν η κατάσταση είναι `true`, το component αμέσως μετά το `&&` θα εμφανιστεί στο output. Εάν είναι `false`, το React θα το αγνοήσει.

Note that returning a falsy expression will still cause the element after `&&` to be skipped but will return the falsy expression. In the example below, `<div>0</div>` will be returned by the render method.

```javascript{2,5}
render() {
  const count = 0;
  return (
    <div>
      {count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```

### Inline If-Else with Conditional Operator {#inline-if-else-with-conditional-operator}

Μια άλλη μέθοδος για να κάνετε rendering υπό συνθήκες τα components σε μια γραμμή, είναι η χρήση του conditional operator στη JavaScript [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator).

Στο παρακάτω παράδειγμα, το χρησιμοποιούμε για να κάνουμε render υπό συνθήκες ένα μικρό κομμάτι κειμένου.

```javascript{5}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
    </div>
  );
}
```

Μπορεί επίσης να χρησιμοποιηθεί για μεγαλύτερες εκφράσεις, αν και είναι λιγότερο προφανές τι συμβαίνει:

```js{5,7,9}
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

Όπως και στη JavaScript, εξαρτάται από εσάς να επιλέξετε ένα κατάλληλο στυλ με βάση αυτό που εσείς και η ομάδα σας θεωρείτε πιο ευανάγνωστο. Επίσης, να θυμάστε ότι όταν οι συνθήκες είναι υπερβολικά περίπλοκες, ίσως είναι καλή στιγμή να [εξάγετε ένα component](/docs/components-and-props.html#extracting-components).

### Αποτρέψτε το Component από το Rendering {#preventing-component-from-rendering}

Σε σπάνιες περιπτώσεις μπορεί να θέλετε ένα component να μην εμφανίζεται, ακόμα και αν έχει γίνει render από ένα άλλο component. Για να το κάνετε αυτό, επιστρέψτε `null` αντί της κανονικής του εμφάνισης.

Στο παρακάτω παράδειγμα, το `<WarningBanner />` θα εμφανιστεί ανάλογα με την τιμή του prop που ονομάζεται `warn`. Εάν η τιμή του prop είναι `false`, τότε το component δεν θα εμφανιστεί:

```javascript{2-4,29}
function WarningBanner(props) {
  if (!props.warn) {
    return null;
  }

  return (
    <div className="warning">
      Warning!
    </div>
  );
}

class Page extends React.Component {
  constructor(props) {
    super(props);
    this.state = {showWarning: true};
    this.handleToggleClick = this.handleToggleClick.bind(this);
  }

  handleToggleClick() {
    this.setState(state => ({
      showWarning: !state.showWarning
    }));
  }

  render() {
    return (
      <div>
        <WarningBanner warn={this.state.showWarning} />
        <button onClick={this.handleToggleClick}>
          {this.state.showWarning ? 'Hide' : 'Show'}
        </button>
      </div>
    );
  }
}

const root = ReactDOM.createRoot(document.getElementById('root')); 
root.render(<Page />);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/Xjoqwm?editors=0010)

Επιστρέφοντας `null` απο την μέθοδο `render` του component, δεν επηρεάζει την ενεργοποίηση των μεθόδων του κύκλου ζωής του component. Για παράδειγμα το `componentDidUpdate` θα εξακολουθεί να καλείτε.

