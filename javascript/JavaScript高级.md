# JavaScript高级

## 1.原型

```javascript
//构造函数 Person
function Person(){    
}

//实例tom
let tom = new Person();

//实例原型
Person.prototype

//每个实例拥有一个_proto_属性指向原型
tom._proto_ === Person.prototype //true

//原型拥有一个属性指向构造函数
Person.prototype.constructor === Persom //true

//原型也是一个对象，_proto_指向原型(Object.prototype)
Person.prototype._proto_  === object.prototype //true

//Object.prototype._proto_指向null
Object.prototype._proto_ === null //true
```

### 原型链

![](C:\Users\10465\Desktop\学习\javascript\img\prototype.png)

## 2.继承

### 原型链继承

```javascript
//父
function Parent () {
    this.name = 'Lizhi';
}
//原型链绑定方法
parent.prototype.getName = function (){
    console.log(this.name)
}
//子
function Child (){
}

child.prototype = new Parent();

var child1 = new Child();

console.log(child1.getName());

```

### 构造函数

```javascript
function Parent (){
	this.names = ['Lizhi','Huihui'];
}

function Child (){
    Parent.call(this);
}

var child1 = new Child();

child1.names.push('yaya');

console.log(child1.names)

var child2 = new Child();

console.log(child2.names)
```

