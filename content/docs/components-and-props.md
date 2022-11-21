---
id: components-and-props
title: Components και Props
permalink: docs/components-and-props.html
redirect_from:
  - "docs/reusable-components.html"
  - "docs/reusable-components-zh-CN.html"
  - "docs/transferring-props.html"
  - "docs/transferring-props-it-IT.html"
  - "docs/transferring-props-ja-JP.html"
  - "docs/transferring-props-ko-KR.html"
  - "docs/transferring-props-zh-CN.html"
  - "tips/props-in-getInitialState-as-anti-pattern.html"
  - "tips/communicate-between-components.html"
prev: rendering-elements.html
next: state-and-lifecycle.html
---

Τα components σας επιτρέπουν να χωρίσετε το UI σε ανεξάρτητα, επαναχρησιμοποιούμενα κομμάτια και να σκεφτείτε κάθε κομμάτι μεμονωμένα. Αυτή η σελίδα παρέχει μια εισαγωγή στην ιδέα των components. Μπορείτε να βρείτε [εδώ μια αναλυτική αναφορά του component API](/docs/react-component.html).

Εννοιολογικά, τα components είναι σαν συναρτήσεις JavaScript. Δέχονται αυθαίρετες εισόδους (που ονομάζονται "props") και επιστρέφουν React elements που περιγράφουν τι πρέπει να εμφανίζεται στην οθόνη.

## Function και Class Components {#function-and-class-components}

Ο πιο απλός τρόπος για τον ορισμό ενός component είναι να γράψετε μια συνάρτηση JavaScript:

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

Αυτή η συνάρτηση είναι ένα έγκυρο React component επειδή δέχεται ένα μόνο "prop" (που σημαίνει properties) object παράμετρο με δεδομένα και επιστρέφει ένα React element. Ονομάζουμε αυτά τα components "function components", επειδή είναι κυριολεκτικά συναρτήσεις JavaScript.

Μπορείτε επίσης να χρησιμοποιήσετε μια [ES6 κλάση](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) για να ορίσετε ένα component:

