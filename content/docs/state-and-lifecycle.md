---
id: state-and-lifecycle
title: State και Lifecycle
permalink: docs/state-and-lifecycle.html
redirect_from:
  - "docs/interactivity-and-dynamic-uis.html"
prev: components-and-props.html
next: handling-events.html
---
Αυτή η σελίδα παρουσιάζει την έννοια του state και lifecycle σε ένα React component. Μπορείτε να βρείτε μια [λεπτομερή αναφορά για το component API εδώ](/docs/react-component.html).

Λάβετε υπ'όψιν το παράδειγμα με το ρολόι που χτυπάει [σε μια από τις προηγούμενες ενότητες](/docs/rendering-elements.html#updating-the-rendered-element).
Στα [Rendering Elements](/docs/rendering-elements.html#rendering-an-element-into-the-dom), έχουμε μάθει μόνο έναν τροπό για να κάνουμε ανανέωση το UI. Καλούμε `ReactDOM.render()` για να αλλάξουμε το rendered output:

```js{8-11}
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/gwoJZk?editors=0010)

Σε αυτή την ενότητα, θα μάθουμε πώς να κάνουμε το `Clock` component πραγματικά επαναχρησιμοποιήσιμο και περικλειόμενο. Θα ρυθμίσει το δικό του χρονοδιακόπτη και θα ενημερώνει τον εαυτό του κάθε δευτερόλεπτο.

Μπορούμε να ξεκινήσουμε περικλείοντας με το πως φένεται το ρολόι:

```js{3-6,12}
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/dpdoYR?editors=0010)

Ωστόσο, λείπει μια κρίσιμη απαίτηση: το γεγονός ότι το `Clock` δημιουργεί ένα χρονοδιακόπτη και ενημερώνει το UI κάθε δευτερόλεπτο θα πρέπει να είναι μια λεπτομέρεια εφαρμογής του `Clock`.

Ιδανικά θέλουμε να το γράψουμε μια φορά και το `Clock` να ενημερώνει τον εαυτό του:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Για να το υλοποιήσουμε, χρειάζεται να προσθέσουμε το "state" στο `Clock` component.

Το state είναι παρόμοιο των props, αλλά είναι private και ελέγχεται πλήρως από το component.

## Μετατροπή μιας Function σε μια Κλάση {#converting-a-function-to-a-class}

Μπορείτε να μετατρέψετε ένα function component σαν το `Clock` σε μία κλάση με πέντε βήματα:

1. Δημιουργήστε μία [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes), με το ίδιο όνομα, που επεκτείνει το `React.Component`.

2. Προσθέστε μια μόνο κενή μέθοδο και ονομάσετε την `render()`.

3. Μετακινήστε το περιεχόμενο του function στην `render()` μέθοδο.

4. Αντικαταστήστε το `props` με `this.props` στο περιεχόμενο της `render()`.

5. Διαγράψτε τις υπόλοιπες κενές function δηλώσεις.

```js
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/zKRGpo?editors=0010)

Το `Clock` έχει πλέον οριστεί ως κλάση παρά ως function.

H `render` μέθοδος θα καλεστεί κάθε φορά που συμβαίνει μια ανανέωση, αλλά εφ 'όσον κάνουμε render `<Clock />` μέσα στο ίδιο DOM node, μόνο ένα αντικείμενο της `Clock` κλάσης θα χρησιμοποιηθεί. Αυτό μας επιτρέπει να χρησιμοποιήσουμε πρόσθετες λειτουργίες όπως local state και lifecycle methods.

## Προσθέτοντας Local State σε μια Κλάση {#adding-local-state-to-a-class}

Θα αλλάξουμε το `date` από props σε state σε τρία βήματα:

1) Aντικαταστήστε `this.props.date`  με `this.state.date` στην `render()` μέθοδο:

```js{6}
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

2) Προσθέστε [constructor κλάσης](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor) που αναθέτει την αρχική `this.state`:

```js{4}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Προσέξτε πώς περνάμε `props` στον βασικό constructor:

```js{2}
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
```

Τα κλάσεις components πρέπει πάντα να καλούν τον βασικό constructor με `props`.

3) Αφαιρέστε το `date` prop από το `<Clock />` στοιχείο:

```js{2}
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

Αργότερα θα προσθέσουμε τον κώδικα του χρονοδιακόπτη πίσω στο ίδιο component.

Το αποτέλεσμα μοιάζει με αυτό:

