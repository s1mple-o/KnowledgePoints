# TypeScript

TypeScript是基于JavaScript的增加类型系统的静态类型、弱类型语言。

**类型系统：**

**静态类型：**编译阶段就能确认每个变量的类型

**弱类型：**支持隐式类型转换

## 基础

### 定义变量

```typescript
function sayHello( person: string){
    return 'Hello, '+ person;
}
let user = 'Tom';
console.log(sayHello(user));
```



### 原始数据类型

| 类型名 | 关键字    |
| ------ | --------- |
| 布尔   | boolean   |
| 数值   | number    |
| 字符串 | string    |
| 空值   | void      |
| 未引用 | null      |
| 未定义 | undefined |
| 任意   | any       |

#### 类型推论

没有指定类型的变量,在定义变量时进行赋值，会依据赋值内容进行推导变量类型。

```typescript
let myName = 'YWT';
myName = 1;
// Error:TS2322:type 'number' is not assignable to type 'string';
```

在定义变量后，进行赋值，都会推断成any类型

```typescript
let myName;
myName = 'YWT';
myName = 110;
//不会报错
```

#### 联合类型

联合类型（Union Types）表示可以是定义的类型中的任意一种

```typescript
//yes
let me :string | number;
me = 'YWT';
me = 18;
//error
me = true;
// Type 'boolean' is not assignable to type 'string | number'.
```

##### 访问联合类型的属性或方法

联合类型所使用的方法必须为联合类型共有方法

```typescript
function getLength(input:string|number):number{
    return input.length
}
//Property 'length' does not exist on type 'string | number'.
//Property 'length' does not exist on type 'number'.
```

联合类型变量被赋值，会根据赋值进行推断。

```typescript
let me:string|number;
me = 'eighty';
//yes 
console.log(me.length);

me = 18;
//error
console.log(me.length)
//Property 'length' does not exist on type 'number'.
```

### 接口

接（interfaces）是对行为的抽象或者对形状（shape）的描述，需要类（classes）进行实现

```typescript
interface Person{
    name:string;
    size:number;
}
let lz:Person = {
    name:'lz',
    size:18
}

//error 不允许缺少属性
let lz:Person = {
    name:'lz',
}
//Property 'size' is missing in type '{ name: string; }' but required in type 'Person'.

//error 不允许增加属性
let lz:Person = {
    name:'lz',
    size:18,
   	age:24
}
//Type '{ name: string; size: number; age: number; }' is not assignable to type 'Person'.
//Object literal may only specify known properties, and 'age' does not exist in type 'Person'.
```

#### 可选属性

有时不需要对接口的形状完全相同，可设置可选属性：

```typescript
interface Person{
    name:string,
    size?:number
}
let lz:Person = {
    name:'lz'
}
```

#### 任意属性

可通过[propName : string] :type设置任意属性

```typescript
interface Person{
    name:string,
    [propName:string]:string
}

let lz:Person ={
    name:'lz',
    department:'Web'
}
//任意属性会与可选属性冲突
interface Person{
    name:string,
    age?:number,
    [propName:string]:string
}

let lz:Person ={
    name:'lz',
    age:24,
    department:'Web'
}

//Property 'age' of type 'number | undefined' is not assignable to 'string' index type 'string'.
//Type '{ name: string; age: number; department: string; }' is not assignable to type 'Person'.
//Property 'age' is incompatible with index signature.
//Type 'number' is not assignable to type 'string'.

interface Person{
    name:string;
    age?:number;
    [propName:string]:string| number | undefined;
}

let lz:Person ={
    name:'lz',
    age:24,
    department:'Web'
}
//可选属性类型被包含于任意属性类型
```

#### 只读属性

可通过readonly定义只读属性

```typescript
interface Person{
    readonly id:number;
    name:string;
    age?:number;
    [propName:string]:any;
}
let lz :Person = {
    id:999,
    name:'lz',
    department:'Web'
}
lz.id = 123;
//Cannot assign to 'id' because it is a read-only property.
```

### 数组类型

数组类型采用【类型+方括号】表示

```typescript
let arr : number[] = [1,2,3,4,5,6];

//设置类型的数组不允许类型不同
let arr : number[] = [1,2,3,'4',5,6]
//Type 'string' is not assignable to type 'number'.
```

#### 数组泛型

可以==Array<elemType>==表示数组

```typescript
let arr: Array<number> = [1,2,3,4,5];
```

#### 接口表示数组

```typescript
interface NumberArr{
	[index:number]:number;
}
let arr ： NumberArr = [1,2,3,4,5,6]
```

#### 类数组

```typescript
function sum(){
    let arg:{
    	[index:number]:number;
    	length:number;
    	callee:Function;
    } = arguments;
}
```

### 函数

通过函数声明（Function Declaration）和函数表达式（Function Expression）定义函数