```js
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Τα παραπάνω δύο components είναι ισοδύναμα από την άποψη του React.

Οι συναρτήσεις και οι κλάσεις έχουν κάποια επιπλέον χαρακτηριστικά που θα συζητήσουμε στις [επόμενες ενότητες](/docs/state-and-lifecycle.html).

## Κάνοντας render ένα Component {#rendering-a-component}

Προηγουμένως, συναντήσαμε μόνο React elements που αντιπροσωπεύουν DOM tags:

```js
const element = <div />;
```

Ωστόσο, τα elements μπορούν επίσης να αντιπροσωπεύουν components που ορίζονται από το χρήστη:

```js
const element = <Welcome name="Sara" />;
```

Όταν το React βλέπει ένα element που αντιπροσωπεύει ένα component καθορισμένο από το χρήστη, μεταβιβάζει τα JSX attributes και τα children σε αυτό το component ως ένα μόνο object. Ονομάζουμε αυτό το αντικείμενο "props".

Για παράδειγμα, αυτός ο κώδικας κάνει render "Hello, Sara" στη σελίδα:

```js{1,6}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));
const element = <Welcome name="Sara" />;
root.render(element);
```

<<<<<<< HEAD
**[Δοκιμάστε το στο CodePen](codepen://components-and-props/rendering-a-component)**
=======
**[Try it on CodePen](https://codepen.io/gaearon/pen/YGYmEG?editors=1010)**
>>>>>>> e50e5634cca3c7cdb92c28666220fe3b61e9aa30

Ας ανακεφαλαιώσουμε τι συμβαίνει σε αυτό το παράδειγμα:

<<<<<<< HEAD
1. Θα καλέσουμε το `ReactDOM.render()`  με το `<Welcome name="Sara" />` element.
2. Το React καλεί το `Welcome` component με το `{name: 'Sara'}` ως τα props.
3. Το `Welcome` component μας επειστρέφει ως αποτέλεσμα ένα `<h1>Hello, Sara</h1>` element.
4. Το React DOM ενημερώνει αποδοτικά το DOM για να ταιριάζει το `<h1>Hello, Sara</h1>`.
=======
1. We call `root.render()` with the `<Welcome name="Sara" />` element.
2. React calls the `Welcome` component with `{name: 'Sara'}` as the props.
3. Our `Welcome` component returns a `<h1>Hello, Sara</h1>` element as the result.
4. React DOM efficiently updates the DOM to match `<h1>Hello, Sara</h1>`.
>>>>>>> e50e5634cca3c7cdb92c28666220fe3b61e9aa30

>**Σημείωση:** Πάντα να ξεκινάτε τα ονόματα των components με κεφαλαίο γράμμα.
>
>Το React μεταχειρίζεται τα components που ξεκινούν με πεζά γράμματα ως DOM tags. Για παράδειγμα, το `<div />` αντιπροσωπεύει ένα HTML div tag, αλλά το `<Welcome />` αντιπροσωπεύει ένα component και απαιτεί το `Welcome` να είναι στο scope.
>
>Για να μάθετε περισσότερα για την αιτιολογία πίσω από αυτή τη σύμβαση, διαβάστε το [JSX Σε Βάθος](/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized).

## Composing Components {#composing-components}

Τα components μπορούν να αναφέρουν άλλα components στην έξοδο τους. Αυτό μας επιτρέπει να χρησιμοποιήσουμε την ίδια αφαίρεση του component για οποιοδήποτε επίπεδο λεπτομέρειας. Ένα κουμπί, μια φόρμα, ένας διάλογος, μια οθόνη: στις εφαρμογές React, όλα αυτά εκφράζονται συνήθως ως components.

Για παράδειγμα, μπορούμε να δημιουργήσουμε ένα `App` component που κάνει render πολλές φορές το `Welcome`:

```js{8-10}
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App() {
  return (
    <div>
      <Welcome name="Sara" />
      <Welcome name="Cahal" />
      <Welcome name="Edite" />
    </div>
  );
}
```

<<<<<<< HEAD
**[Δοκιμάστε το στο CodePen](codepen://components-and-props/composing-components)**
=======
**[Try it on CodePen](https://codepen.io/gaearon/pen/KgQKPr?editors=1010)**
>>>>>>> e50e5634cca3c7cdb92c28666220fe3b61e9aa30

Συνήθως, οι νέες React εφαρμογές έχουν ένα μόνο `App` component στην κορυφή. Ωστόσο, εαν ενσωματώσετε το React σε μια υπάρχουσα εφαρμογή, μπορεί να ξεκινήσετε από τη βάση προς τα πάνω με ένα μικρό component όπως το `Button` και να φτάσετε σταδιακά στην κορυφή της view ιεραρχίας.

## Εξαγωγή των Components {#extracting-components}

Μην φοβάστε να χωρίσετε τα components σε μικρότερα components.

Για παράδειγμα, εξετάστε αυτό το `Comment` component:

```js
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

