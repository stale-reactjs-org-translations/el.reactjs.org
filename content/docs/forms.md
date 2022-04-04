---
id: forms
title: Φόρμες
permalink: docs/forms.html
prev: lists-and-keys.html
next: lifting-state-up.html
redirect_from:
  - tips/controlled-input-null-value.html
  - docs/forms-zh-CN.html
---

<<<<<<< HEAD
Tα HTML form elements λειτουργούν λίγο διαφορετικά από τα υπόλοιπα DOM elements στο React, λόγω του ότι τα form elements συνήθως διατηρούν εσωτερικό state.  Για παράδειγμα, η επόμενη φόρμα σε απλή HTML δέχεται απλά ένα όνομα:
=======
HTML form elements work a bit differently from other DOM elements in React, because form elements naturally keep some internal state. For example, this form in plain HTML accepts a single name:
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

```html
<form>
  <label>
    Name:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="Submit" />
</form>
```

Αυτή η φόρμα έχει την προεπιλεγμένη συμπεριφορά φόρμας HTML, αυτή της περιήγησης σε μια νέα σελίδα όταν ο χρήστης υποβάλει αυτή τη φόρμα. Στο React η συμπεριφορά της φόρμας είναι ακριβώς η ίδια. Απλά σε πολλές περιπτώσεις, είναι πιο βολικό να υπάρχει μια Javascript function, η οποία θα διαχειρίζεται την υποβολή της φόρμας και θα έχει πρόσβαση στα δεδομένα τα οποία ο χρήστης εισήγαγε σε αυτή. Ο συνήθης τρόπος για να επιτευχθεί αυτό είναι με μια τεχνική που ονομάζεται "controlled components".

## Controlled Components {#controlled-components}

Σε HTML, τα form elements όπως `<input>`, `<textarea>`, και `<select>`  διατηρούν συνήθως το δικό τους state και τo ενημερώνουν με βάση τα στοιχεία εισαγωγής του χρήστη. Στο React, το mutable state συνήθως διατηρείται στο state των components, και ενημερώνεται μόνο με [`setState()`](/docs/react-component.html#setstate).

Μπορούμε να συνδυάσουμε τα δύο, κάνοντας το React state να είναι η "μόνη πηγή της αλήθειας". Στη συνέχεια, το React component που κάνει render μία φόρμα, ελέγχει επίσης τί συμβαίνει σε αυτή τη φόρμα σύμφωνα με την αλληλεπίδραση του χρήστη σε αυτή. Ένα input form element του οποίου η τιμή ελέγχεται από το React με αυτόν τον τρόπο, ονομάζεται "controlled component".

Για παράδειγμα, εάν θέλουμε να κάνουμε το προηγούμενο παράδειγμα να καταγράψει το όνομα όταν υποβληθεί, μπορούμε να γράψουμε τη φόρμα ως controlled component:

```javascript{4,10-12,21,24}
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/VmmPgp?editors=0010)

Καθώς το `value` attribute έχει οριστεί στο form element, η εμφανιζόμενη τιμή θα είναι πάντα το `this.state.value`, καθιστώντας το React state ως την πηγή της αλήθειας. Καθώς το `handleChange` τρέχει σε κάθε πάτημα κάθε πλήκτρου, έτσι ώστε να ενημερωθεί το React state, η εμφανιζόμενη τιμή θα ενημερώνεται όσο ο χρήστης πληκτρολογεί.

<<<<<<< HEAD
Με ένα controlled component, κάθε μεταβολή του state θα συνδέεται και με μία function διαχείρισης του. Με αυτό τον τρόπο είναι πολύ απλό να αλλαχτεί ή να ελέγχθει ότι έχει εισάγει ο χρήστης στη φόρμα. Για παράδειγμα, εάν θέλαμε όλα τα ονόματα της φόρμας να είναι σε κεφαλαία μορφή, θα γράφαμε την `handleChange` ως εξής:

```javascript{2}
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```
=======
With a controlled component, the input's value is always driven by the React state. While this means you have to type a bit more code, you can now pass the value to other UI elements too, or reset it from other event handlers.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

## The textarea Tag {#the-textarea-tag}

Σε HTML, ένα `<textarea>` element ορίζει το περιεχόμενο του από τα παιδιά του:

```html
<textarea>
  Hello there, this is some text in a text area
</textarea>
```

Στο React, αντ' αυτού, ένα `<textarea>` χρησιμοποιεί ένα `value` attribute. Με αυτόν τον τρόπο μία φόρμα χρησιμοποιώντας ένα `<textarea>` μπορεί να γραφεί με παρόμοιο τρόπο με μία φόρμα που χρησιμοποιεί ένα single-line input:

```javascript{4-6,12-14,26}
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Essay:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

Παρατηρείστε ότι το `this.state.value` αρχικοποιείται σε επίπεδο constructor, έτσι ώστε το text area να έχει εξ' αρχής κάποια τιμή.

## Το select Tag {#the-select-tag}

Σε  HTML, το `<select>` δημιουργεί ένα drop-down list. Για παράδειγμα, το επόμενο κομμάτι HTML δημιουργεί ένα drop-down list με γεύσεις:

```html
<select>
  <option value="grapefruit">Grapefruit</option>
  <option value="lime">Lime</option>
  <option selected value="coconut">Coconut</option>
  <option value="mango">Mango</option>
</select>
```

Λάβετε υπόψη ότι η επιλογή Coconut είναι εξ' αρχής επιλεγμένη, λόγω του `selected` attribute. Το React, αντί να χρησιμοποιεί το `selected` attribute, χρησιμοποιεί ένα `value` attribute στο `select` tag. Αυτό είναι πιο βολικό σε ένα controlled component, επειδή χρειάζεται να το ενημερώσετε μόνο σε ένα σημείο. Για παράδειγμα:

```javascript{4,10-12,24}
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/JbbEzX?editors=0010)

Γενικά, τα `<input type="text">`, `<textarea>`, και`<select>` δουλεύουν με τον ίδιο τρόπο - δέχονται ένα `value` attribute που μπορείς να χρησιμοποιήσεις για να υλοποιήσεις ένα controlled component.

> Σημείωση
>
> Μπορείς να περάσεις έναν πίνακα στο `value` attribute, επιτρέποντας σου να κάνεις πολλαπλές επιλογές μέσα στο `select` tag:
>
>```js
><select multiple={true} value={['B', 'C']}>
>```

## Το file input Tag {#the-file-input-tag}

Σε HTML, ένα `<input type="file">` επιτρέπει στο χρήστη να επιλέξει ένα ή περισσότερα αρχεία από το χώρο αποθήκευσης της συσκευής του για μεταφορτωθούν σε ένα διακομιστή ή να γίνει διαχείριση αυτών από τη JavaScript μέσω του  [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications).

```html
<input type="file" />
```

Επειδή η τιμή του είναι μόνο για ανάγνωση (read-only), είναι ένα **uncontrolled** component στο React. Γίνεται εκτενέστερη αναφορά μαζί με τα υπόλοιπα uncontrolled components [παρακάτω στην τεκμηριωση](/docs/uncontrolled-components.html#the-file-input-tag).

## Διαχείριση πολλαπλών Inputs {#handling-multiple-inputs}

Όταν χρειαστεί να διαχειριστείς πολλαπλά controlled `input` elements, μπορείς να προσθέσεις ένα `name` attribute σε κάθε element και να αφήσεις την handler function να διαλέξει τί να κάνει με βάση την τιμή του `event.target.name`.

Για παράδειγμα:

```javascript{15,18,28,37}
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/wgedvV?editors=0010)

