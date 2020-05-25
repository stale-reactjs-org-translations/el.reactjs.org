---
id: test-renderer
title: Test Renderer
permalink: docs/test-renderer.html
layout: docs
category: Reference
---

**Importing**

```javascript
import TestRenderer from 'react-test-renderer'; // ES6
const TestRenderer = require('react-test-renderer'); // ES5 with npm
```

## Επισκόπηση {#overview}

Αυτό το πακέτο παρέχει ένα React renderer το οποίο μπορεί να χρησιμοποιηθεί για να κάνει render React components σε "καθαρά" JavaScript αντικείμενα, χωρίς να εξαρτώνται από το DOM ή απο κάποιο native mobile περιβάλλον.
μπορεί να χρησιμοποιηθεί για να γινουν render React components σε "καθαρά" JavaScript αντικείμενα, χωρίς
να εξαρτώνται απο το DOM ή απο κάποιο native mobile περιβάλλον.

Ουσιαστικά, αυτό το πακέτο καθιστά εύκολο να πάρετε ένα snapshot του view hierarchy της πλατφόρμας (όμοιο με ένα DOM tree) όπως αυτό γίνεται render από ένα React DOM ή React Native component χωρίς τη χρήση ενός browser ή του [jsdom](https://github.com/tmpvar/jsdom).

Παράδειγμα:

```javascript
import TestRenderer from 'react-test-renderer';

function Link(props) {
  return <a href={props.page}>{props.children}</a>;
}

const testRenderer = TestRenderer.create(
  <Link page="https://www.facebook.com/">Facebook</Link>
);

console.log(testRenderer.toJSON());
// { type: 'a',
//   props: { href: 'https://www.facebook.com/' },
//   children: [ 'Facebook' ] }
```

Μπορείτε να χρησιμοποιήσετε το snapshot testing του Jest για να σώσετε αυτόματα ένα αντίγραφο του JSON tree σε ένα αρχείο και να ελέγξετε στα τεστς σας πως αυτό δεν έχει αλλάξει: [Μάθετε περισσότερα](https://jestjs.io/docs/en/snapshot-testing).

Ακόμα μπορείτε να κάνετε traverse το output για να βρείτε συγκεκριμένα nodes και να κάνετε assertions για αυτά.
.

```javascript
import TestRenderer from 'react-test-renderer';

function MyComponent() {
  return (
    <div>
      <SubComponent foo="bar" />
      <p className="my">Hello</p>
    </div>
  )
}

function SubComponent() {
  return (
    <p className="sub">Sub</p>
  );
}

const testRenderer = TestRenderer.create(<MyComponent />);
const testInstance = testRenderer.root;

expect(testInstance.findByType(SubComponent).props.foo).toBe('bar');
expect(testInstance.findByProps({className: "sub"}).children).toEqual(['Sub']);
```

### TestRenderer {#testrenderer}

* [`TestRenderer.create()`](#testrenderercreate)
* [`TestRenderer.act()`](#testrendereract)

### TestRenderer instance {#testrenderer-instance}

* [`testRenderer.toJSON()`](#testrenderertojson)
* [`testRenderer.toTree()`](#testrenderertotree)
* [`testRenderer.update()`](#testrendererupdate)
* [`testRenderer.unmount()`](#testrendererunmount)
* [`testRenderer.getInstance()`](#testrenderergetinstance)
* [`testRenderer.root`](#testrendererroot)

### TestInstance {#testinstance}

* [`testInstance.find()`](#testinstancefind)
* [`testInstance.findByType()`](#testinstancefindbytype)
* [`testInstance.findByProps()`](#testinstancefindbyprops)
* [`testInstance.findAll()`](#testinstancefindall)
* [`testInstance.findAllByType()`](#testinstancefindallbytype)
* [`testInstance.findAllByProps()`](#testinstancefindallbyprops)
* [`testInstance.instance`](#testinstanceinstance)
* [`testInstance.type`](#testinstancetype)
* [`testInstance.props`](#testinstanceprops)
* [`testInstance.parent`](#testinstanceparent)
* [`testInstance.children`](#testinstancechildren)

## Παραπομπή {#reference}

### `TestRenderer.create()` {#testrenderercreate}

```javascript
TestRenderer.create(element, options);
```

<<<<<<< HEAD
Δημιούργησε ένα `TestRenderer` instance με το React element παράμετρο. Δε χρησιμοποιεί το πραγματικό DOM, αλλά παρόλαυτα κάνει render το component tree στη μνήμη ώστε να μπορείς να κάνεις assertions για αυτό. Το επιστρεφόμενο instance έχει τις επόμενες μεθόδους και ιδιότητες.

=======
Create a `TestRenderer` instance with the passed React element. It doesn't use the real DOM, but it still fully renders the component tree into memory so you can make assertions about it. Returns a [TestRenderer instance](#testrenderer-instance).
>>>>>>> de497e250340ff597ce4964279369f16315b8b4b

### `TestRenderer.act()` {#testrendereract}

```javascript
TestRenderer.act(callback);
```

Similar to the [`act()` helper from `react-dom/test-utils`](/docs/test-utils.html#act), `TestRenderer.act` prepares a component for assertions. Use this version of `act()` to wrap calls to `TestRenderer.create` and `testRenderer.update`.

```javascript
import {create, act} from 'react-test-renderer';
import App from './app.js'; // The component being tested

// render the component
let root; 
act(() => {
  root = create(<App value={1}/>)
});

// make assertions on root 
expect(root.toJSON()).toMatchSnapshot();

// update with some different props
act(() => {
  root.update(<App value={2}/>);
})

// make assertions on root 
expect(root.toJSON()).toMatchSnapshot();
```

### `testRenderer.toJSON()` {#testrenderertojson}

```javascript
testRenderer.toJSON()
```

Επιστρέφει ενα αντικείμενο που αντιπροσωπευει τo rendered δέντρο. Αυτό το δέντρο περιέχει platform-specific κόμβους όπως το `<div>` και το `<View>` και τα props τους, αλλά δεν περιέχει user-written components. Κάτι το οποίο είναι πολύ χρησιμο για [snapshot testing](https://facebook.github.io/jest/docs/en/snapshot-testing.html#snapshot-testing-with-jest).

### `testRenderer.toTree()` {#testrenderertotree}

```javascript
testRenderer.toTree()
```

<<<<<<< HEAD
Επιστρέφει ενα αντικείμενο που αντιπροσωπευει τo rendered tree. Εν αντιθέσει με το `toJSON()`, αυτή η απεικόνιση είναι πιο λεπτομερής σε σχέση με αυτή του  `toJSON()`, και περιέχει τα user-written components. Πιθανότατα δε χρειάζεστε αυτή την μέθοδο, εκτός κι αν γράφετε τη δικιά σας assertion βιβλιοθήκη κάνοντας χρήση του test renderer.
Πιθανότατα δε χρειάζεστε αυτή την μέθοδο, εκτός και εαν γράφετε τη δικιά σας assertion βιβλιοθήκη κάνοντας χρήση του test renderer.
=======
Return an object representing the rendered tree. The representation is more detailed than the one provided by `toJSON()`, and includes the user-written components. You probably don't need this method unless you're writing your own assertion library on top of the test renderer.
>>>>>>> 941f54180ef5e652221f54ef6d1f6f2e9e063597

### `testRenderer.update()` {#testrendererupdate}

```javascript
testRenderer.update(element)
```

Κάνει re-render το δέντρο που βρίσκεται στη μνήμη με ένα νέο root element. Αυτό προσομοιώνει ένα React update στο root. Αν το νέο element έχει τον ίδιο τυπο και key με το προηγούμενο element, το δέντρο θα γινει update; διαφορετικά, θα γίνει re-mount σε ένα νέο δέντρο.

### `testRenderer.unmount()` {#testrendererunmount}

```javascript
testRenderer.unmount()
```

Κάνει unmount το δέντρο που βρίσκεται στη μνήμη, προκαλώντας τα κατάλληλα lifecycle events.

### `testRenderer.getInstance()` {#testrenderergetinstance}

```javascript
testRenderer.getInstance()
```

Επιστρέφει το instance που αντιστοιχεί στο root element, εαν ειναι διαθέσιμο. Δε θα δουλέψει αν το root element είναι function component επείδη αυτά δεν έχουν instances.

### `testRenderer.root` {#testrendererroot}

```javascript
testRenderer.root
```

Επιστρέφει το root "test instance" αντικείμενο το οποίο είναι χρησιμο για να κάνετε assertions σχετικά με συγκεκριμένα nodes στο δέντρο. Μπορείτε να το χρησιμοποιήσετε για να βρείτε άλλα "test instances" πιο κάτω στην "ιεραρχία".

### `testInstance.find()` {#testinstancefind}

```javascript
testInstance.find(test)
```

Βρίσκει ένα test instance για το οποίο το `test(testInstance)` επιστρέφει `true`. Εάν το `test(testInstance)` δεν επιστρέφει `true` για ακριβώς ένα test instance, θα πετάξει error.

### `testInstance.findByType()` {#testinstancefindbytype}

```javascript
testInstance.findByType(type)
```

Βρίσκει ένα test instance με το παρεχόμενο `type`. Εαν δεν υπάρχει ακριβώς ένα test instance με το παρεχόμενο `type`, θα πετάξει error.

### `testInstance.findByProps()` {#testinstancefindbyprops}

```javascript
testInstance.findByProps(props)
```

Βρίσκει ένα test instance με τα παρεχόμενα `props`. Αν δεν υπάρχει ενα test instance με τα παρεχόμενα `props`, θα πετάξει error.

### `testInstance.findAll()` {#testinstancefindall}

```javascript
testInstance.findAll(test)
```

Βρίσκει όλα τα test instances για τα οποία `test(testInstance)` επιστρέφει `true`.

### `testInstance.findAllByType()` {#testinstancefindallbytype}

```javascript
testInstance.findAllByType(type)
```

Βρίσκει όλα τα test instances με το παρεχόμενο `type`.

### `testInstance.findAllByProps()` {#testinstancefindallbyprops}

```javascript
testInstance.findAllByProps(props)
```

Βρίσκει όλα τα test instances με τα παρεχόμενα `props`.

### `testInstance.instance` {#testinstanceinstance}

```javascript
testInstance.instance
```

Το component instance που αντιστοιχεί σε αυτό το test instance. Είναι διαθέσιμο μόνο για class components, καθώς τα function components δεν έχουν instances. Κάνει match με το `this` value μέσα σε ένα component.

### `testInstance.type` {#testinstancetype}

```javascript
testInstance.type
```

Το component type που αντιστοιχεί σε αυτό το test instance. Για παράδειγμα, ένα `<Button />` component έχει type `Button`.

### `testInstance.props` {#testinstanceprops}

```javascript
testInstance.props
```

Τα props που αντιστοιχούν σε αυτό το test instance. Για παράδειγμα, το `<Button size="small" />` component έχει `{size: 'small'}` σαν props.

### `testInstance.parent` {#testinstanceparent}

```javascript
testInstance.parent
```

Το test instance του πατέρα για αυτό το test instance.

### `testInstance.children` {#testinstancechildren}

```javascript
testInstance.children
```

Τα test instances των παιδιών για αυτό το test instance.

## Ιδέες {#ideas}

Μπορείτε να περάσετε μια `createNodeMock` συνάρτηση στο `TestRenderer.create` σαν μια επιλογή, η οποία επιτρέπει custom mock refs.
Το `createNodeMock` δέχεται το παρόν element και πρέπει να επιστρέψει ένα mock ref αντικείμενο.
Αυτό ειναι χρησιμο όταν θες να τεστάρεις ενα component που βασίζεται σε refs.

```javascript
import TestRenderer from 'react-test-renderer';

class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.input = null;
  }
  componentDidMount() {
    this.input.focus();
  }
  render() {
    return <input type="text" ref={el => this.input = el} />
  }
}

let focused = false;
TestRenderer.create(
  <MyComponent />,
  {
    createNodeMock: (element) => {
      if (element.type === 'input') {
        // mock a focus function
        return {
          focus: () => {
            focused = true;
          }
        };
      }
      return null;
    }
  }
);
expect(focused).toBe(true);
```
