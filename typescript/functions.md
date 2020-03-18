# Functions
## **Functions**
```typescript
// Named function
function add(x, y) {
    return x + y;
}

// Anonymous function
let myAdd = function(x, y) {
    return x+y;
}
```
* js에서 함수는 함수 밖의 변수를 참조할 수 있고 이 때 이 변수를 capture 한다.
```typescript
let z = 100;

function addToZ(x, y) {
    return x + y + z;
}
```

# **Function Types**
## **Typing the function**
```typescript
function add(x: number, y: number): number {
    return x+y;
}

let myAdd = function(x: number, y: number): number {
    return x+y;
}
```
## **Writing the function type**
```typescript
let myAdd: (x: number, y: number) => number = function(
    x: number,
    y: number
): number {
    return x+y;
}
```
* 파라미터 이름은 단지 가독성을 위한 것
```typescript
let myAdd: (baseValue: number, increment: number) => number = function(
    x: number,
    y: number
): number {
    return x+y;
}
```
* Arrow를 통해 return값을 명확하게 보여줄 수 있다.
## **Inferring the types**
* 한 쪽에 타입이 있으면 명시해주지 않아도 됨.
```typescript
// myAdd has the full function type
let myAdd = function(x: number, y: number): number {
    return x+y;
}

// 'x' and 'y' have the type number.
let myAdd: (baseValue: number, increment: number) => number = function(x, y) {
    return x+y;
}
```
## **Optional and Default Parameters**
* 모든 파라미터는 함수에서 필요한 것으로 요구된다.<br/>
이것은 null이나 undefined를 받을 수 없다는 뜻은 아니다.<br/>
함수가 기대하는 파라미터와 넘겨받는 파라미터의 갯수는 같아야 한다.
```typescript
function buildName(firstName: string, lastName: string) {
    return firstName + ' ' + lastName;
}

let result1 = buildName("Bob") // Error
let result2 = buildNmae("Bob", "Adams", "Sr.") // Error
let result3 = buildName("Bob", "Adams") // Ok
```
* js에서 모든 파라미터는 옵션이다. 이 경우 남은 파라미터는 undefined가 된다. Typescript는 ?로 이를 지원
```typescript
function buildName(firstName: stirng, lastName?: string) {
    if (lastName) return firstName + ' ' + lastName;
    else return firstName;
}

let result1 = buildName("Bob") // Ok
let result2 = buildName("Bob", "Adams", "Sr.") // Error
let result3 = buildName("Bob", "Adams") // Ok
```
* Default 값을 설정할 수 있다.
```typescript
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob"); // Ok
let result2 = buildName("Bob", undefined); // Ok
let result3 = buildName("Bob", "Adams", "Sr.") // Error
let result4 = buildName("Bob", "Adams") // Ok
```
* default 값이 있는 파라미터는 optional 값이 있는 파라미터와 함수 형태가 동일
```typescript
function buildName(firstName: string, lastName?: string) {
    // ...
}

function buildName(firstName: string, lastName = "Smith") {
    // ...
}
```
* 위 두 함수는 함수 형태가 `(firstName: string, lastName?: string) => string`으로 동일
* optional 파라미터와 달리 default 값이 있는 parameter는 필수 파라미터 뒤에 올 필요가 없다. 이 경우 undefined를 명시적으로 전달해야 한다.
```typescript
function buildName(firstName = "Will", lastName: string) {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob"); // Error
let result2 = buildName("Bob", "Adams", "Sr."); // Error
let result3 = buildName("Bob", "Adams"); // Ok
let result4 = buildName(undefined, "Adams") // Ok
```
## **Rest Parameters**
```typescript
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + ' ' + restOfName.join(" ");
}

let employeeName = buildName("Joseph", "Samuel", "Lucas");
let buidlNameFun: (fname: string, ...rest: string[]) => string = buildName;
```
* Rest Parameter는 제한없이 파라미터를 전달할 수 있다.
## **this**
```typescript
let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        return function() {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        };
    }
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);
```
* this 가 window를 바인딩하여 에러를 냄
```typescript
let deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function() {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard / 13);
            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        };
    }
};

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);
```
## **this parameters**
* 위 예시에서 `this.suits[pieckedSuit]`는 여전히 any타입이다. 이를 해결하기 위해서 this를 파라미터로 넘겨줄 수 있다.
```typescript
function f(this: void) {
    // 
}
```
* 이를 응용하여 위의 예에서 반환타입을 명시하는 방법
```typescript
interface Card {
    suit: string;
    card: number;
}
interface Deck {
    suits: string[];
    cards: number[];
    createCardPicker(this: Deck): () => Card;
}
let deck: Deck = {
    suits: ["hearts", "spades", "clubs", "diamonds"],
    cards: Array(52),
    createCardPicker: function(this: Deck) {
        return () => {
            let pickedCard = Math.floor(Math.random() * 52);
            let pickedSuit = Math.floor(pickedCard/13);
            return { suit: this.suits[pickedSuit], card: pickedCard % 13 };
        }
    }
}

let cardPicker = deck.createCardPicker();
let pickedCard = cardPicker();

alert("card: " + pickedCard.card + " of " + pickedCard.suit);
```
## **this paramerts in callbacks**
* callback에서 this 사용시 error를 준다.
```typescript
interface UIElement {
    addClickListener(onClick: (this: void, e: Event) => void): void;
}
```
* this: void 는 이 함수가 this를 요구하지 않음을 의미한다.
```typescript
class Handler {
    info: string;
    onClickBad(this: Handler, e: Event) {
        this.info = e.message; // runtime에 깨짐
    }
}
```
* 화살표 함수를 사용해서 this를 바인딩 할 것!
## **OverLoads**
* 다른 언어의 오버로드와 다름
```typescript
let suits = ["hearts", "spades", "clubs", "diamonds"];

function pickCard(x: { suit: string; card: number }[]): number;
function pickCard(x: number): { suit: string; card: number };
function pickCard(x): any {
    if (typeof x == "object") {
        let pickedCard = Math.floor(Math.random() * x.length);
        return pickedCard;
    }
    else if (typeof x == "number") {
        let pickedSuit = Math.floor(x/13);
        return { suit: suits[pickedSuit], card: x % 13 };
    }
}

let myDeck = [
    { suit: "diamonds", card: 2 },
    { suit: "spades", card: 10 },
    { suit: "hearts", card: 4 }
];

let pickedCard1 = myDeck[pickCard(myDeck)];
let picekdCard2 = pickCard(15);