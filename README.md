# webpak-react-es6-babel
webpak-react-es-babel
React is mostly used for creating Single Page Applications. But it’s possible to integrate the library into any website by using Webpack and Babel.

- React 16
- ES6 & Babel
- React Router
- Redux

Features

    Simple src/index.jsx and src/index.css (local module css).
    Webpack configuration for development (with hot reloading) and production (with minification).
    CSS module loading, so you can include your css by import styles from './path/to.css';.
    Both js(x) and css hot loaded during development.


- `npm install --save react`
- `sudo npm install --save-dev webpack`
- `sudo npm install webpack-dev-server`
- `npm install --save-dev babel-loader`
- `npm install --save-dev babel-core`
- `npm install --save-dev babel-preset-es2015`
- `npm install --save-dev babel-preset-react`


### CSS / SASS

Seems like webpack automatically packs all CSS into the same JS file too (? weird). This shows a technique on how to 
remove it to allow normal browser caching: 
http://www.jonathan-petitcolas.com/2015/05/15/howto-setup-webpack-on-es6-react-application-with-sass.html

Details from webpack here on how to split it out:
http://webpack.github.io/docs/stylesheets.html#separate-css-bundle

### Code Splitting, Multiple Entry Points

This is excellent. This allows bundles created for runtime loading, like what I hacked together with Grunt at CBC. 

Multiple Entry points are different: https://webpack.github.io/docs/multiple-entry-points.html - they're for generating 
multiple bundles loaded by the page (or wherever), e.g. a "libs.js" that loads all libraries, and an "app.js" for 
loading your app code. 

###  Redux

This is a big piece of the puzzle. 

```
npm install --save redux
npm install --save react-redux
npm install --save-dev redux-devtools
```

#### Actions 

All very familiar pub-sub like stuff.

```javascript
var actionObject = { type: "action name", arbitraryData: 123 }; 
store.dispatch(actionObject);
```


#### Reducers

Think about the state (store, I guess) shape. Quotes from the doc (http://rackt.org/redux/docs/basics/Reducers.html):

- try to keep the data separate from the UI state
- we suggest that you keep your state as normalized as possible, without any nesting
- think of the app’s state as a database

Never include these in a reducer:
- Mutate its arguments
- Perform side effects like API calls and routing transitions;
- Calling non-pure functions, e.g. Date.now() or Math.random().

"**Given the same arguments, it should calculate the next state and return it. No surprises. No side effects. No API 
calls. No mutations. Just a calculation.***"

e.g.
```javascript
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      })
    default:
      return state
  }
}
```