# react-native-techstack
Techstack demo from Udemy class on React Native by Stephen Grider

------------------------------

11-77 - Application Setup

* We're going to be using Redux
* Render a list of elements in a performant way
* How to do the animations

* `react-native init techstack`


------------------------------

11-78 - Basic Redux

* We're goin to be doing some basic Redux examples
* Helper demo screen: https://stephengrider.github.io/JSPlaygrounds/
* First example: Turn 'asdf' into an array
* Action: Turn 'asdf' into an array
* Reducer: If the action "type" is "split" take the string and make into an array
* State: ["a","s","d","f"]

------------------------------

11-79 - More on Redux

* JSPlaygrounds has the Redux library built-in: type: Redux and you get back {}
* `const store = Redux.createStore()`
* result `Error: Expected the reducer to be a function.`
* You have to put in at least 1 reducer function or your will get an error
```javascript
const reducer = () => [];
const store = Redux.createStore(reducer);
```
* At any tmie we can ask the store for it's current state
```javascript
const reducer = () => [];
const store = Redux.createStore(reducer);
store.getState();
```
* This will return []
* 






.
