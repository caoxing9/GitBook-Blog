# cycle

之前在网上看过很多js的代码，看到过各种各样的循环方式.今天，特地总结下几种循环。

1. for in\
   适合用来循环一个对象，会遍历到原型链。
2. for each(value,index,array) 适合遍历一个数组，不适合对象，且无法使用break、continue中断循环。 value：当前元素。注意，改变value不会改变原数组.\
   index：数组下标\
   array：原数组
3. for of\
   es6新出的方法适合所有数据结构。包括Set和Map结构、字符串。
