---
id: handling-events
title: Διαχείριση των Events
permalink: docs/handling-events.html
prev: state-and-lifecycle.html
next: conditional-rendering.html
redirect_from:
  - "docs/events-ko-KR.html"
---

<<<<<<< HEAD
Η διαχείριση των events με τα React elements είναι παρόμοια με τη διαχείριση των events στα DOM elements. Υπάρχουν μερικές διαφορές στο συντακτικό:
=======
Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences:
>>>>>>> 95e15d063b205007a92c52efb5311f76ad5a0b6c

* Στο React τα events ονομάζονται χρησιμοποιώντας camelCase, αντί για lowercase.
* Με το JSX περνάτε μια συνάρτηση ως event handler, αντί για ένα string.

Για παράδειγμα, το HTML:

```html
<button onclick="activateLasers()">
  Activate Lasers
</button>
```

είναι λίγο διαφορετικό στο React:

```js{1}
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

<<<<<<< HEAD
Ακόμη μια διαφορά είναι ότι δεν μπορείτε να επιστρέψετε `false` για να σταματήσετε την προκαθορισμένη συμπεριφορά στο React. Θα πρέπει να καλέσετε τη μέθοδο `preventDefault`. Για παράδειγμα, με απλό HTML, για να σταματήσετε τη προκαθορισμένη συμπεριφορά του link, που ανοίγει μια νέα σελίδα, μπορείτε να γράψετε:
=======
Another difference is that you cannot return `false` to prevent default behavior in React. You must call `preventDefault` explicitly. For example, with plain HTML, to prevent the default form behavior of submitting, you can write:
>>>>>>> 95e15d063b205007a92c52efb5311f76ad5a0b6c

```html
<form onsubmit="console.log('You clicked submit.'); return false">
  <button type="submit">Submit</button>
</form>
```

Στο React αυτό θα μπορούσε να είναι:

```js{3}
function Form() {
  function handleSubmit(e) {
    e.preventDefault();
    console.log('You clicked submit.');
  }

  return (
    <form onSubmit={handleSubmit}>
      <button type="submit">Submit</button>
    </form>
  );
}
```

<<<<<<< HEAD
Εδώ το `e` είναι ένα synthetic event. Το React ορίζει αυτά τα synthetic events σύμφωνα με το [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), οπότε δεν χρειάζεται να ανησυχείτε για τη συμβατότητα μεταξύ των browsers. Δείτε τον οδηγό αναφοράς του [`SyntheticEvent`](/docs/events.html) για να μάθετε περισσότερα.
=======
Here, `e` is a synthetic event. React defines these synthetic events according to the [W3C spec](https://www.w3.org/TR/DOM-Level-3-Events/), so you don't need to worry about cross-browser compatibility. React events do not work exactly the same as native events. See the [`SyntheticEvent`](/docs/events.html) reference guide to learn more.
>>>>>>> 95e15d063b205007a92c52efb5311f76ad5a0b6c

Όταν χρησιμοποιείτε το React, γενικά δεν θα χρειαστεί να καλείτε το `addEventListener` για να προσθέσετε listeners σε ένα DOM element μετά τη δημιουργία του. Αντί για αυτό, απλά προσθέστε ένα listener όταν το element γίνεται αρχικά rendered.

Όταν ορίζετε ένα component χρησιμοποιώντας μια [ES6 κλάση](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), ένα κοινό μοτίβο για ένα event handler είναι να ορίζεται ως μια μέθοδος της κλάσης. Για παράδειγμα αυτό το `Toggle` component κάνει render ένα button που αφήνει τον χρήστη να κάνει εναλλαγή μεταξύ των "ON" και "OFF" states:

```js{6,7,10-14,18}
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // Αυτό το binding είναι απαραίτητο για να κάνει το `this` να δουλέψει στο callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

[**Δοκιμάστε το στο Codepen**](https://codepen.io/gaearon/pen/xEmzGg?editors=0010)

Πρέπει να είστε προσεκτικοί με τη σημασία του `this` στα JSX callbacks. Στη JavaScript, οι μεθόδοι των κλάσεων δεν είναι [bound](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) από προεπιλογή. Αν ξεχάσετε να κάνετε bind το `this.handleClick` και το περάσετε στο `onClick`, το `this` θα είναι `undefined` όταν καλεστεί η συνάρτηση.

Αυτή η συμπεριφορά δεν σχετίζεται με το React. Αποτελεί μέρος του [πως λειτουργούν οι συναρτήσεις στη JavaScript](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/). Γενικά, για να αναφερθείτε σε μια μέθοδο χωρίς να την ακολουθούν `()` όπως το `onClick={this.handleClick}`, θα πρέπει να κάνετε bind αυτή τη μέθοδο.

Αν το να καλείτε το `bind` είναι ενοχλητικό, υπάρχουν δύο τρόποι που μπορείτε να το αποφύγετε. Αν χρησιμοποιείτε το πειραματικό [public class fields syntax](https://babeljs.io/docs/plugins/transform-class-properties/), μπορείτε να χρησιμοποιήσετε πεδία κλάσεων για να κάνετε bind τα callbacks:

```js{2-6}
class LoggingButton extends React.Component {
  // Αυτή η σύνταξη εξασφαλίζει ότι το `this` είναι bound μέσα στο handleClick.
  // Προσοχή: αυτό το συντακτικό είναι *πειραματικό*
  handleClick = () => {
    console.log('this is:', this);
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        Click me
      </button>
    );
  }
}
```

Αυτή η σύνταξη είναι ενεργοποιημένη από προεπιλογή στο [Create React App](https://github.com/facebookincubator/create-react-app).

Αν δεν χρησιμοποιήτε το συντακτικό με τα πεδία των κλάσεων, μπορείτε να χρησιμοποιήσετε ένα [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) στο callback:

```js{7-9}
class LoggingButton extends React.Component {
  handleClick() {
    console.log('this is:', this);
  }

  render() {
    // Αυτή η σύνταξη εξασφαλίζει ότι το `this` είναι bound στο handleClick
    return (
      <button onClick={() => this.handleClick()}>
        Click me
      </button>
    );
  }
}
```

Το πρόβλημα με αυτό το συντακτικό είναι ότι δημιουργείται ένα διαφορετικό callback κάθε φορά που το `LoggingButton` κάνει render. Στις περισσότερες περιπτώσεις αυτό είναι οκ. Ωστόσο, εάν αυτό το callback περνιέται σαν prop σε lower components, αυτά τα components μπορεί να κάνουν ένα περιττό re-rendering. Συνήθως συνηστούμε να κάνετε bind στο constructor ή χρησιμοποιώντας τη σύνταξη με τα πεδία των κλάσεων, για να αποφύγετε τέτοια προβλήματα αποδοτικότητας.

## Περνώντας Arguments σε Event Handlers {#passing-arguments-to-event-handlers}

Μέσα σε ένα loop είναι κοινό να θέλετε να περάσετε μια επιπλέον παράμετρο σε ένα event handler. Για παράδειγμα, αν το `id` είναι το row ID, καθένα από τα παρακάτω θα λειτουργούσε:

```js
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

Οι παραπάνω δύο γραμμές είναι ισοδύναμες και χρησιμοποιούν [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) και το [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) αντίστοιχα.

Και στις δυο περιπτώσεις, το `e` argument που αντιπροσωπεύει το React event θα περαστεί ως δεύτερο argument μετά το ID. Με ένα arrow function, θα πρέπει να το περάσουμε συγκεκριμένα, αλλά με το `bind` τα ακόλουθα arguments προωθούνται αυτόματα.
