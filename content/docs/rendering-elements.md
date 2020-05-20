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

Ένα element περιγράφει τι θέλεις να δεις στην οθόνη:

```js
const element = <h1>Hello, world</h1>;
```

Εν αντιθέσει με τα στοιχεία του DOM του προγράμματος περιήγησης, τα React elements είναι απλά αντικείμενα, και δεν έχουν μεγάλο κόστος για να τα δημιουγήσεις. Το React DOM φροντίζει να ενημερώσει το DOM ώστε να ταιριάζει με τα React elements.

>**Σημείωση**
>
>Ένας μπορεί να μπερδέψει τα elements με μια ευρύτερη γνωστή έννοια, αυτή των components. Θα παρουσιάσουμε τα components στο επόμενο [τμήμα](/docs/components-and-props.html). Τα elements είναι αυτό που περιέχονται από components, και σε ενθαρρύνουμε να διαβάσεις αυτό το τμήμα προτού συνεχίσετε στα επόμενα.

## Rendering ένα Element στο DOM {#rendering-an-element-into-the-dom}

Ας υποθέσουμε πως υπάρχει ένα `<div>` κάπου στο HTML αρχείο σου:

```html
<div id="root"></div>
```

Αυτό το λέμε το "ριζικό" DOM node επειδή όλα μέσα του διαχειρίζεται από το React DOM.

Οι εφαρμογές χτισμένες με μόνο React συνήθως έχουν ένα μοναδικό ριζικό DOM node. Άμα ενσωματώνεις React σε μια υπάρχουσα εφαρμογή, μπορείς να έχεις όσα πολλά απομονωμένα ριζικά DOM nodes που θέλεις.

<<<<<<< HEAD
Να κανείς render ένα React element σε ένα ριζικό DOM node, να τα περάσεις στο
`ReactDOM.render()`:
=======
To render a React element into a root DOM node, pass both to [`ReactDOM.render()`](/docs/react-dom.html#render):
>>>>>>> dea4f329ea3a7bba116e07adf67eb5c8b6c528cd

`embed:rendering-elements/render-an-element.js`

[Δοκιμάστε το στο CodePen](codepen://rendering-elements/render-an-element)

Εμφανίζει "Hello, world" στη σελίδα.

## Ενημέρωση του Rendered Element {#updating-the-rendered-element}

Ta React elements είναι [αμετάβλητα](https://en.wikipedia.org/wiki/Immutable_object). Όταν δημουργείς ένα element, δεν μπορείς να αλλάξεις τα παιδιά ή τα χαρακτηριστικά του. Ένα element είναι σαν ένα ενιαίο πλαίσιο σε μια ταινία: εκπροσωπεί το UI σε μια συγκεκριμένη χρονική στιγμή.

<<<<<<< HEAD
Με τις γνώσεις μας μέχρι στιγμής, ο μόνος τρόπος να ενημερώσουμε το UI είναι να δημιουρήσουμε ένα καινούργιο element, και να δοθεί στο `ReactDOM.render()`.
=======
With our knowledge so far, the only way to update the UI is to create a new element, and pass it to [`ReactDOM.render()`](/docs/react-dom.html#render).
>>>>>>> dea4f329ea3a7bba116e07adf67eb5c8b6c528cd

Σκεφτείτε αυτό το παράδειγμα με χρονομετρημένο ρολόι:

`embed:rendering-elements/update-rendered-element.js`

[Δοκιμάστε το στο CodePen](codepen://rendering-elements/update-rendered-element)

<<<<<<< HEAD
Αυτό καλεί `ReactDOM.render()` κάθε δευτερόλεπτο από μια [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) επανάκληση.
=======
It calls [`ReactDOM.render()`](/docs/react-dom.html#render) every second from a [`setInterval()`](https://developer.mozilla.org/en-US/docs/Web/API/WindowTimers/setInterval) callback.
>>>>>>> dea4f329ea3a7bba116e07adf67eb5c8b6c528cd

>**Σημείωση**
>
<<<<<<< HEAD
>Στη πρακτική, οι περισότερες εφαρμογές React καλούν `ReactDOM.render()` μόνο μια φορά. Στα επόμενα τμήματα, θα μάθουμε πώς αυτός ο κώδικας γίνεται εγκλωβισμένος σε [stateful components](/docs/state-and-lifecycle.html).
=======
>In practice, most React apps only call [`ReactDOM.render()`](/docs/react-dom.html#render) once. In the next sections we will learn how such code gets encapsulated into [stateful components](/docs/state-and-lifecycle.html).
>>>>>>> dea4f329ea3a7bba116e07adf67eb5c8b6c528cd
>
>Σας προτείνουμε να μην παραλείψετε θέματα επειδή βασίζονται ο ένας στον άλλο.

## Το React Μόνο Ενημερώνει τα Απαραίτητα {#react-only-updates-whats-necessary}

Το React DOM συγρίνει το element και τα παιδιά του προηγούμενου, και μόνο εφαρμόζει οι ενημερώσεις απαραίτητες του DOM να φέρει το DOM στην επιθυμητή κατάσταση.

Μπορείτε να επαληθεύσετε ελέγχοντας το [τελευταίο παράδειγμα](codepen://rendering-elements/update-rendered-element) με τα εργαλεία του προγράμματος περιήγησης:

![DOM inspector showing granular updates](../images/docs/granular-dom-updates.gif)

Αν και δημιουργούμε ένα στοιχείο περιγράφοντας ολόκληρο το δέντρο του UI σε κάθε "τίκ", μόνο το text node του οποίου τα περιεχόμενα έχουν αλλάξει ενημερώνονται από το React DOM.

<<<<<<< HEAD
Με βάση την εμπειρία μας, όταν σκέφτεστε πως το UI πρέπει να εμφανίζεται σε μια οποιαδήποτε στιγμή αντί για το πως πρέπει να αλλάζει με την πάροδο του χρόνου καταφέρνει να εξαλείψει μια ολόκληρη κατηγορία σφαλμάτων.
=======
In our experience, thinking about how the UI should look at any given moment, rather than how to change it over time, eliminates a whole class of bugs.
>>>>>>> dea4f329ea3a7bba116e07adf67eb5c8b6c528cd
