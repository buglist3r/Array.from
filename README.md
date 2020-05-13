# How Array.from() works
  Lets explain first what Array.from():
Syntax: ```Array.from(arrayLike [, mapFn [, thisArg]])```    // According to MDN
First part : `arrayLik`e:
            An array-like or iterable object to convert to an array.
            Means it can take `Array[]` of iterable objects (objects such as `Map` and `Set`).
           

Second part: `mapFn`:
             Map function to call on every element of the array.
             The same when you are using map methods on array it will call on each element on the giving array

Third part:  `thisArg`:
             Value to use as this when executing `mapFn`.

    
The result from `Array.from()` it will return a **new Array instance** means it will get **shallow-copied Array** for the giving **array** or **iterable objects**.


So lets make simple example:

```javascript
let myValue = "Here we are doing a test";
Array.from(myValue);
["H", "e", "r", "e", " ", "w", "e", " ", "a", "r", "e", " ", "d", "o", "i", "n", "g", " ", "a", " ", "t", "e", "s", "t"]
```
Let do another example with iterable objects (objects such as Map and Set)
```javascript
let myMap = new Map();
myMap.set("name", "adel");
Map(1) {"name" => "adel"}
```
So if you don't know how `Map` works you should go to MDN and read about it.  it will return Map Object so lets convert it with `Array.from()`
[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map

```javascript
Array.from(myMap);
[Array(2)]
    0: (2) ["name", "adel"]
    length: 1
    __proto__: Array(0)
```
As you see `Array.from()` return **new array + Map Object** return also an array if you want to get the value direct then we have to do **destructuring**
```javascript
Array.from(...myMap);
(2) ["name", "adel"]
```

We now did an example with String and `Map Object` lets do now with `Set`

```javascript
let mySet = new Set();
mySet.add("adel")
Set(1) {"adel"}
mySet.add(29)
Set(2) {"adel", 29}
mySet
Set(2) {"adel", 29}
Array.from(mySet)
(2) ["adel", 29]
```
thats simply it if you wanna know more about `Set` you can read it on **MDN**
[1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set

Also **important** note some times you can also use Array.from to convert HTML Node list to array to be able to use the array methods on it.

Now lets go back to our example after knowing full details about what `Array.from()` is.

```javascript
function createArray(numberOne, numberTwo) {
  let theVar = Array.from({ length: (numberTwo - numberOne + 1)}, (_, i) => i + numberOne);
  return theVar;
}
console.log(createArray(5, 10))
```

Lets now analyze that code.
1. Create function takes 2 args `(numberOne, numberTwo)`
2. `Array.from({ length: (numberTwo - numberOne + 1)}, (_, i) => i + numberOne);`
   That mean `Array.from(obj, mapFn, thisArg)`
   `Array.from({length: (numberTwo - numberOne + 1)}) // first part we give the object` key:value so `{length: 10 - 5 + 1}` will be **6** so the result should be **6 numbers* as expected `[5, 6, 7, 8, 9, 10]`

   Second part is the `mapFn` to be able to understand why we are using object with `{length}` you have to read the docs and **the docs say's `Array.from()` let you create: array-like objects (objects with a length property and indexed elements)**
   the **length initaly** is `1`.
   
   so `Array.from({length: 6})` it will return array with `6` times `undefined` because we dont have any array `[undefined, undefined, undefined, undefined, undefined, undefined]`
   lets continue
   
    `Array.from({length: (numberTwo - numberOne + 1)}, (_, i) => i + numberOne)`
    `(_, i)` -> this `_` its a place holders because there is no array here **so you can call it anything** `_` or any **other name** and the `i` is the index
   
        
   lets come to this part now  `i + numberOne` the value of the i will be index of the length object and if you wanna understand it deeply run the function like that without numberOne

```javascript
function createArray(numberOne, numberTwo) {
  let theVar = Array.from({ length: (numberTwo - numberOne + 1)}, (_, i) => i );
  return theVar;
}
console.log(createArray(5, 10))
VM9226:9 (6) [0, 1, 2, 3, 4, 5]
```
As you see it will return the **index of the length number** it make sense yea so when we add now `numberOne` which is `5` it will take the **first index** which is `0 + numberOne` which is `5` then it will be `5` then add `1` more to `5` to be come `6` etc

```javascript
function createArray(numberOne, numberTwo) {
  let theVar = Array.from({ length: (numberTwo - numberOne + 1)}, (_, i) => i + numberOne );
  return theVar;
}
console.log(createArray(5, 10))
VM9526:9 (6) [5, 6, 7, 8, 9, 10]
```

As you see then the function will generate the givin range of numbers.
Hopfully this will help everyone to understand how this wokrs!

