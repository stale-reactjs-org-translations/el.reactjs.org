---
id: add-react-to-a-website
title: Add React to a Website
permalink: docs/add-react-to-a-website.html
redirect_from:
  - "docs/add-react-to-an-existing-app.html"
prev: getting-started.html
next: create-a-new-react-app.html
---

Χρησιμοποιήστε το React σε μικρότερο ή μεγαλύτερο βαθμό, ανάλογα με τις ανάγκες σας.

Το React έχει σχεδιαστεί εξαρχής με γνώμονα την προοδευτική υιοθέτησή του και **μπορείτε να χρησιμοποιήσετε το React σε μικρότερο ή μεγαλύτερο βαθμό, ανάλογα με τις ανάγκες σας**. Ίσως το μόνο που θέλετε είναι να προσθέσετε κάποια "στοιχεία διαδραστικότητας" σε μια υπάρχουσα σελίδα. Τα React components αποτελούν εξαιρετικό τρόπο για να το κάνετε αυτό.

Η πλειονότητα των ιστότοπων δεν είναι -και δεν χρειάζεται να είναι- single-page apps. Με **λίγες σειρές κώδικα και χωρίς εργαλεία δόμησης**, δοκιμάστε το React σε ένα μικρό μέρος του ιστότοπού σας. Στη συνέχεια, μπορείτε είτε να επεκτείνετε σταδιακά την παρουσία του είτε να το διατηρήσετε σε μερικά δυναμικά widgets.

---

