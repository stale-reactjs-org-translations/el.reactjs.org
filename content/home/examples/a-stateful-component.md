---
title: Ένα Stateful Component
order: 1
domid: timer-example
---

Εκτός από τη λήψη δεδομένων εισόδου (πρόσβαση μέσω `this.props`), ένα component μπορεί να διατηρήσει εσωτερικά δεδομένα της κατάστασης (state) του - (πρόσβαση μέσω `this.state`). Όταν μεταβληθούν τα δεδομένα του state ενός component, το rendered markup θα ενημερωθεί με επανάκληση της `render ()`.