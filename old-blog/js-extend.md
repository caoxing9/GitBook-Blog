# js的继承方式

## 前言

  之前看过js的继承方式,看的时候总觉得自己完全明白了，实际上随着时间的流逝，又忘记了，究其根本，是没有透彻的理解，知识机械地记忆。此外，遗忘也是很正常的学习的一个过程。于是，今日理解一番，记录一番日后就算忘记也可快速想起。

### 原型链继承 

本质是重写原型对象。\
优点：所有引用对象的实例可以共享原型。属性被任何一个实例改变，其他实例的属性也会一同改变。\
缺点：没有私有变量。

```
// 一个应用类型
 function father(){
     this.a = 'fa';
 }
// 添加原型方法
father.prototype.getA = function(){
    return this.a;
};
// 子类 
function son(){
    this.b = 'son'
}
// 通过重写原型继承
son.prototype= new father();
// 修改原型函数方法一定要在重写原型后，否则会断链
son.prototype.getB = function(){
    return this.b
}
var ins = new son();
console.log(ins.getA()) // 'fa'
console.log(ins.getB()) // 'son'
```

### 构造函数继承 

本质是在子类型构造函数内通过call、apply调用父类构造函数。\
优点：构造函数的私有变量不共享，可以向父类传递参数.\
缺点：1.每次通过new方法创建一个新的实例，都是等于重新开辟了一块空间放置，无形之中，增加了内存消耗。\
2.修改构造函数后，此前创建的实例并不能享受到新的属性和方法。

```
 // 父类
  function fa(name){
      this.name = name;
  }
  // 子类
  function son(){
    fa.call(this.'tracy'); //通过call函数传递给父类的方法函数
    this.age = 99; //定义私有变量，不会被共享。
  }
  var ins = new son();
  console.log(ins.name)  // tracy
  console.log(ins.age) //99
```

### 组合继承 

本质是使用原型链方法继承原型方法，使用构造函数创建私有属性。将前两种方法的优点结合。\
优点：即原型链方法和构造函数的优点。\
1.可以定义公共方法和属性。\
2.可以给父类传参数。\
3.可以定义私有属性。\
缺点：无论什么情况都会调用2次父类，性能损耗。

```
// 父类
function fa(name){
    this.name = name
}
// 通过原型链实现方法继承。
fa.prototype.getName = function(){
    return this.name;
};
// 子类
function son(){
  // 第一次调用父类传递参数、继承属性。
  fa.call(this,'tracy');
  this.age = 99; //私有变量
}
// 重写原型 #第二次调用父类
son.prototype = new fa();
// 修正constructor否则是fa类,其实不修改也不影响继承，只是保持son的构造函数是son。
son.prototype.constructor = son;
// 在子类添加原型方法
son.prototype.getAge = function(){
    console.log(this.age);
};
// 实例化
var ins = new son();
console.log(ins.getAge()) //99;
console.log(ins.getName()) //tracy;
```

### 原型继承 

本质是对对象的浅拷贝。\
优点：可以简单让一个对象保持另一个对象的特性。父类可以多次使用。\
缺点：属性是共享的(浅拷贝)\


```
// 创建一个对象
var fa = {
    name:'tracy',
    hobbys:['soccer','football'] //共享变量
}
// 对fa进行浅拷贝
var son = Object.create(fa);
var son = Object.create(fa);

son.hobbys.push(swim)
son1.hobbys.push(run)

console.log(fa.hobbys) // ['soccer','football','swim','run']  //共享变量被改变
```

### 寄生继承 

本质是在原型继承浅拷贝一个对象的基础上，增强该对象\
优点：代码简单。\
缺点：类似于构造函数继承，增强的对象部分，如若修改，前面复制的对象不会变更。

```
function creatObj(obj){
    // 浅拷贝对象 不限于es5语法Object.create
    var clone = Object.create(obj);
    // 对象增强
    clone.say = function(){
        console.log('haha')
    }
    return clone;
}
var fa = {
    name:'tracy',
    hobbys:['soccer','football'] //共享变量
}
var son = creatObj(fa);
son.say(); //haha
```

### 寄生组合继承 

本质是结合组合式继承和寄生继承的优点，借用构造函数继承私有属性，利用原型链继承实现公共方法和属性。\
优点：1.有公共属性和方法写在原型上\
2.可以定义私有属性在构造函数中。\
3.可以向父类传递参数。\
4.避免了组合继承的重复调用父类。\
缺点：1.代码复杂冗长。\
2.需要手动绑定constructor。

```
// 对父类复制一个副本，让子类继承。
function inherit(son,fa){
    var prototype = Object.create(fa.prototype); // 拷贝对象
    prototype.constructor = son; //增强对象
    son.prototype = prototype;   //子类继承
}
// 父类公共属性
function fa(name){
    this.name = 'tracy'
}
// 父类原型公共方法
fa.prototype.getName(){
    console.log(this.name)
}
// 子类构造函数
function son(name.age){
    // 向父类传参
    fa.call(this,name);
    this.age = age;
}
inherit(son,fa);
// 子类添加原型方法
son.prototypr.getAge(){
    console.log(this.age);
}
```

### 7. es6-class继承

本质是个语法糖，美化继承代码。

```
// 父类
class fa{
   constructor(name) {
   this.name = name;
   }
   getName(){
       return this.name;
   }

}
// 子类
class son extends fa{
   constructor(name,age) {
   // 调用父类传参
   super(name);
   this.age = age;
   }
   getAge(){
       return this.age;
   }
   getA(){
       return this.name+':'+this.age;
   }
}
```

## 总结

通过学习继承方法，我明白了任何事物都不是完美的，有时候缺点可能是优点，优点即是缺点。
