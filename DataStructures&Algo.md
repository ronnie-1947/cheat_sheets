# Data Structures & Algorithms

## Big O Notation

Big O notation is a way to describe the **efficiency of an algorithm** in terms of its worst-case scenario. It helps us understand how the runtime or space requirements of an algorithm grow as the input size increases. In simpler terms, it's like expressing the upper bound of how long an algorithm might take or how much space it might use. It's a handy tool for comparing algorithms and figuring out which one is more scalable.

## Frequency Counter

A simple problem to check whether the second array's elements array are a squared of the first's

```
const same = (arr, arr2) => {

  // Check array lengths
  if(arr.length !== arr2.length) return false

  // Make 2 new objects
  const freq1 = {}
  const freq2 = {}

  // Fill the objects
  arr.forEach((c, i) => {
    freq1[c] = !freq1[c] ? 1 : freq1[c] + 1 //Count frequency for arr1

    const d = arr2[i]
    freq2[d] = !freq2[d] ? 1 : freq2[d] + 1 // Count frequency for arr2
  })

  let result = true

  for (let key in freq1){
    if(!result) break
    const sqrKey = (+key) ** 2
    if(freq1[key] !== freq2[sqrKey]) result = false
  }

  return result
}
```

## Sets in JS

Using sets is an excellent thing to check if element is present in an array 🤩.

```
const set = new Set()
const set2 = new Set()

set.add(1) // Add new value
set.has(1) // True or false
set.difference(set2) //Prints only the difference

set.forEach(c=>{})
```

### Use of Set

```
function sumZero (arr){

  const set = new Set(arr)

  for (let el of set){
    if (el > 0) break

    if(set.has(-(el)))return [el, -el]
  }

}

const s = sumZero([-3, -2, 2, 5, 7, 9, -5])
```

### Big O for Sets

| Operation | Average/Best Case | Worst Case |
| --------- | ----------------- | ---------- |
| Insertion | O(1)              | O(1)       |
| Deletion  | O(1)              | O(1)       |
| Search    | O(1)              | O(1)       |
| Iteration | O(n)              | O(n)       |

## Linear Search

| Operation     | Time Complexity (Big O) |
| ------------- | ----------------------- |
| Linear Search | O(n)                    |

Linear search is a simple searching algorithm that sequentially checks each element in a list or array until a match is found or the end of the list is reached

## Binary Search

Binary search is a fast search algorithm that finds the position of a target value within a sorted array by repeatedly dividing the search range in half.

Binary search requires sorted data, random access to elements (preferably in an array), the ability to compare for equality, and efficient indexing.

```
const arr = Array.from({length: 2000000}, (_, i)=> i*2)

const findEl = (arr = [], n)=>{

  // Find start, end and mid indexes
  let [strt, end] = [0, arr.length-1]
  let mid = Math.floor(arr.length/2)

  // Start looping
  while (strt <= end){

    if(strt === mid || end === mid) return false //Terminate loop
    if(arr[mid] === n) return [mid, true]

    if(n > arr[mid]){
      strt = 0 + mid
    }else{
      end = 0 + mid
    }

    // Change the mid value
    mid = Math.floor((strt + end)/2)
  }

  return false
}



const fin = findEl(arr, 28)
console.log(fin)
```

## Sorting Algos (Simple ones)

### Bouble sort

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(n^2)                  |
| Best Case  | O(n)                    |

Best case is where the array is almost sorted. Bubble Sort works by repeatedly stepping through the list of elements, comparing adjacent elements, and swapping them if they are in the wrong order.

## Selection sort

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(n^2)                  |
| Best Case  | O(n^2)                  |

In every pass, the algorithm needs to find the minimum (or maximum) element in the unsorted part of the array and swap it with the first unsorted element. This process is repeated until the entire array is sorted.

## Insertion sort

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(n^2)                  |
| Best Case  | O(n)                    |

In the worst case, Insertion Sort has a time complexity of O(n^2). This occurs when the array is in reverse order, and each element needs to be moved to its correct position by shifting elements and making multiple comparisons.

In the best case, the time complexity is O(n), which happens when the array is already sorted

## Sorting Algos (Complex ones)

### Merge Sort

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(n log n)              |
| Best Case  | O(n log n)              |

Merge Sort divides the array into halves recursively until it reaches single-element subarrays. Then, it merges these subarrays in a sorted manner.

### Quick sort (Don't go with name)

Don't use it

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(n^2)                  |
| Best Case  | O(n log n)              |

Works by selecting one element (called the pivot) and finding the index where the pivot should end up in the sorted array. 😕😵

Select any element in an array and all the elements smaller push it to left and all the elements bigger, push it to right. Repeat it.

### Radix sort (Best one so far)

