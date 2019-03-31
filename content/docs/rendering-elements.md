---
id: rendering-elements
title: Rendering Elements
permalink: docs/rendering-elements.html
redirect_from:
  - "docs/displaying-data.html"
prev: introducing-jsx.html
next: components-and-props.html
---

Elements είναι τα μικρότερα δόκιμα στοιχείων React εφαρμογών.

Ένα element περιγράφει τη θέλεις να δεις στην οθόνη:

```js
const element = <h1>Hello, world</h1>;
```

Διαφορετικά από τα στοιχεία του DOM του προγράμματος περιήγησης, τα React elements είναι απλά αντικείμενα, και φθηνά να τα δημιουγίσεις. Το React DOM φροντίζει να ενημερώσει το DOM να ταιριάζει με τα React elements.

>**Σημείωση**
>
>Ένας μπορεί να μπερδέψει τα elements με μια ποιο ευρύτερη γνωστή έννοια των components. Θα παρουσιάσουμε τα components στο επόμενο [τμήμα](/docs/components-and-props.html). Τα elements είναι αυτό που περιέχονται από components, και σε ενθαρύνουμε να διαβάσεις αυτό το τμήμα προτού πηδήξτε μπροστά.

## Rendering ένα Element στο DOM {#rendering-an-element-into-the-dom}

Ας πούμε ότι υπάρχει ένα `<div>` κάπου στο αρχείο HTML σου:

```html
<div id="root"></div>
```

Αυτό το λέμε το "ριζικό" DOM node επειδή όλα μέσα του διαχειρίζεται από το React DOM.

Οι εφαρμογές χτισμένες με μόνο React συνήθως έχουν ένα μοναδικό ριζικό DOM node. Άμα ενσωματώνεις React σε μια υπάρχουσα εφαρμογή, μπορείς να έχεις όσα πολλά απομονωμένα ριζικά DOM nodes που θέλεις.

Να κανείς render ένα React element σε ένα ριζικό DOM node, να τα περάσεις στο
`ReactDOM.render()`:

`embed:rendering-elements/render-an-element.js`

[Δοκιμάστε το στο CodePen](codepen://rendering-elements/render-an-element)

Εμφανίζεται "Hello, world" στη σελίδα.

## Ενημέρωση του Rendered Element

Ta React elements είναι [αμετάβλητα](https://en.wikipedia.org/wiki/Immutable_object). Όταν δημουργείς ένα element, δεν μπορείς να αλλάξεις τα παιδιά ή τα χαρακτηριστικά του. Ένα element είναι σαν ένα ενιαίο πλαίσιο σε μια ταινία: εκπροσωπεί το UI σε μια συγκεκριμένη χρονική στιγμή.

Με τις γνώσεις μας μέχρι στιγμής, ο μόνος τρόπος να ενημερώσουμε το UI είναι να δημιουρήσουμε ένα καινούργιο element, και να δοθεί στο `ReactDOM.render()`.

Σκεφτείτε αυτό το παράδειγμα με χρονομετρημένο ρολόι:

`embed:rendering-elements/update-rendered-element.js`

[Δοκιμάστε το στο CodePen](codepen://rendering-elements/update-rendered-element)

Αυτό καλεί `ReactDOM.render()` κάθε δευτερόλεπτο από μια [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) επανάκληση.

>**Σημείωση**
>
>Στη πρακτική, οι περισότερες εφαρμογές React καλούν `ReactDOM.render()` μόνο μια φορά. Στα επόμενα τμήματα, θα μάθουμε πώς αυτός ο κώδικας γίνεται εγκλωβισμένος σε [stateful components](/docs/state-and-lifecycle.html).
>
>Σας προτείνουμε να μην παραλείψετε θέματα επειδή βασίζονται ο ένας στον άλλο.

## Το React Μόνο Ενημερώνει τα Απαραίτητα {#react-only-updates-whats-necessary}

Το React DOM συγρίνει το element και τα παιδιά του προηγούμενου, και μόνο εφαρμόζει οι ενημερώσεις απαραίτητες του DOM να φέρει το DOM στην επιθυμητή κατάσταση.

Μπορείτε να επαληθεύσετε ελέγχοντας το [τελευταίο παράδειγμα](codepen://rendering-elements/update-rendered-element) με τα εργαλεία του προγράμματος περιήγησης:

![DOM inspector showing granular updates](../images/docs/granular-dom-updates.gif)

Έστω και δημιουργούμε ένα ελεμεντ περιγραφώντας το ολόκληρο δέντρο tou UI σε κάθε τσιμπούρι, μόνο to text node του οποίου τα περιεχόμενα έχουν αλλάξει ενημερώνονται από το React DOM.

Από την εμπειρία μας, να σκέφτεσαι πως το UI πρέπει να εμφανίζεται σε οποιαδήποτε στιγμή αντί πως να αλλάζεται με την πάροδο του χρόνου εξαλείφει μια ολόκληρη κατηγορία σφαλμάτων.