<<<<<<< HEAD
[Δοκιμάστε το στο CodePen](codepen://components-and-props/extracting-components)
=======
**[Try it on CodePen](https://codepen.io/gaearon/pen/VKQwEo?editors=1010)**
>>>>>>> e50e5634cca3c7cdb92c28666220fe3b61e9aa30

Αποδέχεται το `author` (ένα object), το `text` (ένα string), και το `date` (ένα date) ως props, και περιγράφει ένα σχόλιο σε έναν ιστότοπο social media.

Αυτό το component μπορεί να είναι δύσκολο να αλλάξει λόγω όλων των nesting, και είναι επίσης δύσκολο να επαναχρησιμοποιηθούν μεμονωμένα μέρη του. Ας εξάγουμε μερικά components από αυτό.

Πρώτα, θα εξαγάγουμε το `Avatar`:

```js{3-6}
function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

Το `Avatar` δεν χρειάζεται να γνωρίζει ότι γίνεται rendered μέσα σε ένα `Comment`. Αυτός είναι ο λόγος για τον οποίο δώσαμε στο prop του ένα γενικότερο όνομα: `user` παρά `author`.

Συνιστούμε να ονομάζετε τα props από την οπτική γωνία του component και όχι από το πλαίσιο στο οποίο χρησιμοποιείται.

Τώρα μπορούμε να απλοποιήσουμε λίγο το `Comment`:

```js{5}
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <Avatar user={props.author} />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

Στη συνέχεια, θα εξαγάγουμε ένα `UserInfo` component που κάνει render ένα `Avatar` δίπλα στο όνομα του χρήστη:

```js{3-8}
function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
```

Αυτό μας επιτρέπει να απλοποιήσουμε ακόμα περισσότερο το `Comment`:

```js{4}
function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

<<<<<<< HEAD
[Δοκιμάστε το στο CodePen](codepen://components-and-props/extracting-components-continued)

Η εξαγωγή των components ίσως μοιάζει αρχικά με εργασία, αλλά μια παλέτα επαναχρησιμοποιούμενων components αποδίδει σε μεγαλύτερες εφαρμογές. Ένας καλός κανόνας είναι ότι εαν ένα μέρος του UI σας χρησιμοποιείται αρκετές φορές (`Button`, `Panel`, `Avatar`), ή είναι αρκετά σύνθετο από μόνο του (`App`, `FeedStory`, `Comment`), είναι ένας καλός υποψήφιος να είναι ένα επαναχρησιμοποιήσιμο component.
=======
**[Try it on CodePen](https://codepen.io/gaearon/pen/rrJNJY?editors=1010)**

Extracting components might seem like grunt work at first, but having a palette of reusable components pays off in larger apps. A good rule of thumb is that if a part of your UI is used several times (`Button`, `Panel`, `Avatar`), or is complex enough on its own (`App`, `FeedStory`, `Comment`), it is a good candidate to be extracted to a separate component.
>>>>>>> e50e5634cca3c7cdb92c28666220fe3b61e9aa30

## Τα Props είναι Read-Only {#props-are-read-only}

Είτε δηλώνετε ένα component [ως συνάρτηση είτε ως κλάση](#function-and-class-components), δεν πρέπει ποτέ να τροποποιήσει τα δικά του props. Εξετάστε αυτή τη συνάρτηση `sum ':

```js
function sum(a, b) {
  return a + b;
}
```

Αυτές οι συναρτήσεις ονομάζονται ["pure"](https://en.wikipedia.org/wiki/Pure_function) επειδή δεν επιχειρούν να αλλάξουν τις εισόδους τους και πάντα επιστρέφουν το ίδιο αποτέλεσμα για τις ίδιες εισόδους. 

Αντίθετα, αυτή η συνάρτηση είναι impure επειδή αλλάζει τη δικιά της είσοδο:

```js
function withdraw(account, amount) {
  account.total -= amount;
}
```

Το React είναι αρκετά ευέλικτο αλλά έχει έναν μόνο αυστηρό κανόνα:

**Όλα τα React component πρέπει να ενεργούν σαν pure συναρτήσεις σε σχέση με τα props τους**

Φυσικά, οι UI εφαρμογές είναι δυναμικές και αλλάζουν με την πάροδο του χρόνου. Στο [επόμενο τμήμα](/docs/state-and-lifecycle.html), θα εισαγάγουμε μια νέα έννοια του "state". Το state επιτρέπει στα React components να αλλάζουν την έξοδο τους με την πάροδο του χρόνου, ανταποκρινόμενη στις ενέργειες των χρηστών, τις αποκρίσεις δικτύου και οτιδήποτε άλλο, χωρίς να παραβιάζει αυτόν τον κανόνα.
