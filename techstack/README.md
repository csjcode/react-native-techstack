# react-native-techstack
Techstack demo from Udemy class on React Native by Stephen Grider

------------------------------

### 11-77 - Application Setup

* We're going to be using Redux
* Render a list of elements in a performant way
* How to do the animations

* `react-native init techstack`


------------------------------

### 11-78 - Basic Redux

* We're goin to be doing some basic Redux examples
* Helper demo screen: https://stephengrider.github.io/JSPlaygrounds/
* First example: Turn 'asdf' into an array
* Action: Turn 'asdf' into an array
* Reducer: If the action "type" is "split" take the string and make into an array
* State: ["a","s","d","f"]

------------------------------

### 11-79 - More on Redux

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

* Next we will create an action

```javascript
const reducer = () => [];
const store = Redux.createStore(reducer);
store.getState();
const action = { type: 'split_string', payload:'asdf' }
```

* Next we add state to the reducer AND store.dispatch(action)

```javascript
const action = { type: 'split_string', payload:'asdf' };
const reducer = (state = [], action) => {
  if (action.type === 'split_string'){
    return action.payload.split('');
  }
  return state;
};

const store = Redux.createStore(reducer);

store.getState();
store.dispatch(action)
store.getState();
```

------------------------------

### 11-80 - Redux is Hard

* Redux is one of the best libraries to scale apps.
* Actions modify the state in one predictable way.
* State will always be an array
* Lets make a new reducer
* Add another character to end of array
* We need to add the previous result (add letter to current state)
* To do that we change the state

```javascript
const reducer = (state = [], action) => {
  if (action.type === 'split_string'){
    return action.payload.split('');
  } else if (action.type === 'add_character'){
    // state.push(action.payload); // original, then removed
    // return state; // original, then removed
    return [ ...state, action.payload ];
  }
  return state;
};

const store = Redux.createStore(reducer);

const action = { type: 'split_string', payload:'asdf' };
const action2 = { type: 'add_character', payload:'a' };

store.getState();
store.dispatch(action);
store.getState();
store.dispatch(action2);
store.getState();
```

** CAREFUL **

* We made a mistake above. We should not mutate state.
* We always return brand new objects from reducers
* To fix this REMOVE:

```javascript
state.push(action.payload);
return state;

```

* To fix this ADD:

```javascript
return [ ...state, action.payload ];
```

------------------------------

### 11-81. Application Boilerplate

* We need to install Redux
* `npm install --save redux react-redux`
* install eslint script and Boilerplate
* make src/app.js

------------------------------

### 11-82. More on Redux Boilerplate

* Add 2 new modules for Redux

```javascript
import { Provider } from 'react-redux';
import { createStore } from 'redux';
```

* redux library is not meant for only react, it's agnostic
* react-redux binds redux to react

* STORE: Add Provider tag and createStore

```javascript
<Provider store={createStore}>
  <View />
</Provider>
```

* See Redux Store image in Research

* REDUCER: Next we're going to create a REDUCER
* Create a folder reducers and file index.js
* We're going to use this index.js file for putting in a bunch of library pointers
* `import { combineReducers } from 'redux'`
* We use this so we can list all the reducers in one file.
* next we add an export statement so the current is:

```javascript
import { combineReducers } from 'redux';

export default combineReducers({
  libraries: () => []
});
```

* In App, import `import reducers from './reducers';`
* `<Provider store={createStore(reducers)}>`

* This the bare minimum for a Redux application.


###  83. Rendering the Header

* Create a src/components/common folder
* Create Header - copy from previous project















.