Σημειώστε πώς χρησιμοποιήσαμε την σύνταξη της ES6 [computed property name](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names) για να ενημερώσουμε το key του state που αντιστοιχεί στο όνομα του input:

```js{2}
this.setState({
  [name]: value
});
```

Ισοδύναμη σύνταξη σε ES5 :

```js{2}
var partialState = {};
partialState[name] = value;
this.setState(partialState);
```

Επίσης, λόγω του ότι το `setState()` αυτόματα [συγχωνεύει το μερικό state στο τρέχων](/docs/state-and-lifecycle.html#state-updates-are-merged), χρειάζεται μόνο να το καλέσουμε με τα αλλαγμένα μέρη.

## Controlled Input Null Τιμή{#controlled-input-null-value}

<<<<<<< HEAD
Ο καθορισμός της τιμής prop σε ένα [controlled component](/docs/forms.html#controlled-components) εμποδίζει τον χρήστη να αλλάξει το input εκτός αν το επιθυμεί. Εάν έχεις ορίσει ένα `value` αλλά το input είναι ακόμη επεξεργάσιμο, ενδέχεται να ορίσετε τυχαία το `value` σε `undefined` ή `null`.
=======
Specifying the `value` prop on a [controlled component](/docs/forms.html#controlled-components) prevents the user from changing the input unless you desire so. If you've specified a `value` but the input is still editable, you may have accidentally set `value` to `undefined` or `null`.
>>>>>>> 707f22d25f5b343a2e5e063877f1fc97cb1f48a1

Ο ακόλουθος κώδικας το καταδεικνύει αυτό. (Το input είναι κλειδωμένο στην αρχή αλλά γίνεται επεξεργάσιμο μετά από μια μικρή καθυστέρηση.)

```javascript
ReactDOM.createRoot(mountNode).render(<input value="hi" />);

setTimeout(function() {
  ReactDOM.createRoot(mountNode).render(<input value={null} />);
}, 1000);

```

## Εναλλακτικές για τα Controlled Components {#alternatives-to-controlled-components}

Μερικές φορές μπορεί να είναι κουραστική η χρήση των controlled components, επειδή πρέπει να γράψετε ένα event handler για κάθε τρόπο που τα δεδομένα σας μπορούν να αλλάξουν και να διοχετεύσουν το state του input μέσω του React component. Αυτό μπορεί να γίνει ιδιαίτερα ενοχλητικό όταν μετατρέπετε μια προϋπάρχουσα εφαρμογή σε React ή ενσωματώνοντας μια εφαρμογή React με μια μη-React library. Σε αυτές τις περιπτώσεις, ίσως θελήσετε να ελέγξετε τα [uncontrolled components](/docs/uncontrolled-components.html), μία εναλλακτική υλοποίηση των input forms.

## Πλήρως ολοκληρωμένες λύσεις {#fully-fledged-solutions}

Αν ψάχνετε για μια ολοκληρωμένη λύση που να περιλαμβάνει τον έλεγχο, την παρακολούθηση των πεδίων της φόρμας με τα οποία ο χρήστης είχε αλληλεπίδραση και τον χειρισμό της υποβολής μίας φόρμας, η [Formik](https://jaredpalmer.com/formik) είναι μια από τις δημοφιλείς επιλογές. Ωστόσο, βασίζεται στις ίδιες αρχές των controlled components και της διαχείρισης του state - οπότε μην παραλείψετε να τα μάθετε.
