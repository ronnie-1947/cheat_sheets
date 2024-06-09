# React

## React JS

### State

React is all about state management. State is a component's and its children's internal data. If state changes, rerender of component and its children (who are using that state) takes place.

### Pure functions vs Impure functions

#### Pure Functions

Pure functions only depend on their input parameters and do not modify the state of the application or have side effects.

Result of Pure function don't change with an outside parameter

```javascript
// This function only depend on the value of r (its parameter)
function pureFunc (r) {

  return 3.14 * r*r
}
```

**React components are pure functions**

#### ImPure Functions

Can have unpredictable behavior and can modify the state of the application or have side effects. Side effects such as initiate a timer or change the state of another function.

```javascript
let pi = 3.14 * new Date().getHours()

// This function depends on the value of pi and Date. The final result can change with time.
function impureFunction (r){
  return pi * r*r
}
```

### How React works

React paints the DOM in 3 phases

* Create Virtual DOM (Render phase)
* Create Fiber tree (Reconciliation)
* Commit Phase (Paints the DOM)

#### Create Virtual DOM

Virtual DOM is a JS object and it is created pretty fast. if the state is changed in some component, all of its child component goes to a modified state.

#### Reconciliation (Diffing)

Fiber tree creation, React compares the current tree with the virtual DOM to create an updated Tree.

* The required props are passed in child components
* Repaint happens in child components
* State is morized if the component is not erased or no change in position of component
* Loss of state if component is deleted

Key props is used to uniquely identify a component.

NOTE: Don't set the key to index of the array. If there is remove of elements in between, the state of the elements get jumbled up

### Events

React introduce us to synthetic events so that most of the event works same across all browsers. And synthetic events bouble except the scroll event.

### component LifeCycle

1. Mount / Initial Render 🐣
2. Re-Render 💫
3. Unmount ☠️

### Hooks

Special built-in functions that allow us to hook into React internals:

1. Creating and accessing state from Fiber tree
2. Registering side effects in Fiber tree
3. Manual DOM selection
4. Etc.

#### Hooks list

```
useState
useEffect
useReducer
useContext
useRef
useCallback
useMemo
useTransition
```

#### Hooks Rules

1. Hooks need to called from the top level of a React component
2. React keep record for order of hooks written

#### useState Hook

```javascript
const [state, setState] = useState(true)

<!--  setState accepts a function as arg where cur is the current state -->
const handleState = ()=>{
  setState(cur=> !cur)
}
```

Key points:

* If useState is called multiple times inside handle func

```javascript
const handleState = ()=>{
  setState(cur=> cur + 1)
  setState(cur=> cur + 1)
  setState(cur=> cur + 1)
  }
```

* Multiple setState are batched into one and rendering takes place only once with all state change. Auto batching is enabled in React 18+
* setState is asynchrnous
* Opt out of automatic batching by wrapping a state update in ReactDOM.flushSync()

useState hook also accepts a function with no args

```javascript
const [movies, setMovies] = useState(()=>{
  const movies = localStorage.getItem('movies')
  return JSON.parse(movies)
})
```

#### useRef() Hook

A hook to store DOM elements.

```javascript
const inputEl = useRef(null)

useEffect(()=>{
  console.log(inputEl.current) // Shows the input element
}, [])


return <input ref={inputEL}/>
```

useRef hook don't result in component render when the value is updated.

#### useReducer Hook

It is a more advance way to manage state. Needs a reducer function which is a pure function.

```
import { useReducer } from "react"

const reducer = (state, action)=>{

  const {type, payload} = action

  switch (type) {
    case 'inc':
      return {...state, count: state.count + payload}
    case 'dec':
      return {...state, count: state.count - payload}
    default:
      return state
  }
}

const useCounter = ()=>{

  const [state, dispatch] = useReducer(reducer, {count: 0, step: 1})
  return [state, dispatch]
}

export default useCounter
```

#### useMemo

Used to memoize values. As long as dependencies don't change, the cached value will be returned. It has a dependency array like useEffect.

Big useCases -

1. Memoizing props to prevent wested renders (with memo)
2. Memoizing values to avoid expensive calcs on every render
3. Memoizing values that are used in dependency array of another hook.