Loop run according to the base of the number (10 times for base 10 numbers)

1. make 10 buckets
2. Loop 10 times, and place the elements according to the digits in ones place
3. Then place the elements according to digits in 10s place
   And so on.

```
const {performance} = require('perf_hooks')

const arr = Array.from({length: 1000000}, ()=> Math.floor(Math.random()*100*2))
// const arr = [23, 34, 456, 567, 6745, 4523, 35, 567, 2345]


const maxNumDigits = (arr= [])=>{
  const max = arr.reduce((acc, cur)=>{ // Just because Math.max has limitations
    if (cur > acc) return cur
    return acc
  }, 0)

  numStr = `${max}`
  return numStr.length
}

const placeVal = (num=0, p=0)=>{
  strInt = `${num}`.split('')
  num = +strInt[strInt.length - p] || 0
  return num
}

function sortArr(arr=[]){

  // get the max num of digit (3000 - 4digits)
  const k = maxNumDigits(arr)

  // loop it k times
  for (let i = 1; i<=k; i++){
    const temp = Array.from({length: 10}, ()=> []) // make bucket

    arr.forEach((v)=>{ // Place value in bucket
      const p = placeVal(v, i)
      temp[p].push(v)
    })

    arr = [].concat(...temp) //Redistrubute to array from bucket
  }

  return arr;
}


const t1 = performance.now()
const sort = sortArr(arr)
const t2 = performance.now()

console.log(`performance is ${(t2-t1)/1000}`)
```

| Case       | Time Complexity (Big O) |
| ---------- | ----------------------- |
| Worst Case | O(k \* n)               |
| Best Case  | O(k \* n)               |

The Big O notation for Radix Sort is O(k \* n), where "k" is the number of digits in the maximum number and "n" is the number of elements in the array.

// Only works for positive integers

## The class keyword

To make a class

```

class Student {
  constructor(firstName, lastname){
    this.firstName = firstName
    this.lastname = lastname
  }
}

class StudentExtension  extends Student{
  constructor(firstName, lastname, age){
    super(firstName, lastname)
    this.age = age
  }

  showAge(){
    console.log(`${this.firstName}'s age is ${this.age}`)
  }
}

const s = new StudentExtension('Ronny', 'ynnor', 14)

s.showAge()

```

## Singly Link List

A singly linked list is a data structure that consists of a sequence of elements where each element points to the next one in the sequence. Here's a brief overview:

```
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
```

BIG O for Singly link list
| Operation | Time Complexity (Big O) |
|--------------------------------|--------------------------|
| Access/Search | O(n) |
| Insertion at the beginning | O(1) |
| Deletion at the beginning | O(1) |
| Insertion at the end | O(n) |
| Deletion at the end | O(n) |
| Insertion at a specific position| O(n) |
| Deletion at a specific position | O(n) |

## Doubly Link List

A doubly linked list is a linear data structure that consists of a sequence of elements, where each element points to both the next and the previous element in the sequence.

```
class Node {
  constructor(data) {
    this.data = data;
    this.prev = null;
    this.next = null;
  }
}

class DoublyLinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
  }

  append(data) {
    const newNode = new Node(data);
    if (!this.head) {
      this.head = newNode;
      this.tail = newNode;
    } else {
      newNode.prev = this.tail;
      this.tail.next = newNode;
      this.tail = newNode;
    }
  }
}
```

BIG O for doubly Link List
| Operation | Time Complexity (Big O) |
|--------------------------------|--------------------------|
| Access/Search | O(n) |
| Insertion at the beginning | O(1) |
| Deletion at the beginning | O(1) |
| Insertion at the end | O(1) |
| Deletion at the end | O(1) |
| Insertion at a specific position| O(n) |
| Deletion at a specific position | O(n) |

## Stacks

A stack is a linear data structure that follows the Last In, First Out (LIFO) principle.

- Array push
- Array pop

## Queues

A queue is a linear data structure that follows the First In, First Out (FIFO) principle.

- Single List Unshift
- Single List shift

### Get hand's dirty on queues

```
class Node{
  constructor(val){
    this.value = val
    this.next = null
  }
}

class Queue{
  constructor(){
    this.head = null
    this.tail = null
    this.length = 0
  }

  enqueue(val){
    const node = new Node(val)
    if(!this.head){
      this.head = node
      this.tail = this.head
    }else{
      this.tail.next = node
      this.tail = node
    }
    this.length++
  }