```js{2-5,11,18}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/KgQpJd?editors=0010)

Στη συνέχεια, θα κάνουμε το `Clock` να ρυθμίσει το δικό του χρονοδιακόπτη και να ενημερώνεται κάθε δευτερόλεπτο.

## Προσθέτωντας Lifecycle Methods σε μια Κλάση {#adding-lifecycle-methods-to-a-class}

Σε εφαρμογές με πολλά components, είναι πολύ σημαντικό να ελευθερώσετε πόρους που λαμβάνονται από τα components όταν αυτά καταστρέφονται.

Θέλουμε να [ρυθμίσουμε ένα χρονοδιακόπτη](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) όταν το `Clock` είναι rendered στο DOM για πρώτη φορά. Αυτό ονομάζεται "mounting" στο React.

Επίσης θέλουμε να [σβήσουμε τον χρονοδιακόπτη](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/clearInterval) κάθε φορά που το DOM που παράγεται από το `Clock` αφαιρείται. Αυτό ονομάζεται "unmounting" στο React.

Μπορούμε να δηλώσουμε ειδικές μεθόδους στην κλάση component ώστε να εκτελεί κάποιο κώδικα όταν ένα component γινεται mount και unmount:

```js{7-9,11-13}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {

  }

  componentWillUnmount() {

  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

Αυτές οι μέθοδοι ονομάζονται "lifecycle methods".

Η `componentDidMount()` μέθοδος εκτελείται μετά το component output αφότου γινει rendered στο DOM. Αυτό είναι ένα καλό μέρος για να ρυθμίσετε ένα χρονοδιακόπτη:

```js{2-5}
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
```

<<<<<<< HEAD
Σημειώστε πως αποθηκεύουμε το ID χρονοδιακόπτη σωστά στο `this`.
=======
Note how we save the timer ID right on `this` (`this.timerID`).
>>>>>>> 4af9f2dcd1014c18ea6ce98794ba0d63874ac9d2

Παρόλο που το `this.props` έχει στηθεί από το ίδιο το React και το `this.state` έχει ιδιαίτερη σημασία, μπορείτε να προσθέσετε χειροκίνητα επιπλέον πεδία στην κλάση, σε περίπτωση που πρέπει να αποθηκεύσετε κάτι που δεν συμμετέχει στo data flow (όπως ένα ID χρονοδιακόπτη).

Θα καταστρέψουμε το χρονοδιακόπτη στο `componentWillUnmount()` lifecycle method:

```js{2}
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
```

Τέλος, θα εφαρμόσουμε μια μέθοδο που ονομάζεται `tick()` την οποία το `Clock` component θα εκτελεί κάθε δευτερόλεπτο.

Θα χρησιμοποιήσει `this.setState()` για να προγραμματίσετε ενημερώσεις στο local state του component:

```js{18-22}
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/amqdNA?editors=0010)

Τώρα το ρολόι χτυπάει κάθε δευτερόλεπτο.

Ας ανασκοπήσουμε γρήγορα τι συμβαίνει και τη σειρά με την οποία καλούνται οι μέθοδοι:

1) Όταν το `<Clock />` έχει περαστεί στο `ReactDOM.render()`, το React καλεί τον constructor του `Clock` component. Δεδομένου ότι το `Clock` χρειάζεται να εμφανίσει την τρέχουσα ώρα, αρχικοποιεί το `this.state` με ένα αντικείμενο που περιλαμβάνει την τρέχουσα ώρα. Θα ενημερώσουμε αργότερα αυτό το state.

2) Το React τότε καλεί την `render()` method του `Clock` component's . Με αυτό τον τρόπο το React ενημερώνεται για το τι πρέπει να εμφανίζεται στην οθόνη. Το React τότε ενημερώνει το DOM για να ταιριάζει με την render output του `Clock`'s.

3) Όταν το `Clock` output έχει εισαχθεί στο DOM, το React καλέι την `componentDidMount()` lifecycle method. Ενόσω βρίσκεται σε αυτήν, το `Clock` component ζητά από τον browser να ρυθμίσει ένα χρονοδιακόπτη για να καλέσει την `tick()` μεθοδο του component μια φορά κάθε δευτερόλεπτο.

4) Κάθε δευτερόλεπτο το πρόγραμμα περιήγησης καλεί την `tick()` method. Μέσα σε αυτήν, το `Clock` component προγραμματίζει μια ενημέρωση του UI κάνοντας κλήση στην `setState()` με ένα αντικείμενο που περιέχει την τρέχουσα ώρα. Χάρη στην `setState()` κλήση, το React γνωρίζει ότι το state έχει αλλάξει, και καλεί την `render()` method πάλι για να μάθει τι θα πρέπει να βρίσκεται στην οθόνη. Αυτή τη φορά, η `this.state.date` στην `render()` method θα είναι διαφορετική, και έτσι το render output θα περιλαμβάνει την ενημερωμένη ώρα. Το React ενημερώνει το DOM αναλόγως.

5) Εάν το `Clock` component κάποτε αφαιρεθεί από το DOM, το React καλεί τιην `componentWillUnmount()` lifecycle method οπότε ο χρονοδιακόπτης σταματάει.

## Χρησιμοποιώντας Σωστά το State {#using-state-correctly}

Υπάρχουν τρία πράγματα που πρέπει να ξέρετε σχετικά με την `setState()`.

### Μην τροποποιείτε απευθείας το State {#do-not-modify-state-directly}

Για παράδειγμα, αυτό δεν θα κάνει re-render το component:

```js
// Wrong
this.state.comment = 'Hello';
```

Instead, use `setState()`:

```js
// Correct
this.setState({comment: 'Hello'});
```

Το μόνο μέρος όπου μπορείτε να αναθέσετε το `this.state` είναι ο constructor.

### State Updates May Be Asynchronous {#state-updates-may-be-asynchronous}

Το React μπορεί να κάνει πολλαπλές `setState()` κλήσεις σε μία μόνο ενημέρωση για απόδοση.

Επειδή το `this.props` και το `this.state` μπορεί να ενημερωθούν ασύγχρονα, δεν πρέπει να βασίζεστε στις τιμές τους για τον υπολογισμό του επόμενου state.

Για παράδειγμα, αυτός ο κώδικας ενδέχεται να αποτύχει να ενημερώσει τον μετρητή:

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
```

Για να το διορθώσετε, χρησιμοποιήστε μια δεύτερη φόρμα του `setState()` που δέχεται ένα function παρά ένα αντικείμενο. Αυτή η function θα λάβει το προηγούμενο state ως το πρώτο argument, και τα props την στιγμή που γίνεται η ενημέρωση και εφαρμόζεται ως δεύτερο argument:

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));
```

Χρησιμοποιήσαμε ένα [arrow function](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions) παραπάνω, αλλά λειτουργεί και με κανονικά functions:

```js
// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

