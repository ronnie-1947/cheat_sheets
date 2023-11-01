# React JS

## State

React is all about state management. State is a component's and its children's internal data. If state changes, rerender of component and its children (who are using that state) takes place.

## Pure functions vs Impure functions

### Pure Functions

Pure functions only depend on their input parameters and do not modify the state of the application or have side effects.

Result of Pure function don't change with an outside parameter

```
// This function only depend on the value of r (its parameter)
function pureFunc (r) {

  return 3.14 * r*r
}
```

**React components are pure functions**

### ImPure Functions

Can have unpredictable behavior and can modify the state of the application or have side effects. Side effects such as initiate a timer or change the state of another function.

```
let pi = 3.14 * new Date().getHours()

// This function depends on the value of pi and Date. The final result can change with time.
function impureFunction (r){
  return pi * r*r
}
```

## How React works

React paints the DOM in 3 phases

- Create Virtual DOM (Render phase)
- Create Fiber tree (Reconciliation)
- Commit Phase (Paints the DOM)

### Create Virtual DOM

Virtual DOM is a JS object and it is created pretty fast. if the state is changed in some component, all of its child component goes to a modified state.

### Reconciliation (Diffing)

Fiber tree creation, React compares the current tree with the virtual DOM to create an updated Tree.

- The required props are passed in child components
- Repaint happens in child components
- State is morized if the component is not erased or no change in position of component
- Loss of state if component is deleted

Key props is used to uniquely identify a component.

NOTE: Don't set the key to index of the array. If there is remove of elements in between, the state of the elements get jumbled up

## Events

React introduce us to synthetic events so that most of the event works same across all browsers. And synthetic events bouble except the scroll event.

## component LifeCycle

1. Mount / Initial Render 🐣
2. Re-Render 💫
3. Unmount ☠️

## Hooks

Special built-in functions that allow us to hook into React internals:

1. Creating and accessing state from Fiber tree
2. Registering side effects in Fiber tree
3. Manual DOM selection
4. Etc.

### Hooks list

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

### Hooks Rules

1. Hooks need to called from the top level of a React component
2. React keep record for order of hooks written

### Use State Hook

```
const [state, setState] = useState(true)

<!--  setState accepts a function as arg where cur is the current state -->
const handleState = ()=>{
  setState(cur=> !cur)
}
```

Key points:

- If useState is called multiple times inside handle func

```
const handleState = ()=>{
  setState(cur=> cur + 1)
  setState(cur=> cur + 1)
  setState(cur=> cur + 1)
  }
```

- Multiple setState are batched into one and rendering takes place only once with all state change. Auto batching is enabled in React 18+
- setState is asynchrnous
- Opt out of automatic batching by wrapping a state update in ReactDOM.flushSync()

useState hook also accepts a function with no args

```
const [movies, setMovies] = useState(()=>{
  const movies = localStorage.getItem('movies')
  return JSON.parse(movies)
})
```

### useRef() Hook

A hook to store DOM elements.

```
const inputEl = useRef(null)

useEffect(()=>{
  console.log(inputEl.current) // Shows the input element
}, [])


return <input ref={inputEL}/>
```

useRef hook don't result in component render when the value is updated.

### useReducer Hook

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

## Routing in React

Use the package React-router-dom for routing. Multiple functions are allowed.

### Base

```
function App() {

  return (
    
    <BrowserRouter>
      <Routes>
        <Route index element={<HomePage/>} />
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

### Use \<Link/> and \<NavLink/> to route through pages
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

### Using Querys and Params in urls
```
<Link to={`/app/cities/${id}?lat=${position.lat}&lng=${position.lng}`}>

function City() {

  const {id} = useParams()
  const [searchParams, setSearchParams] = useSearchParams() // For query

  const lat = searchParams.get('lat')
  const lng = searchParams.get('lng')
}
```

### Programatic Navigation

Navigate to any page by using hook UseNavigate
```
const navigate = useNavigate()

clickHandler ()=>{
  navigate('home')
  navigate(-1) // To go back
}
```

## Context API
Manage global state with context API

Provider: Main place to store global state. Usually in the App component.

Consumer: Components that read the provided context value

### Creating context
```
import {useContext, createContext} from React

const Context = createContext()

function ContextProvider({children}){

  const [text, setText] = useState(')
  const initialState = {text, setText}

  return (
    <Context.Provider value={initialState}>
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

### Using Context in child elements
```

import {ContextProvider, useContext} from '../Context'

function Header(){

  const {user, updateUser} = useContext()

  return <ContextProvider><App/></ContextProvider>
}
```



