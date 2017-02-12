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

------------------------------

###  83. Rendering the Header

* Create a src/components/common folder
* Create Header - copy from previous project
* Copy in all the components/common from previous project
* In App import Header and

```javascript
<View>
  <Header headerText="Tech Stack" />
</View>
```

* NOTE: One tricky thing on provider is it can only have 1 child component. So that is why we wrap with the View tag.

------------------------------

###  84. Reducer and State Design

* We want to show a list to the user.
* We will have a file to show list of libraries.
* Produce some data (reducers). Reducers put the data in state.
* We need to get data intot the reducer.
* Different types of REDUCERS: (1) State with list of tech libraries, (2) State with one open currently selected library
* Lets make those into 2 REDUCERS: (1) Library Reducer (id, name, description), (2) Selection Reducer (id)


------------------------------

###  85. Library List data

* Library Reducer should return a static list of data to our user
* reducers/index.js
* Create file in reducers folder LibraryReducer.js most basic boilerplate `export default () => [];`
* Wire it up in the reducers/index.js
* Full index.js
```javascript
import { combineReducers } from 'redux';
import LibraryReducer from 'LibraryReducer';

export default combineReducers({
  libraries: LibraryReducer
});

```

* We have one reducer called "LibraryReducer". That is assigned to the key "libraries".
* LibraryReducer returns an empty aray at this point if console.log(store.getState())
* We'll make a separate file to house the list of tech libraries
* Make new file reducers/LibraryList.json
* Add contents from Grider's github

------------------------------

###  86. JSON CopyPaste

------------------------------

###  87. Connect Function

* Now we need to import the list into thr reducer.
* import data at top of file - make sure to stipluate .json (.js is default)
* Now, LibraryReducer shows:
```javascript
import data from './LibaryList.json';

export default () => data;

```
* to view this you use console.log(store.getState()) and it should give back the JSON data
* Create new component src\components\LibraryList.js

```javascript
import React, { Component } from 'react';
class LibraryList extends Component {
  render() {
    return;
  }
}
export default LibraryList;
```

* At this point without redux we could just import the data, however with redux we have to dispatch an action
* We do this with Connect
* Connect is a tool to conect from the component to the redux store.
* Wrap the LibraryList wth connect.
* To do this (1) import the connect helper
* (2) Add the connect()(LibraryList) at bottom
```javascript
import React, { Component } from 'react';
import connect from 'react-redux';
class LibraryList extends Component {
  render() {
    return;
  }
}
export default connect()(LibraryList);
```

* This calls function connect, when it's called it returns a function, call that return with LibraryList

------------------------------

###  88. MapStateToProps with Connect

* Heres how to use connect
* create a function called mapStateToProps
* Purpose of this is to take global state object, take some properties off as props
* Add library list to App.js
* If you return an object in mapStateToProps then it will show up as a prop
```javascript
const mapStateToProps = state => {
  return { libraries: state.libraries };
};
```
* A this point in the debug console you have 2 main ibjects (and some errors): dispatch object and libraries
* The connect helper forges a connection between the React and Redux sides of the application


------------------------------

###  89. A Quick Review and Breather

See image (App-89)

* When our app boots up Redux creates a Store
* When the Store is created it calls the Libraries Reducer one time
* It gets a piece of State called libraries which is an array containing a list of objects
* Each object represents one tech library we want to show up on the screen
* Next, we pass it to the provider as a prop.
* The provider is a React component that aids in the communication between React and Redux
* Next the App componet is rendered which renders the Library List component.
* When the LibraryList component is about to render the Connect boots up.
* Connect then reaches to provider and get current State.
* Connect then passes to mapStateToProps so the prop can be used in the component
* `return { libraries: state.libraries };` the libraries can be renamed to "data" or somehting else but NOT state.libraries



------------------------------

###  90. The Theory of ListView


* The approach we are currently taking is fine for a small amoutn of data.
* However, if we have thousands of lines of data we need a better approach
* Let's pretend we have a longer list, and come up with a new strategy.
* Currently we are mapping over components in a list. see (image)
* That is bad-- doing a new component for each list item - that's a bad idea because lots of memory to render many components not being even screen
* We should reduce our memory usage and create less components upfront
* To address this ListView is a native component that fixes this.
* The ListView figures out which items are visible and only creates components for those. (see image)
* In fact it uses the same visible components just swaps out data

------------------------------

###  91. ListView in Practice

* ListView will go in our LibraryList component
* `import { ListView } from 'react-native';`
* We're going to put the logic of the ListView inside componentWillMount lifecycle method

```javascript
componentWillMount() {
  const ds = new ListView.DataSource({
    rowHasChanged: (r1, r2) => r1 != r2
  });
  this.dataSource = ds.cloneWithRows(this.props.libraries)
}
```
* Now in render we'll make it show up
```javascript
render() {
  return(
    <ListView dataSource={this.dataSource} />
  );
}
```

* Next we have to instruct ListView to render only a single row.
* Add prop renderRow - renderRow

```javascript
renderRow(){

}

render() {
  return(
    <ListView
      dataSource={this.dataSource}
      renderRow={this.renderRow}
    />
  );
}
```

------------------------------

### 92. Rendering a Single Row

* Lets make a new component called ListItem
```javascript
import React, { Component } from 'react';
class ListItem extends Component {
  render() {
    return(
    );
  }
}
export default ListItem;
```

* Import this into LibraryList
* Now in renderRow we put the ListItem component
* Next, we need to tell it to show a particular tech library
* We'll put in an argument called library `renderRow(library){`
* Remember the name of the prop does not have to be library, but we'll use that name for the prop.

