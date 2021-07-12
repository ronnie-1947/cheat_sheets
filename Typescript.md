# Typescript


* [Basic Types](#basic-types)
    + [Known Types](#known-types)
    + [Object types](#object-types)
        - [Mention specific Object types](#mention-specific-object-types)
        - [Without mentioning Object types](#without-mentioning-object-types)
    + [Array Type](#array-type)
    + [Tuple Type](#tuple-type)
    + [Enum Type](#enum-type)
    + [Any Type](#any-type)
    + [Union Types](#union-types)
    + [Function type](#function-type)
* [Type Alias](#type-alias)

## Basic Types

### Known Types
- number
- string
- boolean
- Object { }
- Array [ ]
- any

__________________________

### Object types

#### Mention specific Object types

```
//--- Interface option ---//

interface person {
    name:string;
    hobby: (string|number)[]
}

const person: person = {
    name: 'John',
    hobby: ['hello', 33]
}

```

#### Without mentioning Object types
Don't get autocompletion here
```
//--- Without Interface option ---//

const user = {
    name: 'Doe',
    age: 23
}
```
__________________

### Array Type

```
const arr: string[] = ['Hello', 'Hi'] // Array of strings

const age: number[] = [ 23, 43] // Array of numbers

const user: (string|number)[] = ['John', 23] // Array of string and numbers
```
__________________

### Tuple Type
Tuple are type of array whose length is fixed and have unique types

Push are allowed on tuples. Typescript allows to push el on tuples

```
const role: [number, string] = [23, 'John Doe']
```
___________________

### Enum Type
Enums can hold global constants , where elements are numbered with position

```
enum Role {
    ADMIN = 'adm', 
    READ_ONLY = 'read', 
    AUTHOR = 33
}

console.log(Role.ADMIN) // = adm
console.log(Role.READ_ONLY) // = read
console.log(Role.AUTHOR) // = 33
```
____________________

### Any Type

Set any type with 'any'

____________________

### Union Types

Accept more than 1 type
```
const man : number|string = 'hello' // man can be of both string and number

const user: (string|number)[] = ['John', 23] // Array of string and number
```
______________________

### Function type
```
type add = (num1:number, num2:number)=>number // Type add function returns a number

const add: add = (num1, num2)=>{
    return num1+num2
}
```
```
type sayit = ()=>void // Type sayit function and returns void

const sayit:sayit = ()=>{
    console.log('Hello world')
}
```
______________________

## Type Alias
Type aliases are used to create your own types
```
type User = {name: string; age:number}

const nameUser = (user:User)=>{
    console.log(user.name + ' is ' + user.age + ' years old')
}
```

```
type name = (string|boolean)[];

const name:name = ['Harry', true, 'Doe', false]
```
_________________________

## Interface

Interfaces are object types 
```
interface User {
    name: string;
    age: number;
}

interface Company extends User {
    company: string;
}

const user: Company = {
    name:'',
    age: 23,
    company:''
}
```
_________________________

## Intersection Types

Combine two or more type alias and combine its common types

```
type Admin = {
    name: string;
    privileges: string[];
}

type Employee = {
    name: string;
    startDate: string;
}

type ElevatedEmployee = Admin & Employee;

const e1: ElevatedEmployee = {
    name: 'Max',
    privileges: ['create-server'],
    startDate: new Date().toDateString()
}
```
```
type Combinable = string | number ;
type Numeric = number | boolean;

type Universal = Combinable & Numeric;

const numb: Universal = 23
```