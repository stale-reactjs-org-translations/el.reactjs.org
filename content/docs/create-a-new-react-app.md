---
id: create-a-new-react-app
title: Create a New React App
permalink: docs/create-a-new-react-app.html
redirect_from:
  - "docs/add-react-to-a-new-app.html"
prev: add-react-to-a-website.html
next: cdn-links.html
---

Χρησιμοποιήστε ένα ενσωματωμένο toolchain για την καλύτερη εμπειρία για χρήστες και προγραμματιστές.

Σε αυτήν τη σελίδα, περιγράφονται μερικά δημοφιλή React toolchains που μπορούν να βοηθήσουν σε εργασίες όπως:

* Κλιμάκωση σε πολλά αρχεία και components.
* Χρήση βιβλιοθηκών τρίτων από το npm.
* Εντοπισμός συνηθισμένων σφαλμάτων σε αρχικό στάδιο.
* Ζωντανή επεξεργασία CSS και JS στον προγραμματισμό.
* Βελτιστοποίηση του αποτελέσματος για παραγωγή.

Τα toolchains που συνιστώνται σε αυτήν τη σελίδα **δεν απαιτούν διαμόρφωση για να ξεκινήσετε**.

## Ίσως να μη χρειάζεστε ένα toolchain {#you-might-not-need-a-toolchain}

