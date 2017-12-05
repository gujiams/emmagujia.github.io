---
title: React Note 1
date: 2017-11-28 18:10:00
<!-- categories: hexo #react -->
tags:
---
### Preparation
#### Install [node.js](https://nodejs.org/en/), [create-react-app](https://github.com/facebookincubator/create-react-app/blob/master/README.md) and switch to [taobao](https://npm.taobao.org/) registry
```sh
npm install -g create-react-app --registry=https://registry.npm.taobao.org
create-react-app my-app
cd my-app/
npm start
```
<!-- more -->
#### Initial project structure
```
my-app
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   └── favicon.ico
│   └── index.html
│   └── manifest.json
└── src
    └── App.css
    └── App.js
    └── App.test.js
    └── index.css
    └── index.js
    └── logo.svg
    └── registerServiceWorker.js
```
#### `npm start` or `yarn start` runs the app in development mode
Open [http://localhost:3000](http://localhost:3000) to view it in the browser. The page will automatically reload if you make changes to the code.
#### `npm test` or `yarn test` runs tests related to files changed since the last commit.
[Read more about testing.](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#running-tests)
#### `npm run build` or `yarn build` bundles React in production mode
Builds the app for production to the `build` folder.
it also [includes a service worker](https://github.com/facebookincubator/create-react-app/blob/master/packages/react-scripts/template/README.md#making-a-progressive-web-app) so that your app loads from local cache on future visits.
### What is react
React is a declarative, efficient, and flexible JavaScript library for building user interfaces.<br>
```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}
// Example usage: <ShoppingList name="Mark" />
```
```javascript
return React.createElement('div', {className: 'shopping-list'},
  React.createElement('h1', /* ... h1 children ... */),
  React.createElement('ul', /* ... ul children ... */)
);
```
Above two blocks of code are the same. <br>
`ShoppingList` is a React component class renders built-in DOM components. we can also  compose custom React components such as <ShoppingList />. Each component is encapsulated so it can operate independently.<br>
A component takes in parameters, called `props`, and returns a hierarchy of views to display via the render method.<br>
See more about createElement() in the [API reference](https://reactjs.org/docs/react-api.html#createelement)
### Tic tac toe demo
#### Copy and paste
[Source Code](https://codepen.io/gaearon/pen/gWWZgR?editors=0010)<br>

#### Some understandings
- `onClick={() => alert('click')}`using [arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) to do alert, if `onClick={alert('click')}` would alert immediately instead of when the button is clicked.
- React components can have state by setting this.state in the constructor, which should be considered private to the component.
- Explicitly call `super();` when defining the constructor of a subclass.
- Best way to aggregate data from multiple children or to have two child components communicate with each other is **move the state upwards** so that it lives in the parent component. The parent can then pass the state back down to the children via props, so that the child components are always in sync with each other and with the parent. The child components called **controlled components**.
- It is conventional in React apps to use `on*` names for the attributes and `handle*` for the handler methods.
- Immutability is important, easier to undo/redo and perform time travel, it can tracking changes and determining when to re-render.<br>
```javascript
//1
var player = {score: 1, name: 'Jeff'};
player.score = 2;
// Now player is {score: 2, name: 'Jeff'}
//2
var player = {score: 1, name: 'Jeff'};
var newPlayer = Object.assign({}, player, {score: 2});
// Now player is unchanged, but newPlayer is {score: 2, name: 'Jeff'}
// Or if you are using object spread syntax proposal, you can write:
// var newPlayer = {...player, score: 2};
```
- **Functional components** syntax can be used for component types that only consist of a render method.
- Assign keys when build dynamic lists. Keys tell react to identify each component, so that can maintain the state across rerenders. Component, will be completely destroyed and recreated with a new state when trying to change key.
#### Add more functions
1. Display the location for each move in the format (col, row) in the move history list.
2. Bold the currently selected item in the move list.
3. Rewrite Board to use two loops to make the squares instead of hardcoding them.
4. Add a toggle button that lets you sort the moves in either ascending or descending order.
5. When someone wins, highlight the three squares that caused the win.