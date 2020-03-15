# Interfaces
* Typescript는 값이 가지는 형태에 초점을 맞춘 타입체크를 한다.<br/>
이는 때때로 duck typing 또는 structural sbutyping이라고 불린다.

## **First Interface**
```typescript
function printLabel(labelObj: { label: string }) {
    console.log(labelObj.label);
}
let myObj ={ size: 10, label: "Size 10 Object" };
printLabel(myObj);
```
* 위 예시에서 printLabel을 호출 시 타입을 체크한다.<br/>
파라미터는 더 많은 값을 가지고 있지만 string 타입의 label만 체크한다.
```typescript
interface LabeledValue {
    label: string;
}

function printLabel(labelObj: LabeledValue) {
    console.log(labelObj.label);
}

let myObj = { size: 10, label: "Size 10 Object" };
printLabel(myObj);
```
* 파라미터의 값이 존재하는지만 체크하며 순서는 상관하지 않는다.<br/>
다른 언어와 같이 특정 인터페이스를 구현하였다고 선언하지 않아도 된다.
## **Optional Properties**
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: "white", area: 100 };
    if (config.color) {
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

let mySquare = createSquare( { color: "black" });
```
* ?를 통해 추가적인 옵션을 줄 수 있다.<br/>
선택옵션으로 명시를 한다면 오탈자 등의 경우 컴파일러가 에러를 표시하여 준다.
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    let newSquare = { color: "white", area: 100 };
    if (config.clor) { // Error, typo
        newSquare.color = config.clor;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}

let mySquare = createSquare( { color: "black" });
```
## **Readonly properties**
```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
let p1: Point = { x: 10, y: 20 };
p1.x = 5; // Error
```
```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // Error
ro.push(5); // Error
ro.length = 100; // Error
a = ro; // Error, ReadonlyArray to Array is illegal
a = ro as number[];
```
## **readonly vs const**
* variable은 const, properties는 readonly
## **Excess Property Checkes**
```typescript
interface SquareConfig {
    color?: stirng;
    width?: number;
}
function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}
// Error, colour는 excess property
let mySqaure = createSquare({ colour: "red". width: 100 });
```
* 이런 검사를 위해서 가장 간단한 방법은 type assertion을 사용
```typescript
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```
* 추가적으로 사용되는 properties가 있다는 것을 확신하면 더 좋은 방법은 index signature를 추가하는 것
```typescript
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```
* 마지막 방법은 다른 변수에 할당하는 것
```typescript
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(sqaureOptions);
```
* 이 방법은 공통 property가 있어야 한다.
```typescript
let sqaureOptions = { colour: "red" };
let mySquare = createSquare(sqaureOptions); // Error
```
* 이러한 시도는 하지 않는 것이 좋으며 가능하다면 원래 interface를 수정하는 것이 옳다.
## **Function Types**
```typescript
interface SearchFunc {
    (source: string, subString: string): boolean;
}
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
    let result = source.search(subString);
    return result > -1;
}
```
* 함수 파라미터 이름이 일치할 필요는 없다.
```typescript
let mySearch: SearchFunc;
mySearch = function(src: string, sub: string): boolean {
    let result = src.search(sub);
    return result > -1;
}
```
* 파라미터 타입이나 반환값 타입은 인터페이스에서 추론됌
* 반환값의 타입이 적절하지 않은 경우 에러
```typescript
let mySearch: SearchFunc;
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```
## **Indexable Types**
```typescript
interface StringArray {
    [index: number]: string;
}
let myArray: StringArray;
myArray = ["Bob", "Fred"];
let myStr: string = myArray[0];
```
* 인덱스는 number나 string을 지원 <br/>
number 인덱서에서 반환되는 타입은 string 인덱서에서 반환되는 타입의 서브타입이어야 한다.<br/>
js에서 100으로 인덱싱하는 것은 '100'으로 인덱싱하는 것과 동일
```typescript
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}
// 이 경우 number 인덱서에서 반환되는 Animal은 string 인덱서에서 반환되는 Dog의 상위 타입이므로 에러
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}
```
* 반환 타입이 매치되도록 강요할 수 있다.
```typescript
interface NumberDictionary {
    [index: string]: number;
    length: number;
    name: string; // Error, stirng은 number의 서브 타입이 아님
}
```
* 반환타입이 여러 개로 다른 타입도 가능하다.
```typescript
interface NumberOrStringDictionary {
    [index: string]: number | string;
    length: number;
    name: string;
}
```
* readonly를 통해 인덱스를 통한 할당을 막을 수 있다.
```typescript
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // Error
```
## **Class Types**
* 다른 언어의 인터페이스처럼 가장 일반적인 사용법은 implementing
```typescript
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date): void;
}

Class Clock implements ClockInterface {
    currentTime: Date = new Date();
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) {}
}
```
* 인터페이스는 퍼블릭 사이드를 묘사, 이것은 인터페이스를 통해 클래스의 프라이빗을 체크하는 것을 막는다.
## **Difference between the static and instance sides of classes**
* static 사이드와 instance 사이드가 있다.
```typescript
interface ClockConstructor {
    new (hour: number, minute: number);
}

class Clock implements ClockConstructor {
    currentTime: Date;
    constructor(h: number, m: number) {}
}
```
* 인터페이스에 생성자 시그니쳐를 넣고 인터페이스를 적용한 클래스를 생성하려고 하면 에러가 날 수 있다.
* 이것은 클래스가 인터페이스를 구현하려고 할 때 클래스의 인스턴스 부분만 체크하기 때문이다. 생성자는 static 사이드이기 때문에 체크되지 않는다.
* 클래스의 정적인 측면에서 작업해야 한다. 아래 예시는 생성자를 위한 인터페이스와 인스턴스를 위한 인터페이스, 두 개의 인터페이스를 정의한다.
```typescript
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}

interface ClockInterface {
    tick(): void;
}

function createClock(
    ctor: ClockConstructor,
    hour: number,
    minute: number
): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log("beep beep");
    }
}

class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```
* 이 경우 첫 번째 파라미터가 올바른 생성자 시그니쳐인지 검사한다.
* 다른 방법은 class expressions를 사용하는 것이다.
```typescript
interface ClockConstructor {
    new (hour: number, minute: number);
}

interface ClockInterface {
    tick();
}

const Clock: ClockConstructor = class Clock implements ClockInterface {
    constructor(h: number, m: number) {}
    tick() {
        console.log("beep beep");
    }
}
```
## **Extending Interfaces**
```typescript
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = {} as Square;
sqaure.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```
## **Hybrid Types**
* 함수와 객체 역할을 함께 하는 객체
```typescript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

function getCounter(): Counter {
    let counter = function(start: number) {} as Counter;
    counter.interval = 123;
    counter.reset = function() {};
    return counter;
}

let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```
## **Interfaces Extending Classes**
* 인터페이스가 클래스를 상속하면 구현은 상속하지 않는다.<br/>
인터페이스는 베이스 클래스의 private과 protected도 상속한다.<br/>
큰 상속 체계를 만들거나 특정 properties를 가지는 서브 클래스를 만들 때 유용하다.
```typescript
class Control {
    private state: any;
}

interface SelectableControl extends Control {
    select(): void;
}

class Button extends Control implements SelectableControl {
    select() {}
}

class TextBox extends Control {
    select() {}
}

// Error: Property 'state'가 Image에 없다.
class Image implements SelectableControl {
    private state: any;
    select() {}
}

class Location {}
```
* 이 예제에서 Control의 자식만 private member인 state를 가질 수 있기 때문에 SelectableControl을 구현할 수 있다.
* SelectableControl은 select method를 가진 Control이다.<br/>
Button과 TextBox는 Control을 상속받으며 select메소드를 가지고 있기 때문에 SelctableControl의 서브타입이다. 하지만 Image와 Location은 그렇지 않다.