```
const {useMemo} from 'react'

function Component(posts){

  const memorizedValue = useMemo(()=>{

    const calc = 234534234**234**(posts.length)
    const obj = {theme: 'dark', value: calc}

    return obj
  }, [posts])

  return (
    <.../>
  )
}

```

#### useCallback

This hook is used to memoize Functions. Usage is same as useMemo

```
const {useCallback} from 'react'

function Component(posts){

  const memorizedFunction = useCallback(
    function handleClick(){
      // Handle click here
    }, []
  )

  return (
    <.../>
  )
}
```

### Routing in React

Use the package React-router-dom for routing. Multiple functions are allowed.

#### Base

```
function App() {

  return (

    <BrowserRouter>
      <Routes>
        <Route index element={<HomePage/> replaceto="/"} />
        <Route path="/product" element={<Product/>} />
        <Route path="app" element={<Home/>}>
          <Route index element={<CityList cities={cities}/>}/>
          <Route path="cities/:id" element={<City />}/>
          <Route path="countries" element={<CountryList cities={cities}/>}/>
        </Route>
        <Route path="*" element={<PageNotFound />} />
      </Routes>
    </BrowserRouter>

  );
}
```

#### React Routers with data loaders >v6

```
import { RouterProvider, createBrowserRouter } from "react-router-dom";

const menuLoader = async()=>{
  const menu = await fetch ('')  // Fetch menu
  return menu
}

const router = createBrowserRouter([
  {
    element: <AppLayout>,   // Parent route
    errorElement: <Error/>,
    children: [
      {
        path: "/",
        element: <Home />,
      },
      {
        path: "/menu",
        element: <Menu />,
        loader: menuLoader
      }
    ]
  }
]);

const App = () => {
  return <RouterProvider router={router}></RouterProvider>;
};

export default App;
```

AppLayout.js File 👇. With loading indicator

```
import { Fragment } from "react";
import Header from "./Header";
import { Outlet } from "react-router-dom";

const AppLayout = () => {

  const navigation = useNavigation()

  if(navigation.state === 'loading') return <Spinner/>

  return (
    <Fragment>
      <Header />
      <main>
        <Outlet/>  // To load the childrens
      </main>
    </Fragment>
  );
};

export default AppLayout;

```

How to Use the loaded data in File 🤔 ?? Ahah!! got it 👇

```
import { useLoaderData } from "react-router-dom";

function Menu() {
  const menu  = useLoaderData()
  return <ul>{menu.map(pizza=> <li key={pizza.id}>{pizza.name}</li>)}</ul>;
}

export default Menu;
```

What happens when error ?? 🫨😢😭. No worries

```
import { useNavigate, useRouteError } from 'react-router-dom';

function NotFound() {
  const navigate = useNavigate();
  const error = useRouteError() // Fetch messages

  return (
    <div>
      <h1>Something went wrong 😢</h1>
      <p>{error.data || error.message}</p>
      <button onClick={() => navigate(-1)}>&larr; Go back</button>
    </div>
  );
}

export default NotFound;

```

#### Use \<Link/> and \<NavLink/> to route through pages

```
function AppNav() {
  return (
    <nav className={styles.nav}>
      <ul>
        <li>
          <NavLink to="cities">Cities</NavLink>
        </li>
        <li>
          <NavLink to="countries">Countries</NavLink>
        </li>
      </ul>
    </nav>
  );
}

export default AppNav;
```

#### Using Querys and Params in urls

```
<Link to={`/app/cities/${id}?lat=${position.lat}&lng=${position.lng}`}>

function City() {

  const {id} = useParams()
  const [searchParams, setSearchParams] = useSearchParams() // For query

  const lat = searchParams.get('lat')
  const lng = searchParams.get('lng')
}
```

#### Programatic Navigation

Navigate to any page by using hook UseNavigate

```
const navigate = useNavigate()

clickHandler ()=>{
  navigate('home')
  navigate(-1) // To go back
}
```

### Context API

Manage global state with context API

Provider: Main place to store global state. Usually in the App component.

Consumer: Components that read the provided context value

#### Creating context

