---
id: react-without-jsx
title: React Without JSX
permalink: docs/react-without-jsx.html
---

Το JSX δεν αποτελεί βασική προϋπόθεση για τη χρήση του React. Η χρήση του React χωρίς JSX είναι ιδιαίτερα βολική όταν δεν θέλετε να ρυθμίσετε τη συλλογή στο περιβάλλον κατασκευής σας (build environment).

Κάθε στοιχείο JSX είναι απλά ένα χαρακτηριστικό (syntactic sugar) για να χρησιμοποιήσουμε το `React.createElement(component, props, ...children)`. Έτσι, οτιδήποτε μπορείτε να κάνετε με το JSX μπορεί επίσης να γίνει με την χρήση απλής JavaScript.

Για παράδειγμα, αυτός ο κωδικός γραμμένος με JSX:

```js
class Hello extends React.Component {
  render() {
    return <div>Hello {this.props.toWhat}</div>;
  }
}

ReactDOM.render(
  <Hello toWhat="World" />,
  document.getElementById('root')
);
```

μπορεί να "μεταφραστεί" σε αυτόν τον κώδικα που δεν χρησιμοποιεί JSX:

```js
class Hello extends React.Component {
  render() {
    return React.createElement('div', null, `Hello ${this.props.toWhat}`);
  }
}

ReactDOM.render(
  React.createElement(Hello, {toWhat: 'World'}, null),
  document.getElementById('root')
);
```

Αν είστε περίεργοι να δείτε περισσότερα παραδείγματα για το πώς το JSX μετατρέπεται σε JavaScript, μπορείτε να δοκιμάσετε [τον online Babel compiler](babel://jsx-simple-example).

Το component μπορεί να παρέχεται ως string, ως ένα subclass του `React.Component`, ή ως μία απλή συνάρτηση.

Εάν έχετε κουραστεί να πληκτρολογείτε το `React.createElement` τόσο πολύ, ένα κοινό μοτίβο (pattern) είναι να το εκχωρήσετε (assign) σε μια συντομογραφία (shorthand):

```js
const e = React.createElement;

ReactDOM.render(
  e('div', null, 'Hello World'),
  document.getElementById('root')
);
```

Εάν χρησιμοποιείτε αυτήν τη "σύντομη φόρμα" (shorthand form) για το `React.createElement`, μπορεί να είναι εξίσου βολικό να χρησιμοποιήσετε το React χωρίς JSX

Εναλλακτικά, μπορείτε να αναφερθείτε σε κοινοτικά (community) έργα (projects), όπως [`react-hyperscript`](https://github.com/mlmorg/react-hyperscript) και [`hyperscript-helpers`](https://github.com/ohanhi/hyperscript-helpers) όπου προσφέρουν πιο σύνθετη σύνταξη.