```javascript
renderRow(library){
  return <ListItem library={library} />;
}
```

------------------------------

### 93. Styling the List

* this.props.library can be referenced
* We need to get the title and description (fields specified int he JSON file)
* in ListItem `import CardSection from './common';`
* `import { Text } from 'react-native'`
```javascript
render() {
  return(
    <CardSection>
      <Text>{this.props.library.title}</Text>
    </CardSection>
  );
}
```

* commit

* add styles object
* destructure the styles

------------------------------

### 94. Creating the Selection Reducer

* We need to create a new Reducer - Selection Reducer
* Create new SelectionReducer.js in the reducers directory
* `export default () => {};`
* Note: We cannot leave this undefined  
* So to start we can just return null `export default () => { return null; };`
* Wire it up: remember we for every Reducer we create we have to wire it back up in the reducers/index.js
```javascript
export default combineReducers({
  libraries: LibraryReducer,
  selectedLibraryId: SelectionReducer
});
```      

------------------------------

### 95. Introducting Action Creators

* We need to find out how to update the selected ID state

### Component -> Create Action -> Action -> Reducer

* Action creator is called by Component. It's a javascript function that creates an action.
* An action is a plain javascript object with a "type" property
* Think of this as a command or instruction to be passed off to the Reducer
* We can create an action with the ID, in the reducer recorder the ID

* So let's make a new action directory, and in that make a new file called index.js.
* Like the reducer/index list of reducers, it will hold all our actions.
* inside we need the type and payload:

```javascript
export const selectLibrary = () => {
  return (
    type: 'select_library',
    payload: libraryId
  );
};

```

------------------------------

### 96. Calling Action Creators

* Here isthe process for alling an action creator.
* (1): We need to decide where we want to call the action creator from.
* (2): We have to wire this up to Redux.
* (3): Go to file where you want to create it. In that file:
* (4): `import * as actions from '../actions';`
* This gets the actions from our actions/index.js (index.js is default if just stating a folder)
* * as actions is to import all exported functions from that file
* (5) Import connect helper from react-redux `import { connect } from 'react-redux';`
* (6) Setup that connect helper at the bottom of the file. NOTE: first argument is for mapStateToProps, so null if not needed
* `export default connect(null,actions)(ListItem);`
* This tells us that the action will be dispathed to the redux store, and passed as props
* Add console.log statement to check on the props.

commit

* Note: We are mot there, but we still don;t have anything to receive the touch/press event in the CardSection component

------------------------------

### 97. Adding a Touchable

* We need to import a Touchable component
* There are 4 types of Touchable components.
* TouchableWithoutFeedback is what we want
* `import { Text, TouchableWithoutFeedback, View } from 'react-native';`
* Wrap the jsx render in TouchableWithoutFeedback and View
* Now a prop is added for the onPress
```javascript
<TouchableWithoutFeedback
  onPress={() => this.props.selectLibrary(id)} >
```
* Some destructuring
`const { id, title} = this.props.library;`

* Check on this: add console.log to SelectionReducer, also add state, action

```javascript
export default (state, action) => {
  console.log(action);
  return null;
};
```

* Last thing we have to do is have ou Reducer action of a select_library get & return that ID

------------------------------

### 98. Rules of Reducers - BOILERPLATE

* We need to return the ID instead of null in the SelectionReducer

### Reducer Boilerplate

* Boilplate we will always use for a reducer switch (state=null sets a default if there is no state):

```javascript
export default (state = null, action) => {
  switch (action.type){
    case 'select_library':
    return action.payload;
    default:
      return state;
  }
};

```

------------------------------

### 99. Expanding a Row

* import state result into ListItem
* mapStateToProps is what does this.
* selectedLibraryId piece of state comes from reducer that was assigned to it (see reducers/index)
* Code for mapStateToProps in ListItem
```javascript
const mapStateToProps = state => {
  return { selectedLibraryId: state.selectedLibraryId };
};

export default connect(mapStateToProps, actions)(ListItem);
```

* Now we can compare the Id when the component is generated to the selectedLibraryId in state
* If they are equal show more detail
* Use a helper method called renderDescription, this is an important part of the logic:

```javascript
renderDescription() {
  const { library, selectLibraryId } = this.props;
  if (library.id === selectLibraryId) {
    return (
      <Text>{library.description}</Text>
    );
  }
}
```

* Add the renderDescription under the CardSection

------------------------------

### 100. Moving Logic Out of Components

* We need to get some styling done.
* Also we need to move out some of the logic of the renderDescription, so we can check if it's been expanded.
* Let's make a change to mapStateToProps in the ListItem
* Add ownProps `const mapStateToProps = (state, ownProps) => {`
* this ownProps is the same as this.props - so we can add/manipulate any props into the mapStateToProps section
* By changing this section we can decide which props to pass down to the component, and remove prop logic from the component
```javascript
const mapStateToProps = (state, ownProps) => {
  const expanded = state.selectedLibraryId === ownProps.library.id;
  return { expanded };
};
```
* Change renderDescription putting in expanded variable


------------------------------

### 101. Animations

* Wrap Text tag in CardSection
* Style for cleaner expanded text description `<Text style={{flex:1}}>{library.description}</Text>`
* import another react native primitive called LayoutAnimation
* From the Udemy course questions: in index.android.js improt UIManager and add `UIManager.setLayoutAnimationEnabledExperimental(true);
`

### 102. Wrapup

https://www.udemy.com/the-complete-react-native-and-redux-course/learn/v4/t/lecture/5744930






















.
