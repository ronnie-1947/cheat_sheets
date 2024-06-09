# Typescript

* [Basic Types](Typescript.md#basic-types)
  * [Known Types](Typescript.md#known-types)
  * [Object types](Typescript.md#object-types)
    * [Mention specific Object types](Typescript.md#mention-specific-object-types)
    * [Without mentioning Object types](Typescript.md#without-mentioning-object-types)
  * [Array Type](Typescript.md#array-type)
  * [Tuple Type](Typescript.md#tuple-type)
  * [Enum Type](Typescript.md#enum-type)
  * [Any Type](Typescript.md#any-type)
  * [Union Types](Typescript.md#union-types)
  * [Function type](Typescript.md#function-type)
* [Type Alias](Typescript.md#type-alias)

## Basic Types

### Known Types

* number
* string
* boolean
* Object { }
* Array \[ ]
* any

***

### Object types

#### Mention specific Object types

```typescript
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

```typescript
//--- Without Interface option ---//

const user = {
    name: 'Doe',
    age: 23
}
```

***

### Array Type

```typescript
const arr: string[] = ['Hello', 'Hi'] // Array of strings

const age: number[] = [ 23, 43] // Array of numbers

const user: (string|number)[] = ['John', 23] // Array of string and numbers
```

***

### Tuple Type

Tuple are type of array whose length is fixed and have unique types

Push are allowed on tuples. Typescript allows to push el on tuples

```
const role: [number, string] = [23, 'John Doe']
```

***

### Enum Type

Enums can hold global constants , where elements are numbered with position

```typescript
enum Role {
    ADMIN = 'adm', 
    READ_ONLY = 'read', 
    AUTHOR = 33
}

console.log(Role.ADMIN) // = adm
console.log(Role.READ_ONLY) // = read
console.log(Role.AUTHOR) // = 33
```

***

### Any Type

Set any type with 'any'

***

### Union Types

Accept more than 1 type

```typescript
const man : number|string = 'hello' // man can be of both string and number

const user: (string|number)[] = ['John', 23] // Array of string and number
```

***

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

***

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

***

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

***

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
