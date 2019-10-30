JS ES6--
============


## New Functions

| Function | Description |
| ------- | ----------- |
| `n.repeat(5)` |repeat n 5 times|
| `new Date()` |current date|
| `new Date().getFullYear()` |current year|
| `Object.keys(obj_name)` |fetch keys of the object in an array|
| `Object.values(obj_name)` |fetch values of the object in an array|
| `Math.max(x , y , z ...)` |return the maximum number|
| `Math.min(x , y , z , ...)` |return the minimum number|
| `` |return the minimum number|

### Notes
* document.querySelector(.'className').addEventListener('click', function() { } )
* use 'function (){...}' while using prototype.functionName . Don't use arrow function
* Objects can store anything . A function too.
    ```
    const box = {
        color: green,
        position: 1,
        clickMe: function (){
            document.querySelector('.class').addEventListener('click', ()=>{
                <!-- do anything -->
            })
        }
    }
    ```

* Function Constructors returns object
    ```
    function Person(name, age , sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
    }

    const John = new Person('John', 21, 'male');

    console.log(John);
    console.log(Object.keys(John));
    console.log(Object.values(John));

    // Destructuring
    const {name: any_name, age: any_age, sex: any_sex} = John;

    console.log(any_name);
    console.log(any_age);
    console.log(any_sex);
    ```
    ```
    //console
    Person { name: 'John', age: 21, sex: 'male' }
    [ 'name', 'age', 'sex' ]
    [ 'John', 21, 'male' ]
    John
    21
    male
    ```
* Destructuring is done on Arrays and Objects and also on function constructors. Use [ ] for arrays and { } for objects.
* document.querySelectorAll gives a NodeList . To convert NodeList to an array, use Array.from(List_name);
    ```
    const boxes = document.querySelectorAll('.box');

    Array.from(boxes).forEach((cur)=>{
    cur.style.backgroundColor = 'dodgerblue';
    })
    ```


## Array Functions

| Function | Description |
| ------- | ----------- |
| `Array.from(List_name)` |Convert list to an array ES6 way|
| `String.split('')` |Convert string to an array|
| `arr.join('')` |convert array to a string|
| `arr.startsWith('x')` |check if first element in arr is x|
| `arr.endsWith('x')` |check if last element in arr is x|
| `arr.includes('x')` |check if arr includes element x|
| `arr.map((el,index,cur) => { });` |loop over arr using map|
| `arr.slice(x,y)` |Start array from position x and ends at y-1|
| `arr.reduce(acc, cur)` |sums the array|
| `arr.shift()` |clip first element of array|
| `arr.pop()` |clip last element of array|
| `arr.reverse()` |reverse an array|
| `arr.sort()` |sort an array in alphabatical order|
| `arr.sort((a,b)=> a - b)` |sort an array increasing order|
| `arr.filter()` |sort an array increasing order|


## Looping an Array
4 methods to Loop an array

1. Using regular for loop
    ```
    for(let i = 0; i<= array.length; i++){
        // Blablabla
    }
    ```
2. Using forEach method
    ```
    array.forEach((cur)=>{
        // Blablabla
    })
    ```
3. Using map method . Creates a new array
    ```
    const newArr = array.map((cur)=>{
        // Blablabla
    })
    ```
4. Using For-of . Can use Break & continue methods
    ```
    for(const cur of Array ){
        // Blablabla
    })
    ```

## Maps
Maps are adv objects . In Maps we can use anything as keys , functions strings numbers etc.

| Map Properties & Methods | Description |
| ------- | ----------- |
| `Map_name.set('x', abc)` |Add element in a map|
| `Map_name.get('x')` |get the value of x|
| `Map_name.size` |No. of keys in the map|
| `Map_name.has(x)` |check if map has (x) key in it|
| `Map_name.delete(x)` |Delete (x) key from the map|
| `Map_name.clear()` |Clear everything from the map. Its a method|

* 2 ways to loop over a Map
```
Map.forEach((value, key)=>{
    `Bla bla bla`
})
```
```
for (let [key, value] of Map_name.entries()){
    bla bla bla
}
```

## Sets
Sets are types of adv arrays . In Sets no duplicates are there.

| Set Properties & Methods | Description |
| ------- | ----------- |
| `set_name.add('x')` |Add element in a set|
| `set_name.add(array)` |Add element of an array in a set|
| `set_name.size` |No. of elements in the set|
| `set_name.has(x)` |check if set has (x) in it|
| `set_name.delete(x)` |Delete (x) key from the set|
| `set_name.clear()` |Clear everything from the set. Its a method|

------------------------

## DOM Manupulation
```
elements.searchResPages.addEventListener('click', (e) => {
    console.log(e.target);
    const goToPage = parseInt(btn.dataset.goto);
    console.log(goToPage)
    } );
```
dataset property is used to read data of HTML element
