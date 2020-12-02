## 基础类型

+ `let isDone: boolean = false`--布尔值

+ `let num: number = 6`--数字

+ `let name: string = 'Bob'`--字符串

  + ```typescript
    //模版字符串
    let name: string = `Gene`;
    let age: number = 37;
    let sentence: string = `Hello, my name is ${ name }.
    
    I'll be ${ age + 1 } years old next month.`;
    
    //sentence
    let sentence: string = "Hello, my name is " + name + ".\n\n" +
        "I'll be " + (age + 1) + " years old next month.";
    ```

### 数组

- `let list: number[] = [1, 2, 3]`
- `let list: Array<number> = [1, 2, 3]`--数组泛型

### 元组Tuple

> 元组类型允许表示一个已知元素数量和类型的数组，各元素类型不必相同。

```typescript
let x: [number, string]
x = [10, 'hello'] //ok
x = ['hello', 10] //error
```

### 枚举enum

```typescript
//默认从0开始编号，下例是手动从1开始编号
enum Color {Red = 1, Green, Blue} //等同于 `enum Color {Red = 1, Green = 2, Blue = 3}`
let c: Color = Color.Green;
let colorName: string = Color[2]
console.log(colorName) //"Green"
```

### Any

```typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

#### Any和Object的区别

两种类型都可以赋任意的值，但是不能在Object上调用任意方法。

```typescript
let notSure: any = 4;
notSure.ifItExists(); // okay, ifItExists might exist at runtime
notSure.toFixed(); // okay, toFixed exists (but the compiler doesn't check)

let prettySure: Object = 4;
prettySure.toFixed(); // Error: Property 'toFixed' doesn't exist on type 'Object'.
```

### Void

表示没有任何类型，只能为其赋予undefined或null。

```typescript
// 下例表示该函数没有返回值
function warnUser(): void {
  console.log("This is my warning message");
}
```

### Null和Undefined

和Void类型类似，本身类型用处不大，分别只能被赋值为null和undefined。

默认情况下，这俩类型是所有其他类型的子类型，也就是你可以为number类型的变量赋值null或undefined。

当指定了`--strictNullChecks`标记时，null和undefined只能赋值给void和它们各自。

### Never

该类型表示的是永不存在的值的类型。Never类型是任何类型的子类型，可以赋值给任何类型，但没有类型是Never类型的子类型。

```typescript
// 返回never的函数必须存在无法达到的终点
function error(message: string): never {
    throw new Error(message);
}

// 推断的返回值类型为never
function fail() {
    return error("Something failed");
}

// 返回never的函数必须存在无法达到的终点
function infiniteLoop(): never {
    while (true) {
    }
}
```

### Object

该类型表示非原始类型，即除了umber，string，boolean，symbol，null或undefined之外的类型。

```typescript
declare function create(o: object | null): void;

create({ prop: 0 }); // OK
create(null); // OK

create(42); // Error
create("string"); // Error
create(false); // Error
create(undefined); // Error	
```

### 类型断言

类型断言有两种形式，其一是尖括号语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (<string>someValue).length;
```

另一种为as语法：

```typescript
let someValue: any = "this is a string";

let strLength: number = (someValue as string).length;
```

类型断言不会对运行产生任何影响，它只在编译时起作用。它的作用就是告诉编译器，这个数据是什么类型，让它无需做数据检查和解构。

## 变量声明

typescript是JavaScript的超集，所以它本身就支持`let`和`const`，并且可以解析ES6的解构和展开运算符特性。

## 接口

> typescript的核心原则之一就是为值所具有的结构进行类型检测。接口的作用就是为这些类型命名和为代码定义契约。

```typescript
interface LabelledValue {
  label: string;
}

function printLabel(labelledObj: LabelledValue) {
  console.log(labelledObj.label);
}

let myObj = {size: 10, label: "Size 10 Object"};
printLabel(myObj);
```

### 可选属性

接口中的属性不全是必须的，可选属性的定义就是在定义名后加一个`?`号。

```typescript
interface SquareConfig {
  color?: string;
  width?: number;
}

function createSquare(config: SquareConfig): {color: string; area: number} {
  let newSquare = {color: "white", area: 100};
  if (config.color) {
    newSquare.color = config.color;
  }
  if (config.width) {
    newSquare.area = config.width * config.width;
  }
  return newSquare;
}

