---
id: introducing-jsx
title: Εισαγωγή στο JSX
permalink: docs/introducing-jsx.html
prev: hello-world.html
next: rendering-elements.html
---

Εξετάστε τη δήλωση αυτής της μεταβλητής

```js
const element = <h1>Hello, world!</h1>;
```

Αυτό το περίεργο συντακτικό με το tag δεν είναι ούτε string ούτε HTML.

Ονομάζεται JSX και είναι μια επέκταση του συντακτικού της JavaScript. Σας συνιστούμε να το χρησιμοποιείται με το React για να περιγράψετε πως θα πρέπει να μοιάζει το UI. Το JSX μπορεί να σας θυμίζει ένα template language, αλλά έρχεται με την πλήρη ισχύ της JavaScript.

Το JSX παράγει τα React “elements”. Θα διερευνήσουμε τη διαδικασία του rendering τους στο DOM στην [επόμενη ενότητα](/docs/rendering-elements.html). Παρακάτω, θα βρείτε τα βασικά του JSX που είναι απαραίτητα για να ξεκινήσετε.

### Γιατί JSX? {#why-jsx}

Το React θεωρεί το γεγονός ότι η λογική του rendering συνδέεται φυσικά με άλλη λογική του UI: πώς γίνεται ο χειρισμός των events, πώς αλλάζει το state με την πάροδο του χρόνου καθώς και πώς προετοιμάζονται τα δεδομένα για εμφάνιση.

