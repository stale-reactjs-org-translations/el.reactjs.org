---
id: test-utils
title: Test Εργαλεία
permalink: docs/test-utils.html
layout: docs
category: Reference
---

**Importing**

```javascript
import ReactTestUtils from 'react-dom/test-utils'; // ES6
var ReactTestUtils = require('react-dom/test-utils'); // ES5 with npm
```

## Επισκόπηση {#overview}

Το `ReactTestUtils` σας δίνει τη δυνατότητα να κάνετε εύκολα test τα React components χρησιμοποιώντας το testing framework της επιλογής σας. Στο Facebook χρησιμοποιούμε το [Jest](https://facebook.github.io/jest/) για JavaScript testing χωρίς προβλήματα. Μάθετε πως να ξεκινήσετε με το Jest μέσα από το [React Tutorial](https://jestjs.io/docs/tutorial-react) στη σελίδα του Jest.

> Σημείωση:
>
<<<<<<< HEAD
> Συνιστούμε τη χρήση του [`react-testing-library`](https://testing-library.com/react) το οποίο είναι σχεδιασμένο να επιτρέπει και να ενθαρρύνει το γράψιμο tests τα οποία χρησιμοποιούν τα components με τον ίδιο τρόπο που τα χρησιμοποιούν και οι χρήστες.
>
> Εναλλακτικά, το Airbnb έχει ανακοινώσει ενα testing utility, το  [Enzyme](https://airbnb.io/enzyme/), το οποίο διευκολύνει στο assert, manipulate, και traverse του output των React Components.
=======
> We recommend using [React Testing Library](https://testing-library.com/react) which is designed to enable and encourage writing tests that use your components as the end users do.
> 
> For React versions <= 16, the [Enzyme](https://airbnb.io/enzyme/) library makes it easy to assert, manipulate, and traverse your React Components' output.


>>>>>>> 3ff6fe871c6212118991ffafa5503358194489a0

 - [`act()`](#act)
 - [`mockComponent()`](#mockcomponent)
 - [`isElement()`](#iselement)
 - [`isElementOfType()`](#iselementoftype)
 - [`isDOMComponent()`](#isdomcomponent)
 - [`isCompositeComponent()`](#iscompositecomponent)
 - [`isCompositeComponentWithType()`](#iscompositecomponentwithtype)
 - [`findAllInRenderedTree()`](#findallinrenderedtree)
 - [`scryRenderedDOMComponentsWithClass()`](#scryrendereddomcomponentswithclass)
 - [`findRenderedDOMComponentWithClass()`](#findrendereddomcomponentwithclass)
 - [`scryRenderedDOMComponentsWithTag()`](#scryrendereddomcomponentswithtag)
 - [`findRenderedDOMComponentWithTag()`](#findrendereddomcomponentwithtag)
 - [`scryRenderedComponentsWithType()`](#scryrenderedcomponentswithtype)
 - [`findRenderedComponentWithType()`](#findrenderedcomponentwithtype)
 - [`renderIntoDocument()`](#renderintodocument)
 - [`Simulate`](#simulate)

## Παραπομπές {#reference}

### `act()` {#act}

Για να προετοιμάσετε ένα component για assertions, κάντε wrap τον κώδικα που το εμφανίζει και κάνει updates μέσα σε μια κλήση `act()`. Έτσι το test σας εκτελείται παρόμοια με τον τρόπο που το React λειτουργεί στον browser.

>Σημείωση
>
>Αν χρησιμοποιείτε το `react-test-renderer`, αυτό περιέχει μια `act` η οποία συμπεριφέρεται με τον ίδιο τρόπο.

Για παράδειγμα, ας υποθέσουμε πως έχουμε το `Counter` component:

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {count: 0};
    this.handleClick = this.handleClick.bind(this);
  }
  componentDidMount() {
    document.title = `Κάνατε κλικ ${this.state.count} φορές`;
  }
  componentDidUpdate() {
    document.title = `Κάνατε κλικ ${this.state.count} φορές`;
  }
  handleClick() {
    this.setState(state => ({
      count: state.count + 1,
    }));
  }
  render() {
    return (
      <div>
        <p>Κάνατε κλικ {this.state.count} φορές</p>
        <button onClick={this.handleClick}>
          Πάτησε με
        </button>
      </div>
    );
  }
}
```

Δείτε πως μπορούμε να το τεστάρουμε:

```js{3,20-22,29-31}
import React from 'react';
import ReactDOM from 'react-dom/client';
import { act } from 'react-dom/test-utils';
import Counter from './Counter';

let container;

beforeEach(() => {
  container = document.createElement('div');
  document.body.appendChild(container);
});

afterEach(() => {
  document.body.removeChild(container);
  container = null;
});

it('can render and update a counter', () => {
  // Τεστάρει το πρώτο render και το componentDidMount
  act(() => {
    ReactDOM.createRoot(container).render(<Counter />);
  });
  const button = container.querySelector('button');
  const label = container.querySelector('p');
  expect(label.textContent).toBe('You clicked 0 times');
  expect(document.title).toBe('You clicked 0 times');

  // Test-άρει το δεύτερο render και το componentDidUpdate
  act(() => {
    button.dispatchEvent(new MouseEvent('click', {bubbles: true}));
  });
  expect(label.textContent).toBe('Κάνατε κλικ 1 φορά');
  expect(document.title).toBe('Κάνατε κλικ 1 φορά');
});
```

<<<<<<< HEAD
Μη ξεχνάτε πως το dispatching των DOM events λειτουργεί μόνο όταν το DOM container έχει προστεθεί στο `document`. Μπορείτε να χρησιμοποιήσετε μια βοηθητική βιβλιοθήκη, την [`react-testing-library`](https://github.com/kentcdodds/react-testing-library) για να μειώσετε τον boilerplate κώδικα που θα χρειαστεί να γράψετε.
=======
- Don't forget that dispatching DOM events only works when the DOM container is added to the `document`. You can use a library like [React Testing Library](https://testing-library.com/react) to reduce the boilerplate code.

<<<<<<< HEAD
- The [`recipes`](/docs/recipes.html) document contains more details on how `act()` behaves, with examples and usage.
>>>>>>> ddbd064d41d719f9ec0c2f6a4227f797a5828310
=======
- The [`recipes`](/docs/testing-recipes.html) document contains more details on how `act()` behaves, with examples and usage.
>>>>>>> de497e250340ff597ce4964279369f16315b8b4b

* * *

### `mockComponent()` {#mockcomponent}

```javascript
mockComponent(
  componentClass,
  [mockTagName]
)
```

Περάστε ένα mocked component module σε αυτή τη μέθοδο για να της προσθέσετε χρήσιμες μεθόδους που θα της επιτρέψουν να λειτουργήσει σαν ένα dummy React component. Αντί να κάνετε render όπως συνήθως, το component θα εμφανιστεί σαν ένα `<div>` (ή κάποιο άλλο tag εάν του παρέχετε το `mockTagName`) όπου θα περιέχει όλα τα παρεχόμενα children.

> Σημείωση:
>
<<<<<<< HEAD
<<<<<<< HEAD
> Το `mockComponent()` είναι ένα legacy API. Συνιστούμε να χρησιμοποιήσετε το [shallow rendering](/docs/shallow-renderer.html) ή το [`jest.mock()`](https://facebook.github.io/jest/docs/en/tutorial-react-native.html#mock-native-modules-using-jestmock).
=======
> `mockComponent()` is a legacy API. We recommend using [`jest.mock()`](https://facebook.github.io/jest/docs/en/tutorial-react-native.html#mock-native-modules-using-jestmock) instead.
>>>>>>> ddbd064d41d719f9ec0c2f6a4227f797a5828310
=======
> `mockComponent()` is a legacy API. We recommend using [`jest.mock()`](https://jestjs.io/docs/tutorial-react-native#mock-native-modules-using-jestmock) instead.
>>>>>>> 3ff6fe871c6212118991ffafa5503358194489a0

* * *

### `isElement()` {#iselement}

```javascript
isElement(element)
```

Επιστρέφει `true` εάν το `element` είναι κάποιο React element.

* * *

### `isElementOfType()` {#iselementoftype}

```javascript
isElementOfType(
  element,
  componentClass
)
```

Επιστρέφει `true` εάν το `element` είναι ένα React element με τύπο React `componentClass`.

* * *

### `isDOMComponent()` {#isdomcomponent}

```javascript
isDOMComponent(instance)
```

Επιστρέφει `true` εάν το `instance` είναι ένα DOM component (όπως για παράδειγμα `<div>` ή `<span>`).

* * *

### `isCompositeComponent()` {#iscompositecomponent}

```javascript
isCompositeComponent(instance)
```

Επιστρέφει `true` εάν το `instance` είναι ένα component που έχει ορίσει ο χρήστης, όπως ένα class ή function component.

* * *

### `isCompositeComponentWithType()` {#iscompositecomponentwithtype}

```javascript
isCompositeComponentWithType(
  instance,
  componentClass
)
```

Επιστρέφει `true` εάν το `instance` είναι ένα component με τύπο React `componentClass`.

* * *

### `findAllInRenderedTree()` {#findallinrenderedtree}

```javascript
findAllInRenderedTree(
  tree,
  test
)
```

Διασχίζει όλα τα components στο `tree` και συλλέγει όλα τα components όπου το `test(component)` είναι `true`. Από μόνο του αυτό δεν είναι πολύ χρήσιμο, αλλά χρησιμοποιείται σαν βάση για άλλα test εργαλεία.

* * *

### `scryRenderedDOMComponentsWithClass()` {#scryrendereddomcomponentswithclass}

```javascript
scryRenderedDOMComponentsWithClass(
  tree,
  className
)
```

Βρίσκει όλα τα DOM elements των components στο rendered tree τα οποία είναι DOM components με όνομα κλάσης που να ταιριάζει στο `className`.

* * *

### `findRenderedDOMComponentWithClass()` {#findrendereddomcomponentwithclass}

```javascript
findRenderedDOMComponentWithClass(
  tree,
  className
)
```

Σαν το [`scryRenderedDOMComponentsWithClass()`](#scryrendereddomcomponentswithclass) αλλά περιμένει να υπάρχει ένα αποτέλεσμα, το οποίο και επιστρέφει, ή πετάει exception εάν υπάρχει οποιοσδήποτε άλλος αριθμός εκτός του ένα.

* * *

### `scryRenderedDOMComponentsWithTag()` {#scryrendereddomcomponentswithtag}

```javascript
scryRenderedDOMComponentsWithTag(
  tree,
  tagName
)
```

Βρίσκει όλα τα DOM elements των components στο rendered tree τα οποία είναι DOM components με tag name που να ταιριάζει στο `tagName`.

* * *

### `findRenderedDOMComponentWithTag()` {#findrendereddomcomponentwithtag}

```javascript
findRenderedDOMComponentWithTag(
  tree,
  tagName
)
```

Όπως το  [`scryRenderedDOMComponentsWithTag()`](#scryrendereddomcomponentswithtag) αλλά περιμένει να υπάρχει ένα αποτέλεσμα, το οποίο και επιστρέφει, ή πετάει exception εάν υπάρχει οποιοσδήποτε άλλος αριθμός εκτός του ένα.

* * *

### `scryRenderedComponentsWithType()` {#scryrenderedcomponentswithtype}

```javascript
scryRenderedComponentsWithType(
  tree,
  componentClass
)
```

Βρίσκει όλα τα instances των components με τύπο ίδιο με αυτό του `componentClass`.

* * *

### `findRenderedComponentWithType()` {#findrenderedcomponentwithtype}

```javascript
findRenderedComponentWithType(
  tree,
  componentClass
)
```

Όπως το [`scryRenderedComponentsWithType()`](#scryrenderedcomponentswithtype)  αλλά περιμένει να υπάρχει ένα αποτέλεσμα, το οποίο και επιστρέφει, ή πετάει exception εάν υπάρχει οποιοσδήποτε άλλος αριθμός εκτός του ένα.

***

### `renderIntoDocument()` {#renderintodocument}

```javascript
renderIntoDocument(element)
```

Έμφανίζει ένα React element σε ένα detached DOM node στο document. **This function requires a DOM.** Είναι αντίστοιχο με το:

```js
const domContainer = document.createElement('div');
ReactDOM.createRoot(domContainer).render(element);
```

> Σημείωση:
>
> Θα χρειαστείτε το `window`, το `window.document` και το `window.document.createElement` globally διαθέσιμα **πριν** κάνετε import το `React`. Διαφορετικά το React θα πιστέψει πως δεν έχει πρόσβαση στο DOM και μέθοδοι όπως το `setState` δε θα λειτουργούν.

* * *

## Άλλα εργαλεία {#other-utilities}

### `Simulate` {#simulate}

```javascript
Simulate.{eventName}(
  element,
  [eventData]
)
```

Προσομοιώστε ένα event dispatch σε ένα DOM node με προαιρετική επιλογή του `eventData` event data.

Το `Simulate` έχει μία μέθοδο [για κάθε event που μπορεί να καταλάβει το React](/docs/events.html#supported-events).

**Κάντε κλικ σε ένα element**

```javascript
// <button ref={(node) => this.button = node}>...</button>
const node = this.button;
ReactTestUtils.Simulate.click(node);
```

**Αλλάξτε την τιμή ενός input field και ακολούθως πατήστε ENTER.**

```javascript
// <input ref={(node) => this.textInput = node} />
const node = this.textInput;
node.value = 'giraffe';
ReactTestUtils.Simulate.change(node);
ReactTestUtils.Simulate.keyDown(node, {key: "Enter", keyCode: 13, which: 13});
```

> Σημείωση
>
> Θα χρειαστεί να παρέχετε κάθε event property που χρησιμοποιείτε στο component (π.χ. keyCode, which, etc...) καθώς το React δεν τα δημιουργεί για εσάς.

* * *
