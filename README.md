# Redux Saga Dynamic Injector

Allows dynamically injecting sagas into the app at runtime.

Typically when we use redux saga all the sagas are injected at the time of creating redux store. However, this does not allow to add new sagas later which can be lazy loaded or added by plugin modules. This module helps to overcome this issue by allowing to inject sagas dynamically anytime whether it is route change or loading of new component.

## Install

### npm

`Redux Saga Injector` can be bundled with the client using an
npm-compatible packaging system such as [webpack](http://webpack.github.io/).

```
npm install redux-saga-injector --save
```

## Usage

### 1. Create store
```javascript
// store.js 
export default function configureStore() {
    const reducers = combineReducers({
        app: appReducer,
    });
    const sagaMiddleware = createSagaMiddleware();
    const store = createStore(reducers, {}, applyMiddleware(sagaMiddleware));
    store.runSaga = sagaMiddleware.run;
    return store;
}
```

### 2. Create sagas
```javascript
// sagas.js 
// export sagas which later can be injected dynamically using injectSagas function
export default[
    getName,
];

function* getName(){
    /* saga code goes here */
}
```

### 3. Inject sagas dynamically
```javascript
// using an ES6 transpiler, like babel
import injectSagas from './index';
import testSagas from './sagas';
// initialize store, we need to pass this store to the injectSagas function
const store = configureStore();

// Just pass the store to the function and inject saga dynamically from any part of the application
const injectSagasAsync = injectSagas(store);
// use this function to inject sagas
injectSagasAsync(testSagas);
```

## License
Released under the [MIT License](http://www.opensource.org/licenses/MIT).
