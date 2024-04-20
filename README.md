Async Js
============

## Promise
A promise is an object which can be returned synchronously from an asynchronous function.

```
// create a promise

const prom = new Promise((resolve, reject)=>{ ... })
```

## Consume Promises ( .then method & .catch method)
.then and .catch methods are ways to consume promises. 

```
const prom = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        resolve([1 , 2 , 3 , 4]);
    },2000)
})

const thrdelement = function (arr3){
    return new Promise ((resolve, reject)=>{
        setTimeout((el)=>{
            resolve(el)
        },2000, arr3)
    })
}

prom
.then((arr)=>{
    console.log(arr)
    return thrdelement(arr[2])
})
.then((x)=>{
    console.log(x);
})
```


## Consume Promises (Async Await)
Modern technique to run asynchrnous functions and synchrnous functions in synchrnous way.

```
const prom = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        resolve([1 , 2 , 3 , 4]);
    },2000)
})

const thrdelement = function (arr3){
    return new Promise ((resolve, reject)=>{
        setTimeout((el)=>{
            resolve(el)
        },2000, arr3)
    })
}

async function show(){
    const frst = await prom;
    console.log(frst);

    console.log(await thrdelement(frst[2]))
}
show()
```




## SetTimeOut Function
Asynchronous function in Javascript in which a function is delayed for n seconds.
```
setTimeout(()=>{...} , time) // function is executed after the time
```

## SetInterval Function
Asynchronous function in Javascript in which a function is repeated afterr n seconds delay.

```
ssetInterval(()=>{...}, n) // function is executed again and again on interval of n seconds
```