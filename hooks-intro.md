---
id: hooks-intro
title: Introducing Hooks
permalink: docs/hooks-intro.html
next: hooks-overview.html
---

Τα *Hooks* είναι η νέα προσθήκη στο React 16.8. Σας δίνουν τη δυνατότητα να χρησιμοποιήσετε το state και άλλα features τουReact χωρίς να χρειαστεί να γράψετε κάποιο class.

```js{4,5}
import React, { useState } from 'react';

function Example() {
  // Δηλώστε μια νέα μεταβλητή state, την οποία ονομάζουμε "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Αυτή η νέα συνάρτηση `useState` είναι το πρώτο "Hook" που θα μάθουμε, αλλά αυτό το παράδειγμα έχει ως σκοπό να σας δώσει απλώς μια ιδέα για τη συνέχεια. Μην ανησυχείτε αν δεν το καταλαβαίνετε αυτήν τη στιγμή!

**Μπορείτε να αρχίσετε να μαθαίνετε τα Hooks [στην επόμενη σελίδα](/docs/hooks-overview.html).** Σε αυτήν τη σελίδα, θα εξηγήσουμε γιατί προσθέσαμε τα Hooks στο React και πώς μπορούν να σας βοηθήσουν να γράψετε εξαιρετικές εφαρμογές.

>Σημείωση
>
>Το React 16.8.0 είναι η πρώτη έκδοση που υποστηρίζει τα Hooks. Όταν κάνετε αναβάθμιση, μην παραλείψετε να ενημερώσετε όλα τα packages, συμπεριλαμβανομένου του React DOM.
>Το React Native υποστηρίζει τα Hooks από [την έκδοση 0.59 του React Native](https://facebook.github.io/react-native/blog/2019/03/12/releasing-react-native-059).

## Βίντεο παρουσίασης {#video-introduction}

Στο React Conf 2018, η Sophie Alpert και ο Dan Abramov παρουσίασαν τα Hooks και ακολούθησε ο Ryan Florence που έδειξε πώς μπορείτε να κάνετε refactor μια εφαρμογή για να τα χρησιμοποιήσετε. Παρακολουθήστε το βίντεο εδώ:

<br>

<iframe width="650" height="366" src="//www.youtube.com/embed/dpw9EHDh2bM" frameborder="0" allowfullscreen></iframe>

## Όχι αλλαγές που προκαλούν προβλήματα {#no-breaking-changes}

Προτού προχωρήσουμε, λάβετε υπόψη ότι τα Hooks:

* **Είναι εντελώς προαιρετικά.** Μπορείτε να δοκιμάσετε τα Hooks σε λίγα components χωρίς να ξαναγράψετε τον υπάρχοντα κώδικα. Σε κάθε περίπτωση, δεν χρειάζεται να μάθετε ή να χρησιμοποιήσετε τα Hooks τώρα, αν δεν θέλετε.
* **Έχουν 100% συμβατότητα προς τα πίσω.** Τα Hooks δεν περιέχουν αλλαγές που μπορούν να προκαλέσουν προβλήματα.
* **Είναι διαθέσιμα τώρα.** Τα Hooks είναι διαθέσιμα τώρα με την έκδοση v16.8.0.

**Δεν υπάρχουν σχέδια για την κατάργηση των classes από το React.** Μπορείτε να διαβάσετε περισσότερα σχετικά με τη στρατηγική προδευτικής υιοθέτησης για τα Hooks στην [ενότητα στη βάση](#gradual-adoption-strategy) της παρούσας σελίδας.

**Τα Hooks δεν υποκαθιστούν την εκ μέρους σας γνώση των εννοιών του React.** Αντ' αυτού, τα Hooks παρέχουν ένα πιο άμεσο API στις έννοιες του React που γνωρίζετε ήδη: props, state, context, refs και lifecycle. Όπως θα δείξουμε αργότερα, τα προσφέρουν επίσης έναν πιο δυνατό τρόπο συνδυασμού τους.

**Αν θέλετε απλώς να ξεκινήσετε να μαθαίνετε τα Hooks, μπορείτε [να μεταπηδήσετε απευθείας στην επόμενη σελίδα!](/docs/hooks-overview.html)** Μπορείτε, επίσης, να συνεχίσετε την ανάγνωση αυτής της σελίδας για να μάθετε περισσότερα σχετικά με το γιατί προσθέσαμε τα Hooks και πώς μπορείτε να αρχίσετε να τα χρησιμοποιείτε χωρίς να χρειαστεί να ξαναγράψετε τις εφαρμογές σας.

## Κίνητρο {#motivation}

Τα Hooks επιλύουν ευρεία γκάμα φαινομενικά ασύνδετων προβλημάτων στο React, τα οποία έχουμε αντιμετωπίσει στη διάρκεια των 5 ετών που γράφουμε και συντηρούμε δεκάδες χιλιάδες components. Ανεξάρτητα από το αν μαθαίνετε το React, αν το χρησιμοποιείτε σε καθημερινή βάση ή αν προτιμάτε ένα άλλο library με παρόμοιο μοντέλο components, ενδέχεται να αναγνωρίσετε μερικά από αυτά τα προβλήματα.

### Είναι δύσκολη η επαναχρησιμοποίηση stateful λογικής μεταξύ components {#its-hard-to-reuse-stateful-logic-between-components}

Το React δεν προσφέρει έναν τρόπο σύνδεσης μιας επαναχρησιμοποιούμενης συμπεριφοράς σε ένα component (για παράδειγμα, η σύνδεσή του με ένα store). Εάν έχετε εργαστεί με το React για κάποιο χρόνο, μπορεί να έχετε ήδη γνωρίσει μοτίβα όπως τα [render props](/docs/render-props.html) και τα [higher-order components](/docs/higher-order-components.html) που επιχειρούν να λύσουν αυτό το πρόβλημα. Όμως, αυτά τα μοτίβα απαιτούν από εσάς να αναδομήσετε τα components σας όταν τα χρησιμοποιείτε, κάτι που μπορεί να είναι περίπλοκο και να καταστήσει δυσκολότερη την κατανόηση του κώδικα. Εάν δείτε μια τυπική εφαρμογή με React στο React DevTools, το πιο πιθανό είναι ότι θα βρείτε μια «κόλαση από wrappers» των components, τα οποία περιβάλλονται από στρώματα παρόχων, καταναλωτών, higher-order components, render props και άλλες αφηρημένες έννοιες. Παρόλο που μπορούμε [να τα φιλτράρουμε στο DevTools](https://github.com/facebook/react-devtools/pull/503), αναδεικνύεται ένα βαθύτερο υποκείμενο πρόβλημα: το React χρειάζεται μια καλύτερη βάση για τον διαμοιρασμό της stateful λογικής.

Με τα Hooks, μπορείτε να εξαγάγετε τη stateful λογική από ένα component, ούτως ώστε να είναι δυνατό να τεσταριστεί ανεξάρτητα και να επαναχρησιμοποιηθεί. **Τα Hooks σάς επιτρέπουν να επαναχρησιμοποιήσετε τη stateful λογική χωρίς να αλλάξετε την ιεραρχία των component σας.** Αυτό καθιστά εύκολο τον διαμοιρασμό των Hooks μεταξύ πολλών components ή με την κοινότητα.

Θα συζητήσουμε περαιτέρω αυτό το σημείο στην ενότητα [Δημιουργία των δικών σας Hooks](/docs/hooks-custom.html).

### Τα περίπλοκα components είναι δυσνόητα {#complex-components-become-hard-to-understand}

We've often had to maintain components that started out simple but grew into an unmanageable mess of stateful logic and side effects. Each lifecycle method often contains a mix of unrelated logic. For example, components might perform some data fetching in `componentDidMount` and `componentDidUpdate`. However, the same `componentDidMount` method might also contain some unrelated logic that sets up event listeners, with cleanup performed in `componentWillUnmount`. Mutually related code that changes together gets split apart, but completely unrelated code ends up combined in a single method. This makes it too easy to introduce bugs and inconsistencies.

In many cases it's not possible to break these components into smaller ones because the stateful logic is all over the place. It's also difficult to test them. This is one of the reasons many people prefer to combine React with a separate state management library. However, that often introduces too much abstraction, requires you to jump between different files, and makes reusing components more difficult.

To solve this, **Hooks let you split one component into smaller functions based on what pieces are related (such as setting up a subscription or fetching data)**, rather than forcing a split based on lifecycle methods. You may also opt into managing the component's local state with a reducer to make it more predictable.

We'll discuss this more in [Using the Effect Hook](/docs/hooks-effect.html#tip-use-multiple-effects-to-separate-concerns).

### Classes confuse both people and machines {#classes-confuse-both-people-and-machines}

In addition to making code reuse and code organization more difficult, we've found that classes can be a large barrier to learning React. You have to understand how `this` works in JavaScript, which is very different from how it works in most languages. You have to remember to bind the event handlers. Without unstable [syntax proposals](https://babeljs.io/docs/en/babel-plugin-transform-class-properties/), the code is very verbose. People can understand props, state, and top-down data flow perfectly well but still struggle with classes. The distinction between function and class components in React and when to use each one leads to disagreements even between experienced React developers.

Additionally, React has been out for about five years, and we want to make sure it stays relevant in the next five years. As [Svelte](https://svelte.dev/), [Angular](https://angular.io/), [Glimmer](https://glimmerjs.com/), and others show, [ahead-of-time compilation](https://en.wikipedia.org/wiki/Ahead-of-time_compilation) of components has a lot of future potential. Especially if it's not limited to templates. Recently, we've been experimenting with [component folding](https://github.com/facebook/react/issues/7323) using [Prepack](https://prepack.io/), and we've seen promising early results. However, we found that class components can encourage unintentional patterns that make these optimizations fall back to a slower path. Classes present issues for today's tools, too. For example, classes don't minify very well, and they make hot reloading flaky and unreliable. We want to present an API that makes it more likely for code to stay on the optimizable path.

To solve these problems, **Hooks let you use more of React's features without classes.** Conceptually, React components have always been closer to functions. Hooks embrace functions, but without sacrificing the practical spirit of React. Hooks provide access to imperative escape hatches and don't require you to learn complex functional or reactive programming techniques.

>Examples
>
>[Hooks at a Glance](/docs/hooks-overview.html) is a good place to start learning Hooks.

## Gradual Adoption Strategy {#gradual-adoption-strategy}

>**TLDR: There are no plans to remove classes from React.**

We know that React developers are focused on shipping products and don't have time to look into every new API that's being released. Hooks are very new, and it might be better to wait for more examples and tutorials before considering learning or adopting them.

We also understand that the bar for adding a new primitive to React is extremely high. For curious readers, we have prepared a [detailed RFC](https://github.com/reactjs/rfcs/pull/68) that dives into motivation with more details, and provides extra perspective on the specific design decisions and related prior art.

**Crucially, Hooks work side-by-side with existing code so you can adopt them gradually.** There is no rush to migrate to Hooks. We recommend avoiding any "big rewrites", especially for existing, complex class components. It takes a bit of a mindshift to start "thinking in Hooks". In our experience, it's best to practice using Hooks in new and non-critical components first, and ensure that everybody on your team feels comfortable with them. After you give Hooks a try, please feel free to [send us feedback](https://github.com/facebook/react/issues/new), positive or negative.

We intend for Hooks to cover all existing use cases for classes, but **we will keep supporting class components for the foreseeable future.** At Facebook, we have tens of thousands of components written as classes, and we have absolutely no plans to rewrite them. Instead, we are starting to use Hooks in the new code side by side with classes.

## Frequently Asked Questions {#frequently-asked-questions}

We've prepared a [Hooks FAQ page](/docs/hooks-faq.html) that answers the most common questions about Hooks.

## Next Steps {#next-steps}

By the end of this page, you should have a rough idea of what problems Hooks are solving, but many details are probably unclear. Don't worry! **Let's now go to [the next page](/docs/hooks-overview.html) where we start learning about Hooks by example.**
