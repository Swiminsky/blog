## 0X00 序


熟悉 JS 数据类型的同学一定知道，null 和 undefined 均属于基本类型。

至于为什么在 JS 中会有两种表示 "无" 的基本类型，这其中有一些历史原因。

## 0x01 历史


1995年 JavaScript 诞生时，最初像 Java 一样，只设置了 null 作为表示 "无" 的值。
根据 C 语言的传统，null 被设计成可以自动转为0。

    Number(null)
    // 0
    
    5 + null
    // 5

但是，JavaScript 的设计者 Brendan Eich，觉得这样做还不够，有两个原因。

 1. null 像在 Java 里一样，被当成一个对象。但是，JavaScript 的数据类型分成原始类型（primitive）和合成类型（complex）两大类，Brendan Eich觉得表示 "无" 的值最好不是对象

 2. 其次，JavaScript 的最初版本没有包括错误处理机制，发生数据类型不匹配时，往往是自动转换类型或者默默地失败。Brendan Eich 觉得，如果 null 自动转为0，不容易发现错误

因此，设计者又设计了一个 undefined 

## 0x02 比较


### UNDEFINED ###

undefined 表示 "缺少值"，就是此处应该有一个值，但是还未定义

**出现场景**

 1. 已声明，未赋值的变量


    var n;
    console.log(n); // undefined
 2. 获取对象不存在的属性


    var obj = {a:1,b:2};
    console.log(obj.c); // undefined
 3. 无返回值的函数的执行结果


    (function(){
      var x = "No return";
    }()) // undefined
 4. 函数的参数未传入时，将传入 undefined 替换


    function  fun(i,j) {
    console.log(i,j);
    }
    fun(1) //1 undefined
 5. void(expression);


    void(1) // undefined

**类型转换**

    Boolean(undefined); // false

    Number(undefined); // NaN

    String(undefined); // "undefined"

### NULL ###
**出现场景 **

null 表示"没有对象"，即该处不应该有值

 1. 对象不存在时

    document.getElementById("notExist"); // null
 2. 正则表达式未匹配时


    var str = "abcdefg";
    str.match(/\d/g); // null
 3. 表示对象原型链的终点


    Object.getPrototypeOf(Object.prototype) // null
    
**类型转换 **

    Boolean(null); // false

    Number(null); // 0

    String(null); // "null"

### 0x03 SUMMARY ###

 1. 至于为什么 JS 中有了 null 又加了 undefined ，其中有一些历史的原因

 2. 虽然 `null == undefined` 返回 true，但是他们是两种不同的原始类型

 3. 两种类型有完全不同的出现场景

 4. 在条件判断时，都会转换为布尔类型 false，在转换为 Number 时，undefined 转换为 NaN ，null 转换为 0