Αντί να διαχωρίζει τις *τεχνολογίες* με το να βάζει το markup και τη λογική σε διαφορετικά αρχεία, το React ακολουθεί την αρχιτεκτονική του [separation of *concerns*](https://en.wikipedia.org/wiki/Separation_of_concerns) έχωντας και τα δυο μέσα σε ελαφρώς συνδεδεμένα units που λέγονται "components". Θα επανέλθουμε στα components σε μια [επόμενη ενότητα](/docs/components-and-props.html), αλλά αν δεν αισθάνεστε άνετα με το να βάζετε το markup στη JS, [αυτή η ομιλία](https://www.youtube.com/watch?v=x7cQ3mrcKaY) μπορεί να σας πείσει για το αντίθετο.

Το React [δεν απαιτεί](/docs/react-without-jsx.html) τη χρήση του JSX, αλλά οι περισσότεροι το βρίσκουν ως οπτικό βοήθημα όταν εργάζονται με το UI μέσα από την JavaScript. Επιτρέπει επίσης στο React να εμφανίζει πιο χρήσιμα μηνύματα λάθους και προειδοποιήσεις.

Τώρα που βγήκαν αυτά από τη μέση, ας ξεκινήσουμε!

### Ενσωματώνοντας εκφράσεις στο JSX {#embedding-expressions-in-jsx}

Στο παρακάτω παράδειγμα, δηλώνουμε μια μεταβλητή που ονομάζεται `name` και στη συνέχεια τη χρησιμοποιούμε στο JSX περικλείοντας τη σε αγκύλες:

```js{1,2}
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;
```

Μπορείτε να βάλετε οποιαδήποτε έγκυρη [έκφραση της JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) μέσα σε αγκύλες στο JSX. Για παράδειγμα οι `2 + 2`, `user.firstName` ή `fotmatName(user)` είναι έγκυρες εκφράσεις της JavaScript.

Στο παρακάτω παράδειγμα, βάζουμε το αποτέλεσμα μιας συνάρτησης της JavaScript, `formatName(user)`, σε ένα `<h1>` element.

```js{12}
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);
```

<<<<<<< HEAD
**[Δοκιμάστε το στο CodePen](codepen://introducing-jsx)**
=======
**[Try it on CodePen](https://codepen.io/gaearon/pen/PGEjdG?editors=1010)**
>>>>>>> c7d858947f832d1ba4e78caebc391fd964ff6de6

Διαχωρίζουμε το JSX σε πολλές γραμμές για ευκολία στην ανάγνωση. Ενώ δεν απαιτείται, όταν το κάνετε αυτό, προτείνουμε να το κλείσετε σε παρενθέσεις για να αποφύγετε τις παγίδες [της αυτόματης εισαγωγής ερωτηματικού](https://stackoverflow.com/q/2846283).

### Το JSX μέσα σε εκφράσεις {#jsx-is-an-expression-too}

Έπειτα από το compilation, οι εκφράσεις του JSX γίνονται JavaScript function calls και κάνουν evaluate σε JavaScript objects.

Αυτό σημαίνει ότι μπορείτε να χρησιμοποιήσετε το JSX μέσα σε `if` statements και `for` loops, να το αναθέσετε σε μεταβλητές, να το δεχτείτε σαν arguments και να το επιστρέψετε από functions.

```js{3,5}
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}
```

### Καθορίζοντας Attributes με το JSX {#specifying-attributes-with-jsx}

Μπορείτε να χρησιμοποιήσετε εισαγωγικά για να καθορίσετε τα string literals ως attributes:

```js
const element = <a href="https://www.reactjs.org"> link </a>;
```

Μπορείτε επίσης να χρησιμοποιήσετε άγκιστρα για να ενσωματώσετε μια έκφραση JavaScript σε ένα argument:

```js
const element = <img src={user.avatarUrl}></img>;
```

Μην βάλετε εισαγωγικά γύρω από τα άγκιστρα όταν θέλετε να ενσωματώσετε μια έκφραση JavaScript σε ένα argument. Θα πρέπει να χρησιμοποιήσετε είτε εισαγωγικά (για strings) είτε άγκιστρα (για εκφράσεις), αλλά όχι και τα δύο στο ίδιο argument.

>**Προσοχή:**
>
>Εφόσον το JSX είναι πιο κοντά στη JavaScript από ό,τι στην HTML, το React DOM χρησιμοποιεί ως σύμβαση `camelCase` για την ονοματολογία ενός property αντί για τα ονόματα των attributes της HTML.
>
>Για παράδειγμα, το `class` γίνεται [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className) στο JSX, και το `tabindex` γίνεται [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex).

### Καθορίζοντας Children με το JSX {#specifying-children-with-jsx}

Αν ένα tag είναι κενό, μπορείτε να το κλείσετε άμεσα με `/>`, όπως στην XML:

```js
const element = <img src={user.avatarUrl} />;
```


Τα JSX tags μπορεί να περιέχουν children

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

### To JSX αποτρέπει τα Injection Attacks {#jsx-prevents-injection-attacks}

Είναι ασφαλές να ενσωματώσετε δεδομένα δοσμένα από τον χρήστη στο JSX:

```js
const title = response.potentiallyMaliciousInput;
// Αυτό είναι ασφαλές:
const element = <h1>{title}</h1>;
```

Από προεπιλογή, το React DOM κάνει [escape](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html) οποιεσδήποτε τιμές είναι ενσωματωμένες στο JSX πριν από τη στιγμή του rendering. Έτσι εξασφαλίζει ότι δεν μπορεί ποτέ να γίνει inject οτιδήποτε δεν είναι γραμμένο αποκλειστικά στην εφαρμογή σας. Τα πάντα μετατρέπονται σε ένα string πριν γίνουν rendered. Αυτό αποτρέπει τις επιθέσεις τύπου [XSS (cross-site-scripting)](https://en.wikipedia.org/wiki/Cross-site_scripting).

### Το JSX αντιπροσοπέυει Objects {#jsx-represents-objects}

Το Babel κανει compile το JSX σε `React.createElement()`.

Αυτά τα δύο παραδείγματα είναι ταυτόσημα:

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

To `React.createElement()` εκτελεί μερικούς ελέγχους για να σας βοηθήσει να γράψετε bug-free κώδικα αλλά στην ουσία φτιάχνει ένα object σαν κι αυτό:

```js
// Σημείωση: αυτή η δομή είναι απλοποιημένη
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

Αυτά τα objects ονομάζονται "React elements". Μπορείτε να τα σκεφτείτε σαν περιγραφές του τι θα θέλατε να δείτε στην οθόνη. Το React διαβάζει αυτά τα objects και τα χρησιμοποιεί για να κατασκευάσει το DOM και να το ενημερώνει.

Θα διευρύνουμε το rendering των React elements στο DOM στην [επόμενη ενότητα](/docs/rendering-elements.html).

>**Tip:**
>
<<<<<<< HEAD
>Προτείνουμε τη χρήση του ["Babel" language definition](https://babeljs.io/docs/editors) για τον editor της επιλογής σας, ώστε ο κώδικας ES6 και JSX να επισημαίνονται σωστά.
=======
>We recommend using the ["Babel" language definition](https://babeljs.io/docs/en/next/editors) for your editor of choice so that both ES6 and JSX code is properly highlighted.
>>>>>>> c7d858947f832d1ba4e78caebc391fd964ff6de6
