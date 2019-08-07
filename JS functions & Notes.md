JS ES6--
============


## New Functions

| Function | Description |
| ------- | ----------- |
| `n.startsWith('x')` |check if first element in n is x|
| `n.endsWith('x')` |check if last element in n is x|
| `n.includes('x')` |check if n includes element x|
| `n.repeat(5)` |repeat n 5 times|
| `n.map((el,index,cur) => { });` |loop over n using map|
| `new Date()` |current date|
| `new Date().getFullYear()` |current year|
| `Object.keys(obj_name)` |fetch keys of the object in an array|
| `Object.values(obj_name)` |fetch values of the object in an array|

### Notes
* document.querySelector(.'className').addEventListener('click', function() { } )
* use 'function (){...}' while using prototype.functionName . Don't use arrow function
* Objects can store anything . A function too . 
    ```
    const box = {
        color: green,
        position: 1,
        clickMe: function (){
            document.querySelector(.class).addEventListener('click', ()=>{
                <!-- do anything -->
            })
        }
    }
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
* Destructuring is done on Arrays and Objects and also on function constructors. Use [ ] for arrays and { } for objects.