- [Προσθήκη του React σε ένα λεπτό](#add-react-in-one-minute)
- [Προαιρετικό: Δοκιμή του React με το JSX](#optional-try-react-with-jsx) (δεν απαιτείται bundler!)

## Προσθήκη του React σε ένα λεπτό {#add-react-in-one-minute}

Σε αυτήν την ενότητα, θα σας δείξουμε πώς να προσθέσετε ένα React component σε μια υπάρχουσα σελίδα HTML. Μπορείτε ακολουθήσετε τα βήματα στον δικό σας ιστότοπο ή να δημιουργήσετε ένα κενό αρχείο HTML για εξάσκηση.

Δεν θα χρειαστούν περίπλοκα εργαλεία ούτε απαιτήσεις εγκατάστασης. **Για να ολοκληρώσετε αυτήν την ενότητα, το μόνο που θα χρειαστείτε είναι μια σύνδεση στο διαδίκτυο και ένα λεπτό από τον χρόνο σας.**

Προαιρετικό: [Κάντε λήψη του πλήρους παραδείγματος (2 KB συμπιεσμένο)](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605/archive/f6c882b6ae18bde42dcf6fdb751aae93495a2275.zip)

### Βήμα 1: Προσθήκη ενός DOM container στην HTML {#step-1-add-a-dom-container-to-the-html}

Καταρχάς, ανοίξτε τη σελίδα HTML που θέλετε να επεξεργαστείτε. Προσθέστε ένα κενό `<div>` tag για να επισημάνετε το σημείο όπου θέλετε να εμφανιστεί κάτι με το React. Για παράδειγμα:

```html{3}
<!-- ... υπάρχουσα HTML ... -->

<div id="like_button_container"></div>

<!-- ... υπάρχουσα HTML ... -->
```

Δώσαμε σε αυτό το `<div>` ένα μοναδικό `id` HTML attribute. Αυτό θα μας δώσει τη δυνατότητα να το εντοπίσουμε αργότερα μέσα από τον κώδικα JavaScript και να εμφανίσουμε ένα React component εντός του.

>Συμβουλή
>
>Μπορείτε να τοποθετήσετε ένα "container" `<div>` σαν κι αυτό **οπουδήποτε** εντός του `<body>` tag. Μπορείτε να έχετε όσα ανεξάρτητα DOM containers χρειάζεστε σε μια σελίδα. Συνήθως είναι κενά. Το React αντικαθιστά όποιο περιεχόμενο υπάρχει ήδη εντός των DOM containers.

### Βήμα 2: Προσθήκη των script tags {#step-2-add-the-script-tags}

Στη συνέχεια, προσθέστε και τα 3 `<script>` tags στη σελίδα HTML ακριβώς πριν από το tag κλεισίματος `</body>`:

```html{5,6,9}
  <!-- ... άλλη HTML ... -->

<<<<<<< HEAD
  <!-- Φόρτωση του React. -->
  <!-- Σημείωση: Όταν κάνετε deploy, αντικαταστήστε το "development.js" με το "production.min.js". -->
  <script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
=======
  <!-- Load React. -->
  <!-- Note: when deploying, replace "development.js" with "production.min.js". -->
  <script src="https://unpkg.com/react@17/umd/react.development.js" crossorigin></script>
  <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js" crossorigin></script>
>>>>>>> 5e9d673c6bc1530c901548c0b51af3ad3f91d594

  <!-- Φόρτωση του React component μας. -->
  <script src="like_button.js"></script>

</body>
```

Τα πρώτα δύο tags φορτώνουν το React. Το τρίτο φορτώνει τον κώδικα του component σας.

### Βήμα 3: Δημιουργία ενός React Component {#step-3-create-a-react-component}

Δημιουργήστε ένα αρχείο με την ονομασία `like_button.js` δίπλα στη σελίδα HTML σας.

Ανοίξτε **[αυτόν τον κώδικα starter](https://gist.github.com/gaearon/0b180827c190fe4fd98b4c7f570ea4a8/raw/b9157ce933c79a4559d2aa9ff3372668cce48de7/LikeButton.js)** και επικολλήστε τον στο αρχείο που δημιουργήσατε.

>Συμβουλή
>
>Αυτός ο κώδικας ορίζει ένα React component που ονομάζεται `LikeButton`. Μην ανησυχείτε αν δεν τον κατανοείτε ακόμη. Θα καλύψουμε τα δομικά στοιχεία του React αργότερα στο [πρακτικό tutorial](/tutorial/tutorial.html) και τον [οδηγό βασικών εννοιών](/docs/hello-world.html). Για την ώρα, ας τον κάνουμε να εμφανιστεί στην οθόνη!

Μετά από **[τον κώδικα starter](https://gist.github.com/gaearon/0b180827c190fe4fd98b4c7f570ea4a8/raw/b9157ce933c79a4559d2aa9ff3372668cce48de7/LikeButton.js)**, προσθέστε δύο σειρές στο κάτω μέρος του `like_button.js`:

```js{3,4}
// ... ο κώδικας starter που επικολλήσατε ...

const domContainer = document.querySelector('#like_button_container');
ReactDOM.render(e(LikeButton), domContainer);
```

<<<<<<< HEAD
Αυτές οι δύο σειρές κώδικα εντοπίζουν το `<div>` που προσθέσαμε στην HTML μας στο 1ο βήμα και, έπειτα, εμφανίζουν εντός του το React component μας με το κουμπί "Μου αρέσει". 
=======
These two lines of code find the `<div>` we added to our HTML in the first step, and then display our "Like" button React component inside of it.
>>>>>>> 5e9d673c6bc1530c901548c0b51af3ad3f91d594

### Αυτό ήταν όλο! {#thats-it}

Δεν υπάρχει 4ο βήμα. **Μόλις προσθέσατε το πρώτο σας React component στον ιστότοπό σας.**

Για περισσότερες συμβουλές σχετικά με την ενσωμάτωση του React, δείτε τις επόμενες ενότητες.

**[Προβολή του πλήρους πηγαίου κώδικα του παραδείγματος](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605)**

**[Λήψη του πλήρους παραδείγματος (2 KB συμπιεσμένο)](https://gist.github.com/gaearon/6668a1f6986742109c00a581ce704605/archive/f6c882b6ae18bde42dcf6fdb751aae93495a2275.zip)**

### Συμβουλή: Επαναχρησιμοποίηση ενός Component {#tip-reuse-a-component}

Είναι πολύ συνηθισμένο να θέλετε να εμφανίζετε React components σε πολλά σημεία στη σελίδα HTML. Ακολουθεί ένα παράδειγμα που εμφανίζει 3 φορές το κουμπί "Μου αρέσει" και περνάει ορισμένα δεδομένα σε αυτό:

[Προβολή του πλήρους πηγαίου κώδικα του παραδείγματος](https://gist.github.com/gaearon/faa67b76a6c47adbab04f739cba7ceda)

[Λήψη του πλήρους παραδείγματος (2 KB συμπιεσμένο)](https://gist.github.com/gaearon/faa67b76a6c47adbab04f739cba7ceda/archive/9d0dd0ee941fea05fd1357502e5aa348abb84c12.zip)

>Σημείωση
>
>Αυτή η στρατηγική είναι ιδιαίτερα χρήσιμη όταν τα τμήματα της σελίδας που βασίζονται στο React είναι απομονωμένα το ένα από το άλλο. Αντιθέτως, εντός του κώδικα του React, είναι ευκολότερο να χρησιμοποιηθεί η [σύνθεση components](/docs/components-and-props.html#composing-components).

### Συμβουλή: Ελαχιστοποίηση (Minify) JavaScript για την παραγωγή {#tip-minify-javascript-for-production}

Πριν από το deploy του ιστότοπού σας για παραγωγή, λάβετε υπόψη ότι η μη ελαχιστοποιημένη JavaScript μπορεί να επιβραδύνει σημαντικά τη σελίδα για τους χρήστες σας.

Εάν έχετε ήδη ελαχιστοποιήσει τα application scripts, **ο ιστότοπός σας θα είναι έτοιμος για παραγωγή**, εφόσον διασφαλίσετε ότι η deployed HTML φορτώνει τις εκδόσεις του React με κατάληξη `production.min.js`:

```js
<script src="https://unpkg.com/react@17/umd/react.production.min.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@17/umd/react-dom.production.min.js" crossorigin></script>
```

Εάν δεν περιλαμβάνετε ένα βήμα ελαχιστοποίησης για τα scripts σας, [εδώ μπορείτε να βρείτε έναν τρόπο για να δημιουργήσετε ένα](https://gist.github.com/gaearon/42a2ffa41b8319948f9be4076286e1f3).

## Προαιρετικό: Δοκιμή του React με το JSX {#optional-try-react-with-jsx}

<<<<<<< HEAD
Στα παραπάνω παραδείγματα, βασιστήκαμε αποκλειστικά σε features που υποστηρίζονται εγγενώς από τα προγράμματα περιήγησης. Αυτός είναι ο λόγος που χρησιμοποιήσαμε ένα JavaScript function call για να πούμε στο React τι να εμφανίσει:
=======
In the examples above, we only relied on features that are natively supported by browsers. This is why we used a JavaScript function call to tell React what to display:
>>>>>>> 5e9d673c6bc1530c901548c0b51af3ad3f91d594

```js
const e = React.createElement;

// Εμφάνιση ενός κουμπιού "Μου αρέσει" <button>
return e(
  'button',
  { onClick: () => this.setState({ liked: true }) },
  'Like'
);
```

Ωστόσο, το React προσφέρει επίσης την επιλογή χρήσης του [JSX](/docs/introducing-jsx.html):

```js
// Εμφάνιση ενός κουμπιού "Μου αρέσει" <button>
return (
  <button onClick={() => this.setState({ liked: true })}>
    Like
  </button>
);
```

Αυτά τα δύο snippets κώδικα είναι ισοδύναμα. Ενώ το **JSX είναι [εξ ολοκλήρου προαιρετικό](/docs/react-without-jsx.html)**, πολλοί το θεωρούν χρήσιμο για τη σύνταξη κώδικα UI - τόσο με το React όσο και με άλλες βιβλιοθήκες.

<<<<<<< HEAD
Μπορείτε να πειραματιστείτε με το JSX χρησιμοποιώντας [αυτόν τον online μετατροπέα](https://babeljs.io/en/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=DwIwrgLhD2B2AEcDCAbAlgYwNYF4DeAFAJTw4B88EAFmgM4B0tAphAMoQCGETBe86WJgBMAXJQBOYJvAC-RGWQBQ8FfAAyaQYuAB6cFDhkgA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=7.4.3).
=======
You can play with JSX using [this online converter](https://babeljs.io/en/repl#?babili=false&browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=DwIwrgLhD2B2AEcDCAbAlgYwNYF4DeAFAJTw4B88EAFmgM4B0tAphAMoQCGETBe86WJgBMAXJQBOYJvAC-RGWQBQ8FfAAyaQYuAB6cFDhkgA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015%2Creact%2Cstage-2&prettier=false&targets=&version=7.15.7).
>>>>>>> 5e9d673c6bc1530c901548c0b51af3ad3f91d594

### Γρήγορη δοκιμή του JSX {#quickly-try-jsx}

Ο γρηγορότερος τρόπος να δοκιμάσετε το JSX στο project σας είναι προσθέτοντας αυτό το `<script>` tag στη σελίδα σας:

```html
<script src="https://unpkg.com/babel-standalone@6/babel.min.js"></script>
```

<<<<<<< HEAD
Τώρα, μπορείτε να χρησιμοποιήσετε το JSX σε οποιοδήποτε `<script>` tag προσθέτοντας το `type="text/babel"` attribute σε αυτό. Ακολουθεί [ένα παράδειγμα αρχείου HTML με JSX](https://raw.githubusercontent.com/reactjs/reactjs.org/master/static/html/single-file-example.html), το οποίο μπορείτε να κάνετε λήψη και να πειραματιστείτε μαζί του.
=======
Now you can use JSX in any `<script>` tag by adding `type="text/babel"` attribute to it. Here is [an example HTML file with JSX](https://raw.githubusercontent.com/reactjs/reactjs.org/main/static/html/single-file-example.html) that you can download and play with.
>>>>>>> 5e9d673c6bc1530c901548c0b51af3ad3f91d594

Αυτή η προσέγγιση είναι ιδανική για εκμάθηση και δημιουργία απλών demos. Ωστόσο, καθιστά τον ιστότοπό σας πιο αργό και **δεν είναι κατάλληλη για το στάδιο της παραγωγής**. Όταν είστε έτοιμος να προχωρήσετε, αφαιρέστε αυτό το νέο `<script>` tag και τα `type="text/babel"` attributes που προσθέσατε. Αντ' αυτού, στην επόμενη ενότητα θα ρυθμίσετε ένα JSX preprocessor για την αυτόματη μετατροπή όλων των `<script>` tags σας.

### Προσθήκη του JSX σε ένα project {#add-jsx-to-a-project}

Η προσθήκη του JSX σε ένα project δεν απαιτεί πολύπλοκα εργαλεία, όπως ένα bundler ή ένα development server. Κατ' ουσίαν, η προσθήκη του JSX **μοιάζει πάρα πολύ με την προσθήκη ενός CSS preprocessor.** Η μόνη απαίτηση είναι να υπάρχει εγκατεστημένο το [Node.js](https://nodejs.org/) στον υπολογιστή σας.

Μεταβείτε στον φάκελο του project σας στο τερματικό και επικολλήστε αυτές τις δύο εντολές:

1. **Βήμα 1:** Εκτελέστε: `npm init -y` (εάν αποτύχει, [δείτε έναν τρόπο επιδιόρθωσης](https://gist.github.com/gaearon/246f6380610e262f8a648e3e51cad40d))
2. **Βήμα 2:** Εκτελέστε: `npm install babel-cli@6 babel-preset-react-app@3`

>Συμβουλή
>
>**Εδώ χρησιμοποιούμε το npm μόνο για την εγκατάσταση του JSX preprocessor.** Δεν θα το χρειαστείτε για οτιδήποτε άλλο. Τόσο το React όσο και ο κώδικας της εφαρμογής μπορούν να παραμείνουν ως `<script>` tags χωρίς αλλαγές.

Συγχαρητήρια! Μόλις προσθέσατε στο project σας μια **ρύθμιση του JSX έτοιμη για παραγωγή**.


### Εκτέλεση του JSX Preprocessor {#run-jsx-preprocessor}

Δημιουργήστε έναν φάκελο με την ονομασία `src` και εκτελέστε αυτήν την εντολή τερματικού:

```
npx babel --watch src --out-dir . --presets react-app/prod
```

>Σημείωση
>
>Το `npx` δεν είναι τυπογραφικό λάθος. Πρόκειται για ένα [εργαλείο package runner που παρέχεται με το npm 5.2+](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b).
>
>Εάν εμφανιστεί ένα μήνυμα σφάλματος "You have mistakenly installed the `babel` package" ("Έχετε εγκαταστήσει εσφαλμένα το `babel` package"), ίσως να παραλείψατε [το προηγούμενο βήμα](#add-jsx-to-a-project). Εκτελέστε το στον ίδιο φάκελο και δοκιμάστε πάλι.

Μην περιμένετε να ολοκληρωθεί. Αυτή η εντολή εκκινεί ένα αυτοματοποιημένο watcher για το JSX.

Εάν τώρα δημιουργήσετε ένα αρχείο με την ονομασία `src/like_button.js` με αυτόν τον **[κώδικα JSX starter](https://gist.github.com/gaearon/c8e112dc74ac44aac4f673f2c39d19d1/raw/09b951c86c1bf1116af741fa4664511f2f179f0a/like_button.js)**, το watcher θα δημιουργήσει ένα preprocessed `like_button.js` με κώδικα απλής JavaScript που είναι κατάλληλος για το πρόγραμμα περιήγησης. Όταν επεξεργαστείτε το πηγαίο αρχείο με το JSX, ο μετασχηματισμός θα εκτελεστεί εκ νέου αυτομάτως.

Επιπλέον, αυτό σας επιτρέπει επίσης να χρησιμοποιήσετε σύγχρονα features σύνταξης JavaScript, όπως κλάσεις, χωρίς να ανησυχείτε μήπως διακοπεί η εκτέλεση σε παλαιότερα προγράμματα περιήγησης. Το εργαλείο που μόλις χρησιμοποιήσαμε ονομάζεται Babel και μπορείτε να μάθετε περισσότερα για αυτό με [την τεκμηρίωσή του](https://babeljs.io/docs/en/babel-cli/).

Εάν διαπιστώσετε ότι εξοικειωθήκατε με τα εργαλεία δόμησης και θέλετε να τα βάλετε να κάνουν περισσότερα για εσάς, [η επόμενη ενότητα](/docs/create-a-new-react-app.html) περιγράφει μετρικά από τα δημοφιλέστερα και πιο προσιτά toolchains. Εάν όχι, αυτά τα script tags θα σας εξυπηρετήσουν μια χαρά!
