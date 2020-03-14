# Type
* Typescript는 타입 선언을 뒤에 한다.
* 세미콜론은 필수는 아님.

### Boolean
```typescript
let isDone: boolean = false;
```

### Number
```typescript
let decimal: number = 6;
let hex: number = 0xfood;
let binary: number = 0b1010;
let octal: number = 0o744;
```

### String
```typescript
let color: string = "blue";
color = 'red';
let sentence: string = "my favoriate color is ${ color }.";
```

### Array
```typescript
let list: number[] = [1, 2, 3];
```
```typescript
let list: Array<number> = [1, 2, 3];
```


### Tuple
```typescript
let x: [string, number];
x = ["hello", 10]; // OK
x = [10, "hello"]; // Error
console.log(x[0].substring(1)); // OK
console.log(x[1].substring(1)); // Error
x[3] = "world"; // Error
console.log(x[5]); // Error
```

### Enum
```typescript
enum Color {Red, Green, Blue}; // Red is 0
let c: Color = Color.Green;
```
```typescript
enum Color {Red = 1, Green, Blue};
```
```typescript
enum Color {Red = 1, Green = 2, Blue = 4};
```
```typescript
enum Color {Red = 1, Green, Blue}; 
let colorName: stirng = Color[2];
console.log(colorName); // Green, Green value is 2
```
### Any
```typescript
let notSure: any = 4;
notSure = "string";
notSure = false;
```
```typescript
let notSure: any = 4;
notSure.ifItExists(); // Okay
notSure.toFixed(); // Okay, compiler does not check.

let prettySure: Object = 4;
prettySure.toFixed(); // Error, toFixed does not exist on type Object.
```
* any는 컴파일에서 체크하지 않음.
```typescript
let list: any[] = [1, true, "free"];
list[1] = 100;
```

### void
```typescript
function warnUser(): void {
    console.log("warning message");
}
```
```typescript
let unusable: void = undefined;
unusable = null; // Ok, if --strictNullChecks is not given.
```
### Null & Undefined
```typescript
let u: undefined = undefined;
let n: null = null;
```
* number같은 타입에도 기본적으로는 할당 가능하지만 --strictNullChecks flag를 사용하면 any에만 할당 가능 (undefined는 void에도 할당 가능)
### Never
* 절대 발생해서는 안되는 값의 종류에 사용
* 함수는 throw를 던지거나 end point에 도달 불가능
* never는 모든 타입에 할당 가능한 서브타입
* any조차도 never 자체에는 할당 불가능
```typescript
function error(message: string): never {
    throw new Error(message);
}

function fail() {
    return error("Something is failed");
}

function infiniteLoop(): never {
    while (true) {
    }
} 
```

### Obejct
* non-primitive type
```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // Ok
create(null); // Ok

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error
```

### Type assertions
* 프로그래머가 컴파일러에게 알아서 한다고 말해주는 것
* 타입 캐스팅과 비슷하지만 체크나 데이터 재구축을 하지 않음
```typescript
let someValue: any = "This is a string";
let strLength: number = (<string>someValue).length;
strLength = (someValue as string).length;
```