### Οι State ενημερώσεις συγχωνεύονται {#state-updates-are-merged}

Όταν καλείτε `setState()`, το React συγχωνεύει το αντικείμενο που παρέχετε στο τρέχων state.

Για παράδειγμα, το state σου μπορεί να περιέχει πολλές ανεξάρτητες μεταβλητές:

```js{4,5}
  constructor(props) {
    super(props);
    this.state = {
      posts: [],
      comments: []
    };
  }
```

Στη συνέχεια, μπορείτε να τις ενημερώσετε ανεξάρτητα με ξεχωριστές `setState()` κλήσεις:

```js{4,10}
  componentDidMount() {
    fetchPosts().then(response => {
      this.setState({
        posts: response.posts
      });
    });

    fetchComments().then(response => {
      this.setState({
        comments: response.comments
      });
    });
  }
```

Η συγχώνευση είναι shallow, οπότε `this.setState({comments})` αφήνει `this.state.posts` άθικτο, αλλά αντικαθιστά εντελώς `this.state.comments`.

## Τα Δεδομένα Ρέουν Προς τα Κάτω {#the-data-flows-down}

Ούτε τα parent ούτε τα child components μπορούν να γνωρίζουν εάν ένα συγκεκριμένο component είναι stateful ή stateless και δεν πρέπει να ενδιαφέρονται για το αν ορίζεται ως function ή κλάση.

Αυτός είναι ο λόγος για τον οποίο το state συχνά ονομάζεται local ή encapsulated. Δεν είναι προσβάσιμο σε κανένα άλλο component εκτός από αυτό στο οποίο ανήκει και μπορεί να το ορίζει.

Ένα component μπορεί να επιλέξει να περάσει το state του ως props στα child components:

```js
<<<<<<< HEAD
<h2>It is {this.state.date.toLocaleTimeString()}.</h2>
```

Αυτό επίσης λειτουργεί και για τα user-defined components:

```js
=======
>>>>>>> ed88a240d9c97822cc2f02074306965a1a4f4ac4
<FormattedDate date={this.state.date} />
```

Το `FormattedDate` component θα λάβει το `date` μέσα στα props και δε θα ξέρει αν αυτό προήλθε από το `Clock`'s state, από τα `Clock`'s props, ή γράφτηκε με το χέρι:

```js
function FormattedDate(props) {
  return <h2>It is {props.date.toLocaleTimeString()}.</h2>;
}
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/zKRqNB?editors=0010)

Αυτό συνήθως ονομάζεται "top-down" ή "unidirectional" ροή δεδομένων. Οποιοδήποτε state ανήκει πάντα σε κάποιο συγκεκριμένο component, και οποιαδήποτε δεδομένα ή UI που προέρχονται από αυτό το state μπορούν να επηρεάσουν μόνο components "κάτω" από αυτά στο δέντρο.

Εάν φανταστείτε ένα component δέντρο ως καταρράκτη από props, το state κάθε component είναι σαν μια πρόσθετη πηγή νερού που συνδέεται σε ένα αυθαίρετο σημείο, αλλά επίσης ρέει προς τα κάτω.

Για να δείξουμε ότι όλα τα components είναι πραγματικά απομονωμένα, μπορούμε να δημιουργήσουμε ένα `App` component όπως δείχνει το δέντρο `<Clock>`s:

```js{4-6}
function App() {
  return (
    <div>
      <Clock />
      <Clock />
      <Clock />
    </div>
  );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

[**Δοκιμάστε το στο CodePen**](https://codepen.io/gaearon/pen/vXdGmd?editors=0010)

Κάθε `Clock` δημιουργεί το δικό του χρονοδιακόπτη και ενημερώνεται ανεξάρτητα.

Στα React apps, αν ένα component είναι stateful ή stateless θεωρείται ως μια λεπτομέρεια της υλοποίησης του component η οποία μπορεί να αλλάξει με την πάροδο του χρόνου. Μπορείτε να χρησιμοποιήσετε stateless components μέσα σε stateful components και αντίστροφα.