```typescript
//函数声明
function sum(x:number,y:number):number{
    reruen x + y
}
//函数表达式
let sum = function(x:number,y:number):number{
    return x+y;
}
//typescript中=>与es6中的=>不同，typescript中=>是函数赋值符，左边是输入，右边是输出。
let sum: (x: number, y: number) => number = function (x: number, y: number): number {
    return x + y;
};
```

#### 接口定义函数

通过接口定义的函数可以保证函数参数个数、参数类型、返回值类型不变

```typescript
interface SumMath{
    (x:number,y:number):number;
}
let sum:SumMath;
sum = function(x:number,y:number){
    return x + y;
}
```

#### 可选参数

```typescript
function sum(x:number,y:number,z?:number):number{
    return z?x+y+z:x+y;
}
```

#### 默认参数

```typescript
function sum(x:number = 1,y:number = 2){
    return x + y;
}
```

#### 剩余参数

```typescript
function push(array:any[],...items:any[]){
    items.forEach(function(item){
        array.push(item)
    })
}
```

#### 函数重载

通过重载定义多个函数，函数定义从最前面开始匹配。

```typescript
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
    if (typeof x === 'number') {
        return Number(x.toString().split('').reverse().join(''));
    } else if (typeof x === 'string') {
        return x.split('').reverse().join('');
    }
}
```

### 类型断言

用来手动为一个值指定类型

```typescript
值 as 类型 / <类型>值
```

#### 将一个联合类型断言为其中一个类型

```typescript
//当一个联合类型不确定是那种类型时，只能访问所有类型的共有属性或方法
interface Cat {
    name: string;
    run(): void;
}
interface Fish {
    name: string;
    swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof animal.swim === 'function') {
        return true;
    }
    return false;
}
//Property 'swim' does not exist on type 'Cat | Fish'.
//Property 'swim' does not exist on type 'Cat'.

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

#### 将一个父类断言为更加具体的子类

```typescript
class ServeError extends Error {
    code : number = 0;
}

class HttpError extends Error {
    statusCode : number = 200;
}

function isServeError(error:Error) {
    if(typeof (error as ServeError).code === 'number' ) {
        return true;
    }
    
    return false;
}
```

#### 将任意类型断言为==any==（不推荐）

```typescript
function getCacheData(key: string): any {
    return (window as any).cache[key];
}
```

### 声明文件

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。

- [`declare var`](http://ts.xcatliu.com/basics/declaration-files.html#declare-var) 声明全局变量
- [`declare function`](http://ts.xcatliu.com/basics/declaration-files.html#declare-function) 声明全局方法
- [`declare class`](http://ts.xcatliu.com/basics/declaration-files.html#declare-class) 声明全局类
- [`declare enum`](http://ts.xcatliu.com/basics/declaration-files.html#declare-enum) 声明全局枚举类型
- [`declare namespace`](http://ts.xcatliu.com/basics/declaration-files.html#declare-namespace) 声明（含有子属性的）全局对象
- [`interface` 和 `type`](http://ts.xcatliu.com/basics/declaration-files.html#interface-%E5%92%8C-type) 声明全局类型
- [`export`](http://ts.xcatliu.com/basics/declaration-files.html#export) 导出变量
- [`export namespace`](http://ts.xcatliu.com/basics/declaration-files.html#export-namespace) 导出（含有子属性的）对象
- [`export default`](http://ts.xcatliu.com/basics/declaration-files.html#export-default) ES6 默认导出
- [`export =`](http://ts.xcatliu.com/basics/declaration-files.html#export-1) commonjs 导出模块
- [`export as namespace`](http://ts.xcatliu.com/basics/declaration-files.html#export-as-namespace) UMD 库声明全局变量
- [`declare global`](http://ts.xcatliu.com/basics/declaration-files.html#declare-global) 扩展全局变量
- [`declare module`](http://ts.xcatliu.com/basics/declaration-files.html#declare-module) 扩展模块
- [`/// `](http://ts.xcatliu.com/basics/declaration-files.html#san-xie-xian-zhi-ling) 三斜线指令

### 内置对象

ECMAScript内置对象、DOM与BOM的内置对象（Document、HTMLElement、Event、NodeList...）

## 进阶

### 类型别名

用来给类型创建别名，类型别名常用于联合类型

```typescript
type name = string;
const firstName:name = 'John';
```

### 字符串字面量类型

```typescript
type EventNames = 'click'|'mouseDown'|'mouseUp';
let event:eventNames = 'click'; //pass
event = 'scroll'; //error Argument of type '"scroll"' is not assignable to parameter of type 'EventNames'
```

### 元组

能够存储不同数据类型

```typescript
let tuple:[string,number];
tuple = ['demo',1]; //pass
typle = ['demo']; //error Type '[string]' is not assignable to type '[string, number]'. Property '1' is missing in type '[string]'.
```