let mySquare = createSquare({color: "black"});
```

可选属性的好处之一是，可以对可能存在的属性进行预定义，好处之二是，可以捕捉引用了不存在的属性时的错误。

上例中，如果将`createSquare`中，`if (config.color)`里的`color`拼成了`colr`的话，就会得到一个错误提示`// Error: Property 'clor' does not exist on type 'SquareConfig'`。

### 只读属性

在属性名前加上`readonly`就能将其指定为只读属性，该属性只能在对象刚创建时修改其值。

```typescript
interface Point {
    readonly x: number;
    readonly y: number;
}
```

```typescript
let a: number[] = [1, 2, 3, 4];
let ro: ReadonlyArray<number> = a;
ro[0] = 12; // error!
ro.push(5); // error!
ro.length = 100; // error!
a = ro; // error!
```

上例中不可以把整个`ReadonlyArray`赋值给一个普通数组，但可以用类型断言重写`a = ro as number[];`。

#### readonly和const的区别

作为变量使用的话用const，作为属性使用就用readonly。

#### 额外的属性检查

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
}

function createSquare(config: SquareConfig): { color: string; area: number } {
    // ...
}

let mySquare = createSquare({ colour: "red", width: 100 });
```

上例中将对象字面量作为参数传入或赋值给变量时，会造成额外的属性检查。

> 如果一个对象字面量存在任何“目标类型”不包含的属性时，你会得到一个错误。

可以通过类型断言绕开检查，

```typescript
let mySquare = createSquare({ width: 100, opacity: 0.5 } as SquareConfig);
```

但最佳的方法是添加一个*字符串索引签名*，前提是你能够确定这个对象可能具有某些做为特殊用途使用的额外属性。

```typescript
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any; // SquareConfig可以有任意数量的属性，并且只要它们不是color和width，那么就无所谓它们的类型是什么。
}
```

还有一种绕开检查的方法，是将这个对象赋值给另一个变量。

```typescript
let squareOptions = { colour: "red", width: 100 };
let mySquare = createSquare(squareOptions);
```

### 函数类型

为了使用接口表示函数类型，我们需要给接口定义一个*调用签名*。 它就像是一个只有参数列表和返回值类型的函数定义。参数列表里的每个参数都需要名字和类型。

```typescript
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```

下例展示了如何创建一个函数类型的变量，并将一个同类型的函数赋值给这个变量。

```typescript
let mySearch: SearchFunc;
mySearch = function(source: string, subString: string) {
  let result = source.search(subString);
  return result > -1;
}
```

对于函数类型的类型检查来说，函数的参数名不需要与接口里定义的名字相匹配。函数的参数会逐个进行检查，要求对应位置上的参数类型是兼容的。

如果你不想指定类型，TypeScript的类型系统会推断出参数类型，因为函数直接赋值给了 `SearchFunc`类型变量。 

函数的返回值类型是通过其返回值推断出来的（此例是 `false`和`true`）。 如果让这个函数返回数字或字符串，类型检查器会警告我们函数的返回值类型与 `SearchFunc`接口中的定义不匹配。

```typescript
let mySearch: SearchFunc;
mySearch = function(src, sub) {
    let result = src.search(sub);
    return result > -1;
}
```

### 可索引的类型

可索引类型具有一个 *索引签名*，它描述了对象索引的类型，还有相应的索引返回值类型。

```typescript
interface StringArray {
  [index: number]: string; // 这个索引签名表示了当用 number去索引StringArray时会得到string类型的返回值。
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];
```

TypeScript支持两种索引签名：字符串和数字。可同时使用两种类型的索引。但是数字索引的返回值必须是字符串索引返回值类型的子类型。

```typescript
class Animal {
    name: string;
}
class Dog extends Animal {
    breed: string;
}

// 错误：使用数值型的字符串索引，有时会得到完全不同的Animal!
interface NotOkay {
    [x: number]: Animal;
    [x: string]: Dog;
}
```

上例中会报错是因为，当使用 `number`来索引时，JavaScript会将它转换成`string`然后再去索引对象。 也就是说，当使用number索引时，期望的返回值类型应该是`Animal`，但最终会返回`Dog`。

字符串索引签名能够很好的描述`dictionary`模式，并且它们也会确保所有属性与其返回值类型相匹配。

```typescript
interface NumberDictionary {
  [index: string]: number;
  length: number;    // 可以，length是number类型
  name: string       // 错误，`name`的类型与索引类型返回值的类型不匹配
}
```

可以将索引签名设置为只读，这样就防止了给索引赋值：

```typescript
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // error!
```

