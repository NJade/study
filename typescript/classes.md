# Classes
## **Classes**
```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greeet() {
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```
## **Inheritance**
```typescript
class Animal {
    name: string;
    constructor(theName: string) {
        this.name = theName;
    }
    move(distanceInMeters: number = 0) {
        console.log('Animal moved ${distanceInMeters}m.');
    }
}

class Snake extends Animal {
    constructor(name: string) {
        super(name);
    }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) {
        super(name);
    }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```
## **Public, private, and protected modifiers**
## **Public by default**
* public이 기본값
## **ECMAScript Private Fields**
* Typescript 3.8부터 js의 private을 위한 새로운 문법을 지원
```typescript
class Animal {
    #name: string;
    constructor(theName: string) { this.#name = theName; }
}
new Animal("Cat").#name // #name은 private이므로 접근 불가
```

## **Understanding TypeScript's private**
```typescript
class Animal {
    private name: string;
    constructor(theName: stirng) {
        this.name = theName;
    }
}

new Animal("Cat").name; // Error
```
```typescript
class Animal {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}

class Rhino extends Animal {
    constructor() {
        super("Rhino");
    }
}

class Employee {
    private name: string;
    constructor(theName: string) {
        this.name = theName;
    }
}

let animal = new Animal("Goat");
let rhino = new Rhino();
let employee = new Employee("Bob");

animal = rhino;
animal = employee; // Error, Animal과 Employee는 호환되지 않음
```
* Employee는 Animal과 동일하게 private name을 가지고 있지만 Animal에서 선언된 name이 아니기 때문에 호환되지 않는다.
* Animal에서 name을 지우면 호환가능
## **Understanding protected**
* 다른 언어의 protected와 동일
## **Readonly modifier**
* Java의 final
## **Accessors**
```typescript
const fullNameMaxLength = 10;

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (newName && newName.length > fullNameMaxLength) {
            throw new Error("길이초과");
        }
        this._fullName = newName;
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```
1. ECMAScirpt 5 이상의 버전부터 호환
2. get은 가지나 set은 없는 경우 자동으로 readonly로 선언
## **Static Properties**
```typescript
class Grid {
    static origin = { x: 0, y: 0 };
    calculateDistanceFromOrigin(point: { x: number; y: number; }) {
        let xDist = point.x - Grid.origin.x;
        let yDist = point.y - Grid.origin.y;
        return Math.sqrt(xDist*xDist + yDist*yDist) / this.scale;
    }
    constructor(public scale: number) {}
}

let grid1 = new Grid(1.0);
let grid2 = new Grid(5.0);

console.log(grid1.calculateDistanceFromOrigin({ x: 10, y: 10 }));
console.log(grid2.calculateDistanceFromOrigin({ x: 10, y: 10 }));
```
## **Abstract Classes**
* 다른 클래스의 base class이지만 바로 인스턴스화 될 수 없음.
```typescript
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log("roaming the earth...");
    }
}
```
* abstact method는 abstract class에서 implementation되면 안되고 derived class에서 implementation 되야 함
```typescript
abstract class Department {
    constructor(public name: string) {}

    printName(): void {
        console.log(this.name);
    }
    abstract printMeeting(): void;
}

class AccountingDepartment extends Department {
    constructor() {
        super("...");
    }

    printMeeting(): void {
        console.log("....");
    }

    generateReports(): void {
        console.log(".....");
    }
}

let department: Department;
department = new Department(); // Error
department = new AccountingDepartment(); // Ok
department.printName();
department.printMeeting();
department.generateReports(); // Error
```
* abstact class가 아니어도 마지막 줄은 에러남.
## **Constructor functions**
```typescript
class Greeter {
    greeting: string;
    constructor(message: string) {
        this.greeting = message;
    }
    greet() {
        return "Hello, " + this.greeting;
    }
}

let greeter: Greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```
* 위 typescript를 컴파일하면 아래와 같이 된다.
```javascript
var Greeter = /** @class */ (function () {
    function Greeter(message) {
        this.greeting = message;
    }
    Greeter.prototype.greet = function () {
        return "Hello, " + this.greeting;
    };
    return Greeter;
}());
var greeter;
greeter = new Greeter("world");
console.log(greeter.greet());
```
* 각 클래스는 instance와 static side가 존재, var Greeter는 생성자 함수 new와 이 함수를 사용하면 우리는 이 클래스의 인스턴스를 얻을 수 있다. 생성자 함수는 클래스의 모든 static 맴버를 가지고 있다.
```typescript
class Greeter {
    static standardGreeting = "Hello, there";
    greeting: string;
    greet() {
        if (this.greeting) {
            return "Hello. " + this.greeting;
        }
        else {
            return Greeter.standardGreeting;
        }
    }
}

let greeter1: Greeter;
greeter1 = new Greeter();
console.log(greeter1.greet()); // Hello, there

let greeterMaker: typeof Greeter = Greeter;
greeterMaker.standardGreeting = "Hey there!";

let greeter2: Greeter = new greeterMaker();
console.log(greeter2.greet()); // Hey there!
console.log(greeter1.greet()); // Hey, there!
```
* typeof 는 클래스 그 자체를 달라는 것으로 이는 생성자 함수이다. 따라서 greeterMaker는 static 맴버를 가지고 있는 생성자 함수이므로 static변수를 수정하고 인스턴스를 만들 수 있다.
## **Using a class as an interface**
* 클래스 선언은 타입과 생성자를 만들어 내므로 인터페이스를 사용할 수 있는 동일한 곳에 사용이 가능하다.
```typescript
class Point {
    x: number;
    y: number;
}

// interface를 사용하여야하는 곳에 클래스를 사용하여 상속 가능
interface Point3d extends Point { 
    z: number;
}
```
