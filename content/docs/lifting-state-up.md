---
id: lifting-state-up
title: Μεταφέροντας το State σε ανώτερο επίπεδο (Lifting State Up)
permalink: docs/lifting-state-up.html
prev: forms.html
next: composition-vs-inheritance.html
redirect_from:
  - "docs/flux-overview.html"
  - "docs/flux-todo-list.html"
---

Συχνά, πολλαπλά components πρέπει να μεταβάλλονται ανάλογα με κάποιες κοινές αλλαγές στο state. Για αυτό συνιστούμε τη μεταφορά του state στον πλησιέστερο κοινό πρόγονο τους. Ας δούμε πώς λειτουργεί αυτό στην πράξη.

Σε αυτή την ενότητα, θα δημιουργήσουμε έναν υπολογιστή θερμοκρασίας που υπολογίζει εάν το νερό θα βράσει σε μια δεδομένη θερμοκρασία.

Θα ξεκινήσουμε με ένα component που ονομάζεται `BoilingVerdict`. Αυτό θα δέχεται ως prop την `celsius` θερμοκρασία, και θα τυπώνει εάν αυτή είναι αρκετή ώστε να βράσει το νερό. 

```js{3,5}
function BoilingVerdict(props) {
  if (props.celsius >= 100) {
    return <p>The water would boil.</p>;
  }
  return <p>The water would not boil.</p>;
}
```

Στη συνέχεια, θα δημιουργήσουμε ένα component που ονομάζεται `Calculator`. Θα κάνει render ένα `<input>` το οποίο θα σας δίνει την δυνατότητα να εισάγετε τη θερμοκρασία, και κρατάει την τιμή αυτή στο `this.state.temperature`.

Επιπροσθέτως, αυτό θα κάνει render το `BoilingVerdict` για την τρέχουσα τιμή του input.

```js{5,9,13,17-21}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    return (
      <fieldset>
        <legend>Enter temperature in Celsius:</legend>
        <input
          value={temperature}
          onChange={this.handleChange} />
        <BoilingVerdict
          celsius={parseFloat(temperature)} />
      </fieldset>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/ZXeOBm?editors=0010)

## Προσθέτοντας ένα Δεύτερο Input {#adding-a-second-input}

Η νέα μας απαίτηση είναι ότι, εκτός από το Celsius input, θα έχουμε και ένα Fahrenheit input, και αυτά τα δύο θα πρέπει να διατηρούνται σε συγχρονισμό.

Μπορούμε να ξεκινήσουμε με τη δημιουργία ενός `TemperatureInput` component το οποίο πριν υπήρχε σαν απλό input μέσα στο `Calculator`. Θα προσθέσουμε σε αυτό ένα νέο prop με το όνομα `scale` , η τιμή του οποίου μπορεί να είναι είτε `"c"` είτε `"f"`:

```js{1-4,19,22}
const scaleNames = {
  c: 'Celsius',
  f: 'Fahrenheit'
};

class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

Τώρα μπορούμε να αλλάξουμε το `Calculator`  έτσι ώστε να κάνει render δύο ξεχωριστά inputs θερμοκρασίας:

```js{5,6}
class Calculator extends React.Component {
  render() {
    return (
      <div>
        <TemperatureInput scale="c" />
        <TemperatureInput scale="f" />
      </div>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/jGBryx?editors=0010)

Έχουμε δύο inputs τώρα, αλλά όταν εισάγετε τη θερμοκρασία σε ένα από αυτά, το άλλο δεν ενημερώνεται. Αυτό έρχεται σε αντίθεση με την αρχική μας απαίτηση: θέλουμε να τα διατηρήσουμε σε συγχρονισμό.

Επίσης, δεν είναι δυνατή η εμφάνιση του `BoilingVerdict` από το `Calculator`. Το `Calculator` δεν γνωρίζει την τρέχουσα θερμοκρασία αφού αυτή είναι κρυμμένη μέσα στο `TemperatureInput`.

## Γράφοντας τα Functions μετατροπής {#writing-conversion-functions}

Αρχικά. θα γράψουμε δύο functions τα οποία θα μετατρέπουν τους βαθμούς Κελσίου σε Φαρενάιτ και αντίστροφα.

```js
function toCelsius(fahrenheit) {
  return (fahrenheit - 32) * 5 / 9;
}

