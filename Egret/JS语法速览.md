---
title: JS语法速览
categories: JavaScrip 
---

通常，我们学习一门编程语言，应该从“通性”和“特性”两个面下手。所谓通性，既做为编程语言所必需有的，如数据类型和变量、循环和判断、数据结构和面向对像，这些都大同小异；所谓特性，就是这门语言所独有的东西，也是能否学好这个门编程语言的关键，如OC的协议和动态特性、C++的友元和多继承、Lua的元表。下面，将会从通性和特性现两个方面进行JavaScrip的快速学习。

# JavaScrip通性：

### 1 数据类型和变量

##### 1.1 数据类型

- 字符串（String）
- 数字（Number）
- 布尔（Boolean）
- 数组（Array）
- 对象（Object）
- 空（Null）
- 未定义（Undefined）

示例：

    // 字符串
    var carname = "Volovo XC60";
    var carname = 'Volovo "XC60"';

    // 数字
    var x1 = 34.00;

    // Boolean
    var x = true;

    // 数组
    var cars = new Array();
    car[0] = "SS";
    car[0] = "SSS";

    var cars = ["SS", "SSS"];

    // 对象
    var person = {
        firstname : "John",
        lastname  : "Doe",
        id        : 5566
    };

    name1 = person.lastname;
    name2 = person["lastname"];

    // 空（可以通过将变量的值设置为null来清空变量）
    cars = null;
    person = null;

    ++a，加1后再使用
    a++, 使用后再加1

（1）字符串

字符串的加法是把各个字符串连接在一起，生成一个新的额对象；
字符串与基本数据类型相加，会把这个基本数据类型转成字符串后再生成新的字符串上。

    var str = "HelloWorld!!!!";

    // 字符串的长度
    console.log(str.length);

    // 返回子串第一次在字符串出现的位置，如果没有返回-1
    var index = str.indexOf("World");

    // 替换字符串（重新生成一个字符串对象）
    var tem2 = str.replace("World", "WORLD");

    // 字符串大小写转换 toLowerCase， toUpperCase (重新生成一个新的字符串对象)
    var tem3 = str.toUpperCase();


（2）数组

数组里面可以存不同的类型；

    var array_num = [1, 2, 3];
    // 求数组的长度
    console.log(array_num.length);

    // 数组的遍历()
    for(var key in array_num)
    {
        console.log(key + "--------->" + array_num[k]);
    }

    // 队尾插入元素
    array_num.push(5);

    // 查找元素索引
    array_num.indexOf(5);

    // 删除数组第三个数
    array_num.splice(2, 1)；

    // 数组的排序，返回大于0的数则交换lhs和rhs的位置
    array_num.sort(function(lhs, rhs) {
        if (lhs < rhs) {
            return -1;
        } else if (lhs > rhs) {
            return 1;
        } else {
            return 0;
        }
    });

（3）表

    var student = {
        xiaoming : 1,
        xiaohong : 2,
        xiaotian : 3,
    };

    // 表的遍历
    for(var key in student) 
    {
        console.log(key, student[key]);
    }

    // 删除表里的某一项数据
    delete student["xiaoluo"];

（4）对象


- 如果一个变量没有定义或没有赋值，undefine
- 如果一个变量不保存任何数据，初始化为null

##### 1.2 变量

- JavaScrip变量均为对象，当声明一个变量的时候就创建了一个对象。
- 当你声明五变量的时候，可以使用关键词“new”来声明其类型。

示例：

    var carname = new String;
    var x       = new Number;
    var y       = new Boolean;
    var cars    = new Array;

##### 1.3 数据类型转换

（1）强制转换

强制转换主要指使用Number、String、Boolean三个构造函数，手动对各种类型的值进行转换。

    // Number 
    Number('324')        // 324
    Number('325abc')     // NaN 如果不可以解析为数值，返回NaN
    Number('')           // 0 空字符串转换为0
    Number(true)         // 1
    Number(false)        // 0
    Number(undefined)    // NaN
    Number(null)         // 0
    
    // Number是整体转换字符串，parseInt逐个解析字符
    parsenInt('42 cats') // 42
    Number('42 cats')    // NaN

    // Number方法的参数是对象时，将返回NaN，除非量单个数值的数组
    Number({a:1})        // NaN
    Number([1, 2, 3])    // NaN
    Number([5])          // 5

    // Boolean
    Boolean("")            // false
    Boolean("hello")       // true  非空字符串
    Boolena(0)             // false
    Boolean(50)            // true  非零数字
    Boolean(null)          // false
    Boolean(new object())  // 对象