```
import {useContext, createContext, useReducer} from React

const Context = createContext()

const reducer = (state, action)=>{

  switch (action.type){

    case 'note':
      return {...state, action.payload}

    default:
      throw new Error('no type provided')
  }
}

function ContextProvider({children}){
  const initialState = {note: ''}
  const [state, dispatch] = useReducer(reducer, initialState)

  return (
    <Context.Provider value={state, dispatch}>
      {children}
    </Context.Provider>
  )
}

function useContext(){
  const context = useContext(Context)
  return context;
}

export {ContextProvider, useContext}
```

#### Using Context in child elements

```

import {ContextProvider, useContext} from '../Context'

function Header(){

  const {state, dispatch} = useContext()

  return <ContextProvider><App/></ContextProvider>
}
```

For maximum optimizationi use context this way. Use different context for different states. If multiple states are there in single contexts, all the components using the context will rerender on context change.

### Optimization

#### Wasted Renders

A render that didn't produce any change in the DOM are wasted renders. Component get re-renders in 3 situations -

1. State change
2. Context changes
3. Parent - Rerenders

* Common method to avoid rerender of a slow component is to -- Pass the \<SLOW\_COMPONENT> as a prop or a children.

#### Memoization with useMemo

Optimization technique that executes a pure function once, and saves the result in memory. If we try to execute the function again **with same arguments** as before, the previously saved result will be returned, without executing the function again.

Some points to discuss -

* Memoized child will not rerender when parents rerenders
* Memoized child will rerender when it's own state changes or change in context
* Use memo when the component is heave and often rerenders with same props

Memoize a component

```
import {memo} from 'react'

function slowFunction(){
  const expensiveCalc = ()=>{...}
  return(
    <div>{expensiveCalc}</div>
  )
}

export default memo(slowFunction) // Wrap it in memo
```

When props memo components are either functions or objects, use useMemo or useCallback hooks.