  dequeue(){
    if(!this.head) return null
    const temp = this.head
    if(this.tail === this.head) this.tail = null
    this.head = this.head.next
    this.length--
    return temp.value
  }
}
```

## Binary Trees

A binary tree is a hierarchical data structure composed of nodes, where each node has at most two children, referred to as the left child and the right child.

- Node without children are called leaves
- Rooted Structure: The tree must have a designated root node from which all other nodes are descended.

- At Most Two Children: Each node in the tree can have at most two children, the left child and the right child. Left is the smaller one and rigt is the bigger one

- Hierarchical Ordering: Nodes are organized in a hierarchical structure, with each level containing nodes that are connected to the level above and below.

| Operation | Average/Best Case | Worst Case |
| --------- | ----------------- | ---------- |
| Search    | O(log n)          | O(n)       |
| Insertion | O(log n)          | O(n)       |
| Deletion  | O(log n)          | O(n)       |

## Binary Tree Traversal

Binary tree traversal refers to the process of systematically visiting and processing each node in a binary tree.

| Traversal    | Average/Best Case | Worst Case |
| ------------ | ----------------- | ---------- |
| Breath First | O(n)              | O(n)       |
| Pre-order    | O(n)              | O(n)       |
| In-order     | O(n)              | O(n)       |
| Post-order   | O(n)              | O(n)       |

### Breath First search

Breath-First Search (BFS) is a traversal algorithm that systematically explores a binary tree level by level

- Time Complexity: O(n) - Where "n" is the number of nodes in the binary tree.

Traversal: In the worst case, BFS needs to visit all nodes in the binary tree. Since each node is visited once, the time complexity is linear, O(n).

### Depth First Preorder

Pre-Order Traversal:

- Visit the root node.
- Traverse the left subtree.
- Traverse the right subtree.
- Result: Root-Left-Right.

### Depth First Postorder

Post-Order Traversal:

- Visit the root node.
- Traverse the left subtree.
- Traverse the right subtree.
- Result: Root-Left-Right.

### Depth First Inorder

In-Order Traversal:

- Traverse the left subtree.
- Visit the root node.
- Traverse the right subtree.
- Result: Left-Root-Right.

## Heaps

A binary heap is a complete binary tree that satisfies the heap property.

| Operation         | Average/Best Case | Worst Case |
| ----------------- | ----------------- | ---------- |
| Insertion         | O(1)              | O(log n)   |
| Deletion          | O(log n)          | O(log n)   |
| Peek/Extract Root | O(1)              | O(1)       |
| Search            | O(n)              | O(n)       |
| Heapify           | O(n)              | O(n)       |
| Build Heap        | O(n)              | O(n)       |

### Max Heap

For a max heap, the value of each node is greater than or equal to the values of its children. All children are smaller than the parent node

### Min Heap

For a min heap, the value of each node is less than or equal to the values of its children. All children are greater than the parent

### Children Parent relationship

- Left children = (2n+1)
- Right children = (2n+2)
- Parent = Math.floor((n-1)/2)

### Heap insert

- push it in the array
- Check parent , if parent is smaller, swap it
- Log(n)

### Usage of Heaps

- Implement priority queues
- Graph Traversal

### Priority Queue Let's get hands dirty

```
class Node {
  constructor(val, priority) {
    this.val = val;
    this.priority = priority;
  }
}

class PriorityQueue {
  constructor() {
    this.values = [];
  }
  enqueue(val, priority) {
    let newNode = new Node(val, priority);
    this.values.push(newNode);
    this.bubbleUp();
  }
  bubbleUp() {
    let idx = this.values.length - 1;
    const element = this.values[idx];
    while (idx > 0) {
      let parentIdx = Math.floor((idx - 1) / 2);
      let parent = this.values[parentIdx];
      if (element.priority >= parent.priority) break;
      this.values[parentIdx] = element;
      this.values[idx] = parent;
      idx = parentIdx;
    }
  }
  dequeue() {
    const min = this.values[0];
    const end = this.values.pop();
    if (this.values.length > 0) {
      this.values[0] = end;
      this.sinkDown();
    }
    return min;
  }
  sinkDown() {
    let idx = 0;
    const length = this.values.length;
    const element = this.values[0];
    while (true) {
      let leftChildIdx = 2 * idx + 1;
      let rightChildIdx = 2 * idx + 2;
      let leftChild, rightChild;
      let swap = null;

      if (leftChildIdx < length) {
        leftChild = this.values[leftChildIdx];
        if (leftChild.priority < element.priority) {
          swap = leftChildIdx;
        }
      }
      if (rightChildIdx < length) {
        rightChild = this.values[rightChildIdx];
        if (
          (swap === null && rightChild.priority < element.priority) ||
          (swap !== null && rightChild.priority < leftChild.priority)
        ) {
          swap = rightChildIdx;
        }
      }
      if (swap === null) break;
      this.values[idx] = this.values[swap];
      this.values[swap] = element;
      idx = swap;
    }
  }
}


