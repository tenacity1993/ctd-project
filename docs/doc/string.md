---
笔试的时候总会遇到string 和 new String相关的问题，汇总一下
---

###关于""创建字符串和new String创建字符串

```javascript
var s1 = 'abc'
var s2 = String('abc')
var s3 = new String('abc')
var s4 = new String('abc')

console.log('s1 == s2', s1 == s2)  //true
console.log('s1 === s2', s1 === s2) // true
console.log('s1 == s3', s1 == s3)  //true
console.log('s1 === s3', s1 === s3) //false
console.log('s2 == s3', s2 == s3)   //true
console.log('s2 === s3', s2 === s3) //false
console.log('s3 == s4', s3 == s4)  //false
console.log('s3 === s4', s3 === s4)  //false
```

浏览器中运行截图如下：

![string](F:\mygit\ctd-project\src\docs\imgs\string.PNG)



上述字符串创建的三种形式，用“”创建是字符串字面量（通过单引号或者双引号定义）和直接调用String（不用new）生成的字符串都是基本字符串（可以理解为两种方式一样），使用new String创建的字符串为字符串对象。

[基本字符串和字符串对象的区别](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)

>JavaScript会自动将基本字符串转换为字符串对象，只有将基本字符串转化为字符串对象后才可以使用字符串对象的方法。当基本字符串需要调用一个字符串对象才有的方法或者查询值的时候（基本字符串没有这些方法），JavaScript会自动将基本字符串转化为字符串对象并且调用相应的方法或执行查询。
>
>当使用eval时，基本字符串和字符串对象也会产生不同结果，eval会将基本字符串作为源代码处理，而字符串对象则被看作对象处理，返回对象。
>
> ```javascript
> s1 = "2 + 2";               // creates a string primitive
> s2 = new String("2 + 2");   // creates a String object
> console.log(eval(s1));      // returns the number 4
> console.log(eval(s2));      // returns the string "2 + 2"
> ```
>
>利用 [`valueOf`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/valueOf) 方法，我们可以将字符串对象转换为其对应的基本字符串。
>
>```javascript
>console.log(eval(s2.valueOf())); // returns the number 4
>```
>
>



