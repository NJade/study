# Variable Declartions
## **var**
```typescript
function f() {
    var a = 10;
    return function g() {
        var b = a + 1;
        return b;
    }
}

var g = f();
g(); // return 11
```
> g 함수에서 f에 선언되어 있는 a를 캡쳐해서 사용
```typescript
function f(shouldInitialize: boolean) {
    if (shouldInitialize) {
        var x = 10;
    }
    return x;
}
f(true); // return 10
f(false); // return undefined
```
> x가 if 문 안에 선언되어 있으나 밖에서 접근이 가능<br/>
var는 블록에 상관 없이 함수, 모듈 등에서 접근 가능한 scope를 가짐<br/>
이런 scope를 var-scoping 또는 function-scoping이라고 부름<br/>
parameters도 function-scope
```typescript
function sumMatrix(matrix: number[][]) {
    var sum = 0;
    for (var i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (var i = 0; i < current.length; i++) {
            sum += currentRow[i];
        }
    }
    return sum;
}
```
> i를 두 번 선언하더라도 에러를 내지 않음
```typescript
for (var i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100*i);
} 
// 10번의 10
// setTimeout이 같은 scope의 같은 i를 참조하여 나타난 문제

for (var i = 0; i < 10; i++) {
    // 현재 i를 캡처
    (function(i) {
        setTimeout(function() { console.log(i); }, 100 * i);
    })(i);
}
```
## **let**
#### scope
```typescript
function f(input: boolean) {
    let a = 100;
    if (input) {
        let b = a + 1;
        return b;
    }
    return b; // Error, block scope
}

try {
    throw "ha";
}
catch (e) {
    console.log(e);
}
console.log(e); // Error
```
```typescript
a++ // Error
let a;
```
```typescript
function foo() {
    return a; // capture a
}
foo(); // a가 선언 된 적 없기 때문에 에러
let a;
```
#### Re-declartions & shadow
```typescript
function f(x) {
    var x;
    var x;
    if (true) {
        var x;
    }
}
```
> 위의 x는 모두 같은 x를 사용하며 위의 선언은 유효하다.
```typescript
let x = 10;
let x = 20; // Error, x는 이미 선언되어 있기 때문에
```
```typescript
function f(x) {
    let x = 100; // Error, 함수의 파라미터가 이미 선언했음
}
function g() {
    let x = 100;
    var x = 100; // Error, x가 두 번 선언됌
}
```
```typescript
function f(condition, x) {
    if (condition) {
        let x = 100; // 다른 블록이기 때문에 선언 가능
        return x;
    }
    return x;
}
f(false, 0); // 0
f(true, 0); // 100
```
```typescript
function sumMatrix(matrix : number[][]) {
    let sum = 0;
    for (let i = 0; i < matrix.length; i++) {
        var currentRow = matrix[i];
        for (let i = 0; i < currentRow.length; i++) {
            sum += currentRow[i];
        }
    }
    return sum;
}
```
> 안의 루프에서 i가 가려지기 때문에 위 함수는 올바름
```typescript
function theCityThatAlwaysSleeps() {
    let getCity;
    if (true) {
        let city = "Seattle";
        getCity = function() {
            return city;
        }
    }
    return getCity();
}
```
> 환경에 city를 잡기 때문에 블록 밖에서도 결과를 얻을 수 있음.
```typescript
for (let i = 0; i < 10; i++) {
    setTimeout(function() { console.log(i); }, 100*i);
}
// let을 for에서 사용 시 각 반복마다 새로운 scope를 생성하여 원하는 결과를 얻을 수 있음.
```
## **const**
* let과 같은 scope를 가지지만 재할당은 불가능
```typescript
const kitty = {
    name: "kim",
    numLives: 9,
}
// Error
kitty = {
    name: "lee",
    numLives: 9,
}

kitty.name = "lee"; // Ok
kitty.numLives--; // Ok
```
## **Array destructuring**
```typescript
let input = [1, 2];
let [first, second] = input;
[second, first] = [first, second];
function f([first, second]: [number, number]) {
    //
}
f([1, 2]);
```
```typescript
let [first, ...rest] = [1, 2, 3, 4];
console.log(rest) // [2, 3, 4]
```
```typescript
let [first] = [1, 2, 3, 4];
console.log(first);
let [, second, , fourth] = [1, 2, 3, 4];
console.log(second);
console.log(fourth);
```
## **Tuple destructuring**
```typescript
let tuple: [number, string, boolean] = [7, "hello", true];
let [a, b, c] = tuple;
let [d, e, f ,g] = tuple; // Error
let [h, ..ij] = tuple // ij: [string, boolean]
let [k, l, m, ...n] = tuple // n: [], empty tuple
let [o] = tuple // o: number
let [, p] = tuple // string
```
## **Object destructuring**
```typescript
let o = {
    a: "foo",
    b: 12, 
    c: "bar"
};
let { a, b } = o; // a = o.a, b = o.b
let { c, ...rest } = o;
let total = rest.b + rest.c.length;
let { a: newName1, b: newName2 } = o; // property renaming
let { a, b }: { a: string, b: number } = o;
```
```typescript
function keepWholeObject(wholeObject: { a: string, b?: number }) {
    let { a, b = 1001 } = wholeObject; // Default Values
}
```
## **Function declarations**
```typescript
type C = { a: stirng, b?: number };
function f({ a, b }: C): void {
    // ...
}  
function f({ a="", b=0 } = {}): void {
    //...
}
f()
```
```typescript
function f({ a, b=0 } = { a: "" }): void {
    // ...
}
f({ a: "yes" }) // ok
f() // ok
f({}) // Error, 'a'는 필수
```
## **Spread**
* 비구조화의 반대
```typescript
let first = [1, 2];
let second = [3, 4];
let bothPlus = [0, ...first, ...second, 5]; // [0, 1, 2, 3, 4, 5], shallow copy
let defaults = { food: "spicy", price: "$$" };
let search = { ...defaults, food: "rich" }; //{ food: "rich", price: "$$" }
```
```typescript
class C {
    p = 12;
    m() {}
}
let c = new C();
let clone = { ...c };
clone.p; // Ok
clone.m(); // Error, 전개 시 메소드는 전달되지 않음
// typescript 컴파일러는 전개 시 일반 함수로부터 파라미터 전개를 허가하지 않음
```