Εάν δεν αντιμετωπίζετε τα προβλήματα που περιγράφονται παραπάνω ή αν δεν είστε ακόμη εξοικειωμένος με τη χρήση των εργαλείων της JavaScript, εξετάστε το ενδεχόμενο [προσθήκης του React ως απλό `<script>` tag σε μια σελίδα HTML](/docs/add-react-to-a-website.html), προαιρετικά [με το JSX](/docs/add-react-to-a-website.html#optional-try-react-with-jsx).

Αυτός είναι, επίσης, **ο ευκολότερος τρόπος ενσωμάτωσης του React σε έναν υπάρχοντα ιστότοπο.** Μπορείτε πάντα να προσθέσετε ένα μεγαλύτερο toolchain, εφόσον το βρείτε χρήσιμο!

## Συνιστώμενα toolchains {#recommended-toolchains}

Η ομάδα του React συνιστά κατά κύριο λόγο τις ακόλουθες λύσεις:

- Εάν **μαθαίνετε το React** ή **δημιουργείτε μια νέα εφαρμογή [single-page](/docs/glossary.html#single-page-application),** χρησιμοποιήστε το [Create React App](#create-react-app).
- Εάν χρησιμοποιείτε έναν **server-rendered ιστότοπο με Node.js,** δοκιμάστε το [Next.js](#nextjs).
- Εάν δημιουργείτε έναν **στατικό ιστότοπο με προσανατολισμό στο περιεχόμενο,** δοκιμάστε το [Gatsby](#gatsby).
- Εάν δημιουργείτε μια **βιβλιοθήκη component** ή **πραγματοποιείτε ενσωμάτωση με υπάρχον codebase**, δοκιμάστε [πιο ευέλικτα toolchains](#more-flexible-toolchains).

### Create React App (CRA) {#create-react-app}

Το [Create React App](https://github.com/facebookincubator/create-react-app) είναι ένα άνετο περιβάλλον για την **εκμάθηση του React** και αποτελεί τον καλύτερο τρόπο για να ξεκινήσετε τη δημιουργία **μιας νέας εφαρμογής [single-page](/docs/glossary.html#single-page-application)** στο React.

<<<<<<< HEAD
Ρυθμίζει το περιβάλλον προγραμματισμού έτσι ώστε να μπορείτε να χρησιμοποιήσετε τα πιο πρόσφατα features της JavaScript, παρέχει μια θετική εμπειρία για τον προγραμματιστή και βελτιστοποιεί την εφαρμογή σας για παραγωγή. Θα χρειαστεί να έχετε στο μηχάνημά σας τα [Node >= 8.10 και npm >= 5.6](https://nodejs.org/en/). Για τη δημιουργία ενός project, εκτελέστε:
=======
It sets up your development environment so that you can use the latest JavaScript features, provides a nice developer experience, and optimizes your app for production. You’ll need to have [Node >= 14.0.0 and npm >= 5.6](https://nodejs.org/en/) on your machine. To create a project, run:
>>>>>>> 19aa5b4852c3905757edb16dd62f7e7506231210

```bash
npx create-react-app my-app
cd my-app
npm start
```

>Σημείωση
>
>Το `npx` στην πρώτη σειρά δεν είναι ορθογραφικό λάθος. Είναι ένα [package runner tool που συνοδεύει το npm 5.2+](https://medium.com/@maybekatz/introducing-npx-an-npm-package-runner-55f7d4bd282b).

Το Create React App δεν διαχειρίζεται τη λογική backend ούτε βάσεις δεδομένων. Απλώς δημιουργεί ένα frontend build pipeline που μπορείτε να το χρησιμοποιήσετε με όποιο backend θέλετε. Στο παρασκήνιο, χρησιμοποιεί το [Babel](https://babeljs.io/) και το [webpack](https://webpack.js.org/), αλλά δεν χρειάζεται να γνωρίζετε κάτι για αυτά.

Όταν είστε έτοιμος να κάνετε deploy με σκοπό την παραγωγή, η εκτέλεση της εντολής `npm run build` θα δημιουργήσει ένα βελτιστοποιημένο build της εφαρμογής σας στον φάκελο `build`. Μπορείτε να μάθετε περισσότερα σχετικά με το Create React App [από το αρχείο README](https://github.com/facebookincubator/create-react-app#create-react-app--) και τον [οδηγό χρήσης](https://facebook.github.io/create-react-app/) του.

### Next.js {#nextjs}

Το [Next.js](https://nextjs.org/) είναι ένα δημοφιλές και ελαφρύ framework για **στατικές εφαρμογές και εφαρμογές server‑rendered** που έχουν δημιουργηθεί με το React. Περιλαμβάνει άμεσες **λύσεις στυλιστικής διαμόρφωσης και routing** και θεωρεί ως δεδομένο ότι χρησιμοποιείτε το [Node.js](https://nodejs.org/) ως περιβάλλον server.

Για την εκμάθηση του Next.js, ανατρέξτε στον [επίσημο οδηγό του](https://nextjs.org/learn/).

### Gatsby {#gatsby}

Το [Gatsby](https://www.gatsbyjs.org/) είναι ο καλύτερος τρόπος δημιουργίας **στατικών ιστότοπων** με το React. Σας επιτρέπει να χρησιμοποιήσετε React components, αλλά εξάγει pre-rendered HTML και CSS με σκοπό τη διασφάλιση του ταχύτερου χρόνου φόρτωσης.

Για την εκμάθηση του Gatsby, ανατρέξτε στον [επίσημο οδηγό του](https://www.gatsbyjs.org/docs/) και μια [συλλογή από starter kits](https://www.gatsbyjs.org/docs/gatsby-starters/).

### Πιο ευέλικτα toolchains {#more-flexible-toolchains}

Τα παρακάτω toolchains προσφέρουν μεγαλύτερη ευελιξία και περισσότερες επιλογές. Τα προτείνουμε για τους πιο έμπειρους χρήστες:

- Το **[Neutrino](https://neutrinojs.org/)** συνδυάζει τη δύναμη του [webpack](https://webpack.js.org/) με την απλότητα των presets και περιλαμβάνει ένα preset για [εφαρμογές React](https://neutrinojs.org/packages/react/) και [React components](https://neutrinojs.org/packages/react-components/).

<<<<<<< HEAD
- Το **[Parcel](https://parceljs.org/)** είναι ένα γρήγορο bundler διαδικτυακών εφαρμογών που δεν απαιτεί καμία διαμόρφωση και [λειτουργεί με το React](https://parceljs.org/recipes.html#react).
=======
- **[Nx](https://nx.dev/react)** is a toolkit for full-stack monorepo development, with built-in support for React, Next.js, [Express](https://expressjs.com/), and more.

- **[Parcel](https://parceljs.org/)** is a fast, zero configuration web application bundler that [works with React](https://parceljs.org/recipes/react/).
>>>>>>> 19aa5b4852c3905757edb16dd62f7e7506231210

- Το **[Razzle](https://github.com/jaredpalmer/razzle)** είναι ένα server-rendering framework που δεν απαιτεί καμία διαμόρφωση, αλλά προσφέρει μεγαλύτερη ευελιξία από το Next.js.

## Δημιουργία ενός toolchain από μηδενική βάση {#creating-a-toolchain-from-scratch}

Ένα build toolchain της JavaScript συνήθως αποτελείται από:

* Ένα **package manager**, όπως το [Yarn](https://yarnpkg.com/) ή το [npm](https://www.npmjs.com/). Επιτρέπει την αξιοποίηση του διευρυμένου οικοσυστήματος των packages τρίτων και την εύκολη εγκατάσταση ή ενημέρωσή τους.

* Ένα **bundler**, όπως το [webpack](https://webpack.js.org/) ή το [Parcel](https://parceljs.org/). Σας δίνει τη δυνατότητα να γράφετε modular κώδικα κα να τον ομαδοποιείτε σε μικρά packages με σκοπό τη βελτιστοποίηση του χρόνου φόρτωσης.

* Ένα **compiler** όπως το [Babel](https://babeljs.io/). Σας δίνει τη δυνατότητα να γράφετε κώδικα σε σύγχρονη JavaScript, ο οποίος εξακολουθεί να λειτουργεί σε παλαιότερα προγράμματα περιήγησης.

Εάν προτιμάτε να δημιουργήσετε το δικό σας JavaScript toolchain από μηδενική βάση, [ανατρέξτε σε αυτόν τον οδηγό](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658), ο οποίος αναδημιουργεί μερικές από τις λειτουργίες του Create React App.

Μην παραλείψετε να διασφαλίσετε ότι το εξατομικευμένο toolchain σας [έχει ρυθμιστεί σωστά για παραγωγή](/docs/optimizing-performance.html#use-the-production-build).