function toFahrenheit(celsius) {
  return (celsius * 9 / 5) + 32;
}
```

Αυτά τα δύο functions μετατρέπουν αριθμούς. Θα γράψουμε ένα τρίτο function το οποίο θα δέχεται ως παραμέτρους, το `temperature`, ένα function μετατροπής και θα επιστρέφει ένα string. Θα το χρησιμοποιήσουμε για να υπολογίσουμε την τιμή του ενός input βασιζόμενοι στην τιμή του άλλου input.

Θα επιστρέφει ένα άδειο string στην περίπτωση που το `temperature` δεν είναι έγκυρο, και θα στρογγυλοποιεί το αποτέλεσμα στο τρίτο δεκαδικό ψηφίο:

```js
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature);
  if (Number.isNaN(input)) {
    return '';
  }
  const output = convert(input);
  const rounded = Math.round(output * 1000) / 1000;
  return rounded.toString();
}
```

Για παράδειγμα, το `tryConvert('abc', toCelsius)` επιστρέφει ένα άδειο string, και το `tryConvert('10.22', toFahrenheit)` επιστρέφει `'50.396'`.

## Μεταφέροντας το State σε ανώτερο επίπεδο (Lifting State Up) {#lifting-state-up}

Αυτή τη στιγμή και τα δύο `TemperatureInput` components διατηρούν ανεξάρτητα τις τιμές τους στο τοπικό state:

```js{5,9,13}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {temperature: ''};
  }

  handleChange(e) {
    this.setState({temperature: e.target.value});
  }

  render() {
    const temperature = this.state.temperature;
    // ...  
```

Ωστόσο, θέλουμε τα δύο αυτά inputs να είναι συγχρονισμένα μεταξύ τους. Όταν ενημερώνουμε το Celsius input, το Fahrenheit input πρέπει να δείχνει τη θερμοκρασία αυτή σε βαθμούς Φαρενάιτ και αντίστροφα.

Στο React, ο διαμοιρασμός του state επιτυγχάνεται μεταφέροντας το στον πλησιέστερο κοινό πρόγονο από τα components που το χρειάζονται. Αυτό είναι που ονομάζεται "lifting state up". Θα αφαιρέσουμε το τοπικό state από το `TemperatureInput` και θα το μεταφέρουμε στο `Calculator`.

Εάν το `Calculator` διατηρεί το κοινό state, αυτό είναι που γίνεται η "πηγή της αλήθειας" για την τρέχουσα θερμοκρασία και στα δύο components. Επίσης, αυτό είναι υπεύθυνο να συμβαδίζουν οι τιμές και των δύο components. Εφόσον τα props και των δύο `TemperatureInput` components προέρχονται από τον ίδιο γονέα (parent) `Calculator` component, τα δύο inputs θα είναι πάντα σε συγχρονισμό.

Ας δούμε πώς δουλεύει αυτό βήμα προς βήμα.

Αρχικά, θα αντικαταστήσουμε το `this.state.temperature` με το `this.props.temperature` μέσα στο `TemperatureInput` component. Για τώρα, ας υποθέσουμε ότι το `this.props.temperature` υπάρχει ήδη, παρά το γεγονός ότι στο μέλλον θα χρειαστεί να το περνάμε από το `Calculator`:

```js{3}
  render() {
    // Προηγουμένως: const temperature = this.state.temperature;
    const temperature = this.props.temperature;
    // ...
```

Γνωρίζουμε ότι τα [props είναι μόνο για ανάγνωση (read-only)](/docs/components-and-props.html#props-are-read-only). Όταν το `temperature` ήταν στο τοπικό state, το `TemperatureInput` μπορούσε απλά να καλέσει το `this.setState()` για να αλλάξει την κατάστασή του. Ωστόσο, τώρα που το `temperature` προέρχεται από το γονέα (parent) component σαν prop, το `TemperatureInput` δεν έχει κανένα έλεγχο σε αυτό.

Στο React, αυτό συνήθως επιλύεται μετατρέποντας ένα component σε "controlled". Όπως ακριβώς το DOM `<input>` δέχεται τόσο ένα `value` όσο και ένα `onChange` prop, έτσι και το `TemperatureInput` δέχεται και το `temperature` και το `onTemperatureChange` ως props από τον γονέα (parent) `Calculator`.

Τώρα όταν το `TemperatureInput` θέλει να ανανεώσει τη θερμοκρασία, καλεί το `this.props.onTemperatureChange`:

```js{3}
  handleChange(e) {
    // Προηγουμένως: this.setState({temperature: e.target.value});
    this.props.onTemperatureChange(e.target.value);
    // ...
```

>Σημείωση:
>
>Δεν υπάρχει καμία ιδιαίτερη σημασία στις ονομασίες των props `temperature` και `onTemperatureChange` των components αυτών. Θα μπορούσαμε να ονομάσουμε αυτά τα props με οποιοδήποτε τρόπο, όπως `value` και `onChange`, ονομασίες οι οποίες αποτελούν και μία κοινή σύμβαση.

Το `onTemperatureChange` prop μαζί με το `temperature` prop θα δίνονται απο το γονέα (parent) `Calculator` component. Αυτό θα χειριστεί την αλλαγή τροποποιώντας το δικό του τοπικό state, και έτσι θα προκαλέσει re-render και στα δύο τα inputs με τις νέες τιμές. Θα δούμε παρακάτω την νέα υλοποίηση του `Calculator` component.

Προτού προχωρήσουμε στις αλλαγές του `Calculator`, ας ανακεφαλαιώσουμε τις αλλαγές που κάναμε στο `TemperatureInput` component. Έχουμε αφαιρέσει το τοπικό state από αυτό, και αντί να διαβάζουμε το `this.state.temperature`, τώρα διαβάζουμε το `this.props.temperature`. Αντί να καλούμε το `this.setState()` όταν θέλουμε να κάνουμε μία αλλαγή, καλούμε το `this.props.onTemperatureChange()`, το οποίο παρέχεται από το `Calculator`:

```js{8,12}
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
  }

  handleChange(e) {
    this.props.onTemperatureChange(e.target.value);
  }

  render() {
    const temperature = this.props.temperature;
    const scale = this.props.scale;
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]}:</legend>
        <input value={temperature}
               onChange={this.handleChange} />
      </fieldset>
    );
  }
}
```

Τώρα ας επιστρέψουμε στο `Calculator` component.

Θα αποθηκεύσουμε τις τρέχουσες τιμές των `temperature` και `scale` από το input, στο τοπικό του state. Αυτό είναι το state το οποίο "ανεβάσαμε σε ανώτερο επίπεδο" από αυτό των επιμέρους inputs, και αυτό θα αποτελεί την "πηγή της αλήθειας" και για τα δύο. Επίσης, είναι και η ελάχιστη αναπαράσταση των δεδομένων που χρειάζεται να γνωρίζουμε για να προκαλέσουμε render και στα δύο τα inputs.

Για παράδειγμα, εάν εισάγουμε την τιμή 37 στο Celsius input, το state του `Calculator` component θα είναι:

```js
{
  temperature: '37',
  scale: 'c'
}
```

Εάν αργότερα επεξεργαστούμε το Fahrenheit πεδίο ώστε η τιμή του να είναι 212, το state του  `Calculator` θα είναι:

```js
{
  temperature: '212',
  scale: 'f'
}
```

Θα μπορούσαμε να έχουμε αποθηκεύσει τις τιμές και των δύο inputs αλλά αυτό αποδεικνύεται να είναι μη αναγκαίο. Αρκεί να αποθηκεύσουμε την τιμή του πιο πρόσφατα αλλαγμένου input, καθώς και το scale που αυτό αντιπροσωπεύει. Μπορούμε λοιπόν να συμπεράνουμε την τιμή του άλλου input, μόνο βάσει της τρέχουσας τιμής τόσο του `temperature` όσο και του `scale`.

Τα inputs παραμένουν πάντα συγχρονισμένα καθώς οι τιμές τους υπολογίζονται από το ίδιο state:

```js{6,10,14,18-21,27-28,31-32,34}
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```

[**Try it on CodePen**](https://codepen.io/gaearon/pen/WZpxpz?editors=0010)

Τώρα, ανεξάρτητα από το ποιό input θα επεξεργαστείτε, το `this.state.temperature` και το `this.state.scale` του `Calculator` ανανεώνεται. Ένα από τα inputs παίρνει την τιμή όπως είναι, έτσι ώστε κάθε είσοδος χρήστη να διατηρείται ενώ το άλλο input να υπολογίζεται πάντα με βάση αυτή.

Ας ανακεφαλαιώσουμε τι συμβαίνει όταν επεξεργάζεστε ένα input:

* Το React καλεί ένα function το οποίο έχει καθοριστεί ως το `onChange` πάνω στο DOM `<input>`. Στην δική μας περίπτωση, αυτή είναι η μέθοδος `handleChange` του `TemperatureInput` component.
* Η `handleChange` μέθοδος του `TemperatureInput` component καλεί την `this.props.onTemperatureChange()` μέθοδο με τη νέα επιθυμητή τιμή. Τα props του, συμπεριλαμβανομένης της μεθόδου  `onTemperatureChange`, παρέχονται από το γονέα (parent) component, το `Calculator`.
* Όταν προηγουμένως έγινε render, το`Calculator` έχει ορίσει ότι η `onTemperatureChange` μέθοδος του Celsius input `TemperatureInput` είναι του `Calculator` η `handleCelsiusChange` μέθοδος, και η `onTemperatureChange` του Fahrenheit input `TemperatureInput` είναι του `Calculator` η `handleFahrenheitChange` μέθοδος. Έτσι το ποιά είναι η μέθοδος του `Calculator` η οποία θα καλεστεί, εξαρτάται από το ποιό input θα επεξεργαστούμε. 
* Μέσα σε αυτές τις μεθόδους, το `Calculator` component ζητά από το React να προκαλέσει re-render στον εαυτό του καλώντας την `this.setState()` μέθοδο μαζί με τη νέα τιμή του input value και το τρέχον scale του input το οποίο μόλις επεξεργαστήκαμε.
* Το React καλεί τη `render` μέθοδο του`Calculator` component έτσι ώστε να καθορίσει πως θα φαίνεται το UI. Οι τιμές και των δύο inputs επαναυπολογίζονται βασιζόμενες στην τρέχουσα θερμοκρασία (temperature) και κλίμακα (scale). Η μετατροπή της θερμοκρασίας πραγματοποιείται εδώ.
* Το React καλεί τις `render` μεθόδους των επιμέρους `TemperatureInput` components μαζί με τα νέα props τους, όπως αυτά έχουν οριστεί από το `Calculator`. Ουσιαστικά, μαθαίνει πώς θα πρέπει να είναι το UI.
* Το React καλεί τη `render` μέθοδο του `BoilingVerdict` component, περνώντας τη θερμοκρασία (temperature) σε βαθμούς Κελσίου όπως αυτή είναι στα props του.
* Το React DOM ανανεώνει το DOM του `BoilingVerdict`. Το input το οποίο μόλις επεξεργαστήκαμε λαμβάνει την τρέχουσα τιμή του, ενώ το άλλο input ανανεώνεται με τη τιμή της θερμοκρασίας μετά την μετατροπή αυτής.

Κάθε διαδικασια ανανέωσης, περνάει από τα ίδια βήματα έτσι ώστε τα δύο inputs να παραμένουν συγχρονισμένα.

## Μαθήματα τα οποία μάθαμε {#lessons-learned}

Θα πρέπει να υπάρχει μία και μόνο "πηγή της αλήθειας" για κάθε δεδομένο το οποίο αλλάζει σε μία εφαρμογή React. Συνήθως, το state προστίθεται αρχικά στο component το οποίο χρειάζεται να γίνεται rerender. Στη συνέχεια, εάν και άλλα components το χρειάζονται, μπορείτε να μεταφέρετε το state στον κοντινότερο κοινό πρόγονο. Αντί να προσπαθείτε να κρατάτε σε συγχρονισμό το state μεταξύ δύο διαφορετικών components, θα πρέπει να βασιστείτε στη [ροή δεδομένων από πάνω προς τα κάτω](/docs/state-and-lifecycle.html#the-data-flows-down).

Το να μεταφέρετε το state σε υψηλότερο επίπεδο, συνεπάγεται την εγγραφή περισσότερου κώδικα "boilerplate" από ότι στις two-way binding προσεγγίσεις, αλλά ως όφελος έχει ότι χρειάζεται λιγότερη προσπάθεια για να βρείτε και να απομονώσετε σφάλματα. Δεδομένου ότι κάθε state "ζει" σε κάποιο component και αυτό το component μπορεί να το αλλάξει, ο χώρος για σφάλματα μειώνεται σημαντικά. Επιπλέον, μπορείτε να υλοποιήσετε οποιαδήποτε προσαρμοσμένη λογική για να απορρίψετε ή να μετατρέψετε την είσοδο χρήστη.

Εάν υπάρχει κάτι το οποίο μπορεί να υπολογιστεί είτε από τα props είτε από το state, τότε δεν χρειάζεται να σώζεται στο state. Για παράδειγμα, αντί να σώζεται το `celsiusValue` και το `fahrenheitValue`, αποθηκεύουμε απλά την τελευταία τιμή του `temperature` και του `scale`. Η τιμή του άλλου input μπορεί πάντα να υπολογιστεί από αυτά τα δύο μέσα στη `render()` μέθοδο. Αυτό μας επιτρέπει να καθαρίσουμε ή να στρογγυλοποίησουμε τη τιμή του άλλου πεδίου με ακρίβεια.

<<<<<<< HEAD
Όταν βλέπετε ότι πηγαίνει κάτι λάθος με το UI, μπορείτε να χρησιμοποιείτε τα [React Developer Tools](https://github.com/facebook/react/tree/master/packages/react-devtools) έτσι ώστε να εντοπίζετε τα props και να ανέβετε στο δέντρο των components μέχρις ότου βρείτε το υπεύθυνο component για την ανανέωση του state. Αυτό σας επιτρέπει να εντοπίσετε τα σφάλματα στην πηγή τους:
=======
When you see something wrong in the UI, you can use [React Developer Tools](https://github.com/facebook/react/tree/main/packages/react-devtools) to inspect the props and move up the tree until you find the component responsible for updating the state. This lets you trace the bugs to their source:
>>>>>>> e3073b03a5b9eff4ef12998841b9e56120f37e26

<img src="../images/docs/react-devtools-state.gif" alt="Monitoring State in React DevTools" max-width="100%" height="100%">

