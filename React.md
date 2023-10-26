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
- No repaint happens if there is no new component
- Loss of component state if component is deleted
- Child component are left untouched if there is no change required

Key props is used to uniquely identify a component. 

NOTE: Don't set the key to index of the array. If there is remove of elements in between, the state of the elements get jumbled up