（2）自动转换

### 2 


### 函数


##### 闭包

闭包是一个函数引用另一个函数的变量，因为变量被引用着，所以不会释放，因此可以用来封装一个私有变量。这是优点，也是缺点。
或者讲闭包就是子函数可以使用父函数的局部变量和父函数的参数。



### 面向对象

##### 代码模块

（1）js代码可以放在不同的文件里，称为代码模块

（2）一个模块需要引用其他模块代码的时候，可以使用require

（3）require

- 如果是第一次调用，那么就加载并执行这个文件里面的js指令，返回module.export；
- 每个代码模块由module.export导出的对象；
- 每次require的时候，都返回module.exports;

##### this

    // 显式传递this
    function test_func(name, sex){
        this.name = name;
        this.sex = sex;
    };

    var xiaoming = {};
    test_func.call(xiaoming, "xiaoming", 10)
    console.log(xaioming);

    // 隐式的传递this
    var xiaohong = {
        name : "xiaohong",
        test_func : function() {
            console.log(this);
        }
    }

    xiaohong.test_func();   // 将xiaohong这个对象隐式的传递给了这个函数

    // 强制传递this
    var func = function() {
        console.log(this);
    }.bind(xiaohong);       // bind函数在原来的对象的基础上，产生了一个新的对象并返回

    func();

- this的优先级：强制bind > 显式 > 隐式

##### new与构造函数

    // 类和类的实例
    function person(name age) {
        this.name = name;
        this.age = age;
    }

    person.prototype.test_func = function() {
        console.log("person test_func called!!!!");
        console.log(this);
    }

    var p1 = new person("xiaohong", 10);
    var p2 = new person("xiaotian", 12);

new()之后做了哪些事情？

- step1 : 先生成了一个表  var a = {}
- step2 : 向a里面加了 __proto__ :  prototype 键值，函数prototype的浅复制
- step3 : 将a实例做为this，传递给后面的函数
- step4 : 调用这个函数

new的深入分析：

    // 每一个函数对象都有一个prototype变量指向对象
    var func_temp = function(){
        console.log("func_temp", this);
    }
    console.log(func_temp.prototype);

    // prtotype是一个对象，所以可以进去扩充
    func_temp.prototype.test_func = function() {};

    var data = new func_temp();

    // 这里 data.__proto__ 与 func_temp.prototype是相等的
    console.log(data._proto__);
    console.log(func_temp.prototype);

    data.__proto__.test_func();           // 此处函数内部的this是__proto__不是data
    data.tesst_func();                    // 此处函数内部的this就是data，相当于下面：
    data.__proto__.test_func.call(data);  // 把data作为this，传递给data.__proto__对象里面的函数

    // 优先级的使用
    data.test_func = function() {
        console.log("new test_func", this);
    }

    data.test_func()                      // 此处会调用新定义的函数
    // new的新对象，调用函数的时候，首先会在实例的表里面查找，如果没有这样的key，则会到对象的__proto__里面


##### 继承

    // 父类
    var person = functon() {};
    person.prototype.set_name = fucntion(name) {
        this.name = name;
    }

    person.prototype.set_age = funciton(age) {
        this.age = age;
    }

    // 子类
    var Man = function() {};

    // 做法1：
    Man.prototype = person.prototype;         // 不可以，这两个prototype指向同一个对象，如果我扩充了Main的prototype，person的也发生了片尾

    Main.prototype = new Person();

##### 原型引用


# JavaScrip特性：

### 1 动态类型

JavaScripte拥有动态类型，这意味着相同变量可以使用不同类型：

    var x;                      // 现在为undefined类型
    var x = 5;                  // 现在X为数字
    var x = "Jonhn";            // 现在X为字符串



    // while终止本次循环，回到循环判断，一定要注意循环判断变量，在continue不要有死循环；
    i
    var i = 0;
    while(i < 10){
        if (8 == i) {
            i++;
            continue;
        }
        i++;
    }