See [useMemo](react.md#useMemo) for memoize Values passed in props. See [useCallback](react.md#useMemo) for memoize functions passed in props.

#### BundleSize with codesplitting and Suspense

Load each element lazily in React Router. Use Router like this

```
import {lazy, Suspense} from 'react;
import SpinnerFullPage from './SpinnerFullPage'

const HomePage = lazy(()=> import('./HomePage'))
const Home = lazy(()=> import('./Home'))
const CityList = lazy(()=> import('./CityList'))
const PageNotFound = lazy(()=> import('./PageNotFound'))

<BrowserRouter>
  <Suspense fallback={<SpinnerFullPage/>}>
    <Routes>
      <Route index element={<HomePage/>} />
      <Route path="app" element={<Home/>}>
        <Route index element={<CityList cities={cities}/>}/>
      </Route>
      <Route path="*" element={<PageNotFound />} />
    </Routes>
  <Suspense/>
</BrowserRouter>
```

### Redux

Redux is a global state management tool simillar like useReducer + context API.

#### Redux in traditional way

Initiate Redux for a feature

```
// File - customerSlice.js

const InitialState = {
  name: '',
  age: 0
}

export default function(state = initialState, action){

  const {type, payload} = action

  switch (type){
    case 'customer/create':
      const {name, age} = payload
      return {...state, name, age}
    case 'customer/delete':
      return initialState
    default:
      return state
  }
}

export const createCustomer = (name, age)=> {
  return {type: 'customer/create', payload: {name, age}}
}

export const delCustomer = ()=> ({type: 'customer/delete})
```

Redux store file, creating the store by combining all Reducers

```
// - File store.js

import { combineReducers, createStore, applyMiddleware } from "redux"
import thunk from "redux-thunk"

import accReducer from './accountSlice'
import customerReducer from './customerSlice'


const rootReducer = combineReducers({
  account: accReducer,
  customer: customerReducer
})

const store = createStore(rootReducer, applyMiddleware(thunk))


export default store;
```

Index.js file

```
import { Provider } from 'react-redux'
import store from './store'

const root = ReactDOM.createRoot(document.getElementById('root'))
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
)
```

App.js File - Use store for dispatching actions

```
// App.js

import { useSelector, useDispatch } from "react-redux";
import {createCustomer} from './customerSlice'

function App(){

  const customer = useSelector(store=> store.customer)
  const dispatch = useDispatch()

  const handleClick = ()=>{
    dispatch(createCustomer('Ronny', 25))
  }

  return (
    <>
      <h1>Welcome {customer.name}</h1>
      <button onClick={handleClick}>Click Me</button>
    </>

  )
}
```

Using Redux middleware with Thunk

```
// File - customerSlice.js

const InitialState = {
  name: '',
  age: 0,
  loading: false
}

export default function(state = initialState, action){

  const {type, payload} = action

  switch (type){
    case 'customer/create':
      const {name, age} = payload
      return {...state, name, age}
    case 'customer/delete':
      return initialState
    case 'customer/loading':
      return {...state, loading: true}
    default:
      return state
  }
}

export const delCustomer = ()=> {

  dispatch({type: 'customer/loading'})

  return async function(){  // Thunk middleware here 👈

    //Delete customer remotely

    // Dispatch and update locally
    dispatch ({type:'customer/delete'})
  }
}
```

#### Modern Redux with Redux Tool Kit

Configure Store. It brings thunks and combines reducer behind the scenes. 😲

```
import { configureStore } from "@reduxjs/toolkit";
import userReducer from './userSlice'

const store = configureStore({
  reducer: {
    user: userReducer
  }
})

export default store
```

Use store to wrap up in index.jsx or main.jsx file. That's it

```
import { Provider } from "react-redux";
import store from "./redux/store.js";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);

```

Creating Reducers or Slice is way easier now. 😃

```
import {createSlice } from "@reduxjs/toolkit"

const initialState = {
  username: ''
}

const userSlice = createSlice({
  initialState,
  name: 'user',
  reducers: {
    create (state, action){
      state.username = action.payload.name
    },
    delete (state){
      state.username = ''
    }
  }
})

export const {createUser, deleteUser} = userSlice.actions
export default userSlice.reducer
```

### React Query

#### Setup Reqct query

```
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";

const queryClient = new QueryClient({
  defaultOptions:{
    queries: {
      staleTime: 60*1000
    }
  }
})

const App = () => {

  return (
    <QueryClientProvider client={queryClient}>
      <RouterProvider router={router}></RouterProvider>;
    </QueryClientProvider>
  )
};

export default App;
```

#### Using React query

React query can work with graphql as well as any fetch API . Yayy 🤩

```
import MenuItem from './MenuItem'
import { useQuery } from "@tanstack/react-query";

const getMenu = async()=>{
  const res = await fetch('/url')
  const data = await res.json()
  return data
}

function Menu() {

  const {isLoading, data:menu, error} = useQuery({
    queryKey: ['menu'],
    queryFn: getMenu
  })

  if(isLoading) return <Spinner/>
  if(error) return <h1>{error}</h1>
  return <ul>{menu.map(pizza=> <MenuItem key={pizza.id} pizza={pizza}/>)}</ul>;
}


export default Menu;
```

#### Mutate with React query

Mutation is just writing or updating to database

```
const queryClient = useQueryClient()

const {isLoading, mutate} = useMutation({
  mutationFn: (id)=> deleteMenu(id),
  onSuccess: ()=> {

    console.log('success')

    // Invalidate the cache data and fetch new
    queryClient.invalidateQueries({
      queryKey: ['menu']
    })
  }

  onError: (err)=> alert('Error occoured')
})

return <button onClick={mutate}> ❌Click to delete </button>
```

React query can be used for prefetching data. Next page data 🤩

### Advance React patterns

#### Render props

For complete control over what the component renders, by passing in a function that tells the component what to render. **More common before hooks**. But still useful

```
export default function App() {
  return (
    <div>
      <h1>Render Props Demo</h1>

      <div className="col-2">
        <List
          title="Products"
          items={products}
          render={(product) => (
            <ProductItem key={product.productName} product={product} />
          )}
        />
        <List
          title="Companies"
          items={companies}
          render={(company) => (
            <CompanyItem key={company.companyName} company={company} />
          )}
        />
      </div>
    </div>
  );
}
```

```
function List({ title, items, render }) {
  const isCollapsed = false;
  const displayItems = isCollapsed ? items.slice(0, 3) : items;

  return (
    <div className="list-container">
      <div className="heading">
        <h2>{title}</h2>
      </div>
      {<ul className="list">{displayItems.map(render)}</ul>}
    </div>
  );
}
```

This way The List component don't know what it is rendering, and control is with the App. List is acting a kind of layout. App can use List and render Products and Companies list.

#### Higher Order Component

It is a wrapper to a component. HOC takes a component and returns a simillar component with enhanced features. Naming starts "with..." example - withToggles. Given below an example, how a normal list features are enhanced with HOC wrapper.

```
// App.js

export default function App() {

  const ProductListWithToggles = withToggles(ProductList)

  return (
    <div>
      <h1>HOC Demo</h1>

      <div className='col-2'>
        <ProductList title="Products HOC" items={products}/>
        <ProductListWithToggles title="Products HOC" items={products}/>
      </div>
    </div>
  );
}
```

```
// File HOC

import { useState } from "react";

export default function withToggles(WrappedComponent) {
  return function List(props) {
    const [isOpen, setIsOpen] = useState(true);
    const [isCollapsed, setIsCollapsed] = useState(false);

    const displayItems = isCollapsed ? props.items.slice(0, 3) : props.items;

    function toggleOpen() {
      setIsOpen((isOpen) => !isOpen);
      setIsCollapsed(false);
    }

    return (
      <div className="list-container">
        <div className="heading">
          <h2>{props.title}</h2>
          <button onClick={toggleOpen}>
            {isOpen ? <span>&or;</span> : <span>&and;</span>}
          </button>
        </div>
        {isOpen && <WrappedComponent {...props} items={displayItems} />}

        <button onClick={() => setIsCollapsed((isCollapsed) => !isCollapsed)}>
          {isCollapsed ? `Show all ${props.items.length}` : "Show less"}
        </button>
      </div>
    );
  };
}
```

#### Compound component pattern

For very self-contained components that need/want to manage their own state. Compound components are like fancy super-components

```
import { createContext, useContext, useState } from "react";

//1. Create a context
const CounterContext = createContext();

//2. Create parent component
function Counter({children}) {
  const [count, setCount] = useState(0);

  const increase = () => setCount((c) => c + 1);
  const decrease = () => setCount((c) => c - 1);

  return (
    <CounterContext.Provider value={{ count, increase, decrease }}>
      <div>{children}</div>
    </CounterContext.Provider>
  );
}

// 3. Create child components
function Count(){
  const {count} = useContext(CounterContext)
  return <span>{count}</span>
}
function label({children}){
  return <span>{children}</span>
}
function increase({icon}){
  const {increase} = useContext(CounterContext)
  return <button onClick={increase}>{icon}</button>
}
function decrease({icon}){
  const {decrease} = useContext(CounterContext)
  return <button onClick={decrease}>{icon}</button>
}


//4. Add child components as properties
Counter.Count = Count
Counter.Label = label
Counter.Increase = increase
Counter.Decrease = decrease


export default Counter;

```

```
import Counter from "./Counter";
import "./styles.css";

export default function App() {
  return (
    <div>
      <h1>Compound Component Pattern</h1>
      <Counter>
        <Counter.Decrease icon="-"/>
        <Counter.Label>My super flexible counter</Counter.Label>
        <Counter.Increase icon="+"/>
        <Counter.Count/>
      </Counter>
    </div>
  );
}
```

### ESLint Setup

#### Packages needed

```
yarn add -D eslint vite-plugin-eslint
```

#### vite.config.js

```
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
import eslint from 'vite-plugin-eslint'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react(), {
    ...eslint(),
    apply: 'build'
  },
  {
    ...eslint({
      failOnError: false,
      failOnWarning: false,
      emitWarning: true
    }),
    apply: 'serve',
    enforce: 'post'
  }
  ],
})
```

#### .eslintrc.cjs

```
module.exports = {
  root: true,
  env: { browser: true, es2020: true },
  extends: [
    'eslint:recommended',
    'plugin:react/recommended',
    'plugin:react/jsx-runtime',
    'plugin:react-hooks/recommended',
  ],
  ignorePatterns: ['dist', '.eslintrc.cjs'],
  parserOptions: { ecmaVersion: 'latest', sourceType: 'module' },
  settings: { react: { version: '18.2' } },
  plugins: ['react-refresh'],
  rules: {
    'react-refresh/only-export-components': [
      'warn',
      { allowConstantExport: true },
    ],
    'react/prop-types':0,
    'no-unused-vars': 'warn'
  },
}

```