let ER = new PriorityQueue();
ER.enqueue("common cold", 5)
ER.enqueue("gunshot wound", 1)
ER.enqueue("high fever", 4)
ER.enqueue("broken arm", 2)
ER.enqueue("glass in foot", 3)
```

## Hash Maps

These are JS Objects. Nothing fancy 😌

| Operation | Average Case | Worst Case |
| --------- | ------------ | ---------- |
| Search    | O(1)         | O(n)       |
| Insert    | O(1)         | O(n)       |
| Delete    | O(1)         | O(n)       |

## Graphs

A graph is a data structure that consists of a finite set of nodes (or vertices) and a set of edges connecting these nodes. Graphs are used to represent relationships between pairs of objects.

### Terminologies

Graphs can be made in adjency List or adjency Matrice

- Node or Vertices

### Get hands dirty on Graphs

```
class Graph {
  constructor() {
    this.adjacencyList = {}
  }

  addVertex(v) {
    if (!this.adjacencyList[v])
      this.adjacencyList[v] = new Set()
  }

  addConn(v1, v2) {
    if (this.adjacencyList[v1] && this.adjacencyList[v2])
      this.adjacencyList[v1].add(v2)
    this.adjacencyList[v2].add(v1)
  }

  removeConn(v1, v2) {
    if (!this.adjacencyList[v1] || !this.adjacencyList[v2]) return
    this.adjacencyList[v1].delete(v2)
    this.adjacencyList[v2].delete(v1)
  }

  removeVertex(v){
    for (let node in this.adjacencyList){
      if(node === v){
        delete this.adjacencyList[v]
      }else{
        this.adjacencyList[node].delete(v)
      }
    }
  }
}
```

### Uses of Graphs

- Social Networks
- Location / Mapping
- Routing
- Visual Hierarchy
- File system optimizations

## Dijkstra's Algorithm

Find the shortest path between 2 nodes in a WEIGHTED GRAPH.

```
class PriorityQueue {
  constructor() {
    this.values = [];
  }
  enqueue(val, priority) {
    this.values.push({ val, priority });
    this.sort();
  };
  dequeue() {
    return this.values.shift();
  };
  sort() {
    this.values.sort((a, b) => a.priority - b.priority);
  };
}

class WeightedGraph {
  constructor() {
    this.adjacencyList = {};
  }
  addVertex(vertex) {
    if (!this.adjacencyList[vertex]) this.adjacencyList[vertex] = [];
  }
  addEdge(vertex1, vertex2, weight) {
    this.adjacencyList[vertex1].push({ node: vertex2, weight });
    this.adjacencyList[vertex2].push({ node: vertex1, weight });
  }
  Dijkstra(start, finish) {
    const nodes = new PriorityQueue();
    const distances = {};
    const previous = {};
    let path = [] //to return at end
    let smallest;
    //build up initial state
    for (let vertex in this.adjacencyList) {
      if (vertex === start) {
        distances[vertex] = 0;
        nodes.enqueue(vertex, 0);
      } else {
        distances[vertex] = Infinity;
        nodes.enqueue(vertex, Infinity);
      }
      previous[vertex] = null;
    }
    // as long as there is something to visit
    while (nodes.values.length) {
      smallest = nodes.dequeue().val;
      if (smallest === finish) {
        //WE ARE DONE
        //BUILD UP PATH TO RETURN AT END
        while (previous[smallest]) {
          path.push(smallest);
          smallest = previous[smallest];
        }
        break;
      }
      if (smallest || distances[smallest] !== Infinity) {
        for (let neighbor in this.adjacencyList[smallest]) {
          //find neighboring node
          let nextNode = this.adjacencyList[smallest][neighbor];
          //calculate new distance to neighboring node
          let candidate = distances[smallest] + nextNode.weight;
          let nextNeighbor = nextNode.node;
          if (candidate < distances[nextNeighbor]) {
            //updating new smallest distance to neighbor
            distances[nextNeighbor] = candidate;
            //updating previous - How we got to neighbor
            previous[nextNeighbor] = smallest;
            //enqueue in priority queue with new priority
            nodes.enqueue(nextNeighbor, candidate);
          }
        }
      }
    }
    return path.concat(smallest).reverse();
  }
}

var graph = new WeightedGraph()
graph.addVertex("A");
graph.addVertex("B");
graph.addVertex("C");
graph.addVertex("D");
graph.addVertex("E");
graph.addVertex("F");

graph.addEdge("A", "B", 4);
graph.addEdge("A", "C", 2);
graph.addEdge("B", "E", 3);
graph.addEdge("C", "D", 2);
graph.addEdge("C", "F", 4);
graph.addEdge("D", "E", 3);
graph.addEdge("D", "F", 1);
graph.addEdge("E", "F", 1);


graph.Dijkstra("A", "E");

// ["A", "C", "D", "F", "E"]
```
