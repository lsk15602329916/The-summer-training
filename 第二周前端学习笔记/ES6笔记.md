### 1.面向过程与面向对象

- 面向过程：分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用时一个一个依次调用（分析步骤，按照步骤解决问题）。


- **面向对象**：把事物分解成一个一个对象，对象之间分工合作（以对象功能来划分问题，不是按照步骤）。

**面对对象特性**：
- **封装性**
- **继承性**
- **多态性**

面向对象优点：**易维护、易复用、易扩展，系统耦合度更低、更灵活。**

缺点：性能低于面向过程。

**面向对象的思维特点**：
1. 抽取对象共用的属性和行为组织封装成一个类(模板)；
2. 对类进行实例化，获取类的对象。

#### 2.类(class)
- 类抽象了对象的公共部分，泛指某一个大类。
- 对象特指某一个，通过类实例化的一个具体对象。

语法：

    class Person {
        constructor(name,age) {  //参数
            this.name = name;
            this.age = age;
        }  //属性
        say() {
            console.log(this.name + '你好')；
        }  //方法
    }

#### 3.继承(ES6)
继承：子类可以继承父类的一些**属性和方法**(extends关键字)。

语法：

    class Father {
    }//父类
    class Son extends Father {
    }//子类继承父类
    
super关键字：用于访问和调用对象父类上的函数。可以调用父类构造函数和普通函数。例：

    class Father {
        say() {
            return '我是爸爸'
        }
    }//父类
    class Son extends Father {
        say(){
           console.log(super.say() + '的儿子'); //super.say() 就是调用父类中的普通函数 say()
        }
    }//子类继承父类
    var son = new Son();
    son.say(); //我是爸爸的儿子
    
**注意**：
- 继承中的属性或者方法查找原则遵循就近原则（通过实例化子类输出方法，先看子类有没有这个方法，有就先执行子类的方法，没有再去查找父类有没有这个方法，有就执行父类的方法）；
- ES6中类没有变量提升，所以必须先定义类，才能通过类实例化对象；
- 类里面的共有属性和方法一定要加this使用；
- constructor里面的this指向实例对象，方法里面的this指向这个方法的调用者。

#### 4.构造函数
- **构造函数**是一种特殊的函数，主要用来**初始化对象**（为对象成员变量赋初始值，通常与new一起使用）。
- 对象中一些**公共属性和方法**可以抽取出来封装到构造函数里。

**new在执行时会做四件事情**：
1. 在内存中创建一个新的空对象；
2. 让this指向这个新的对象；
3. 执行构造函数里的代码，给这个新对象添加属性和方法；
4. 返回这个新对象(所以构造函数里面不需要 return )。

#### 5.构造函数原型prototype
构造函数通过原型分配的函数是所有对象所**共享**的。

**原型是一个对象，作用是共享方法**。

##### 对象原型__proto__
对象都有一个属性__proto__指向构造函数的prototype原型对象。作用是**使我们对象可以使用构造函数prototype原型对象的属性和方法**。

- 原型对象prototype和__proto__对象原型是等价的；
- __proto__对象意义在于为对象的查找机制提供一个方向，但是它是一个**非标准属性**，因此实际开发中，不可以使用这个属性。

基本模式：

    function SuperType(){
        this.property = true;
    }
    SuperType.prototype.getSuperValue =     function(){
        return this.property;
    };
    function SubType(){
        this.subproperty = false;
    }
    //继承了 SuperType
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function (){
        return this.subproperty;
    };
    var instance = new SubType();
    alert(instance.getSuperValue()); //true
    
##### 原型链：![微信图片_20200726002654](D7C42078F8854CAFBA3DBF336C415AD7)

#### 6.扩展内置对象
通过原型对象，对原来的**内置对象**进行扩展自定义的方法，比如给数组增加求和功能。

**注意**：数组和字符串内置对象不能给原型对象覆盖操作Array.prototype = {}，只能是Array.prototype.xxx =function() {}的方式。

#### 7.继承(ES5)
**ES5继承**（无extends继承）：**构造函数+原型对象**模拟实现，也称为**组合继承**。

###### call()方法：
语法：

    fun.call(thisArg , arg1 , arg2 , ...)

- thisArg：当前调用函数this的指向对象；
- arg1 , arg2：传递的其他参数。

不传递后面参数则只改变函数运行时this的指向。

利用父构造函数继承属性与继承方法：

    function Father {
        this.uname = uname；
        this.age = age;
    }
    Father.prototype.money = function() {
        //函数内容
    }
    function Son(uname, age, score){
        // this 指向子构造函数的对象实例
        Father.call(this, uname, age); //继承属性
        this.score = score;
    }
    //Son.prototytpe = Father.prototype; 错误继承方法，会改变父构造函数的对象
    Son.prototype = new Father(); //继承方法
    var son = new Son('林乐涛'， 20， 10000);
    console.log(son);
    
继承方法的思路：
![继承方法](F1ED944B34664E5081003E2568623B22)
    
#### 8.类的本质：
1. class本质还是function.


2. 类的所有方法都定义在类的prototype属性上

3. 类创建的实例,里面也有__proto__指向类的prototype原型对象

4. 所以ES6的类它的绝大部分功能, ES5都可以做到,新的lass写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

5. ES6的类其实就是语法糖.

6. 语法糖:语法糖就是-一种便捷写法.简单理解,有两种方法可以实现同样的功能但是一种写法更加清晰、方便,那么这个方法就是语法糖


#### 9.数组方法
迭代(遍历)方法: **forEach()**、map()、 **filter()**、**some()**、 every();

所含参数：
- currentValue :数组当前项的值
- index :数组当前项的索引
- arr :数组对象本身。

1. **forEach()方法**：遍历数组


    array.forEach (function (currentvalue, index, arr) )



2. **filter()方法**：创建一个新的数组,新数组中的元素是通过检查指定数组中符合条件的**所有元素**,主要用于筛选数组。语法：


    array.filter (function (currentvalue, index, arr))
    
3. **some()方法**：用于检测数组中的元素是否满足指定条件（查找数组中是否有满足条件的元素）。语法：


    array.some (function (currentvalue, index, arr))

注意：
- 返回值是布尔值，如果查找到这个元素，就返回true,如果查找不到就返回false。
- 如果找到第一个满足条件的元素，则终止循环不在继续查找。

#### 10.对象方法
**Object.defineProperty()**：定义对象中新属性或修改原有的属性。语法：

    object.defineProperty(obj, prop, descriptor)

- obj:必需。目标对象
- prop :必需。需定义或修改的属性的名字
- descriptor:必需。目标属性所拥有的特性

**descriptor**:以 **对象形式{}** 书写，一共有以下四个属性：
- value: 设置属性的值默认为undefined；
- writable: 值是否可以重写。true | false默认为false；
- enumerable: 目标属性是否可以被枚举。true | false默认为false；
- configurable: 目标属性是否可以被删除或是否可以再次修改特性true | false默认为false。

#### 11.严格模式
**1.严格模式**：JavaScript除了提供正常模式外,还提供了严格模式(strict mode)。ES5的严格模式是采用具有限制性JavaScript变体的一种方式,即在严格的条件下运行JS代码。

严格模式对正常的JavaScript语义做了一些更改 :

(1). 消除了Javascript语法的一些不合理、不严谨之处,减少了一些怪异行为。

(2). 消除代码运行的一些不安全之处,保证代码运行的安全。

(3). 提高编译器效率,增加运行速度。

(4). 禁用了在ECMAScript的未来版本中可能会定义的一些语法,为未来新版本的Javascript做好铺垫。比如一些保留字如: class, enum, export, extends, import, super不能做变量名

**2.开启严格模式**：
严格模式可以应用到整个脚本或个别函数中。因此在使用时,我们可以将严格模式分为**为脚本开启严格模式**和**为函数开启严格模式**两种情况。

开启严格模式,需要在所有语句之前放一个特定语句 **"usestrict" ;( 或'usestrict' ;)**  。

(1).**为脚本开启严格模式**

有的script基本是严格模式,有的script脚本是正常模式,这样不利于文件合并,所以可以将整个脚本文件放在-一个立即执行的匿名函数之中。这样独立创建一个作用域而不影响其他script脚本文件。例：

    <script>
        (function () {
            "use strict"; //开启严格模式语句
            var num = 10;
            function fn() {}
        }()
    </script>

(2).**为函数开启严格模式**

在函数内部开启严格模式，只需要在函数作用域内部的最前面放特定语句。

**3.严格模式中的变化**

严格模式对Javascript的**语法和行为**,都做了一些改变。

(1).**变量规定**
- 在正常模式中,如果一个变量没有声明就赋值,默认是全局变量。严格模式禁止这种用法,变量都必须**先用var命令声明,然后再使用**。
- **严禁删除已经声明变量**。例如, deletex;语法是错误的。

(2).**严格模式下this指向问题**
- 以前在全局作用域函数中的this指向window对象。
- **严格模式下全局作用域中函数中的this是undefined.**
- 以前构造函数时不加new也可以调用,当普通函数, this 指向全局对象
- 严格模式下,如果构造函数不加new调用, this会报错.
- **new实例化的构造函数指向创建的对象实例。**
- **定时器this还是指向window**。
- 事件、对象还是指向调用者。

(3).函数变化
- 函数不能有重名的参数。
- 函数必须声明在顶层新版本的JavaScript会引入"块级作用域”(ES6中已引入)。为了与新版本接轨，**不允许在非函数的代码块内声明函数**。

#### 12.浅拷贝和深拷贝
1. 浅拷贝只是拷贝一层，更深层次对象级别的只拷贝引用.
2. 深拷贝拷贝多层,每一级别的数据都会拷贝.
3. Object.assign(target .. sources) es6 新增方法可以浅拷贝

**深拷贝函数封装**：


    //封装函数
    function deepCopy(newobj, o1dobj) {
        for (var k in oldobj) {
            //判断我们的属性值属于那种数据类型
            // 1.获取属性值 oldobj[k]
            var item = oldobj[k]; 
            //2.判断这个值是否是数组
            if (item instanceof Array) {
                newobj[k] = [];
                deepCopy(newobj[k], item)
            } else if (item instanceof Object) {
                // 3.判断这个值是否是对象
                newobj[k] = {};
            deepCopy(newobj[k], item)
        } else {
            // 4.属于简单数据类型
            newobj[k] = item;
    }

**注意**：必须先判断是否对象类型再判断是否数组类型，因为数组也是对象类型。

#### 13.正则表达式
**1.正则表达式**：用于匹配字符串中字符组合的模式。**在JavaScript中,正则表达式也是对象**。

正则表通常被用来**检索、替换**那些符合某个模式(规则)的文本,例如验证表单:用户名表单只能输入英文字
母数字或者下划线，昵称输入框中可以输入中文(匹配)。此外,正则表达式还常用于过滤掉页面内容中的一
些敏感词(替换),或从字符串中获取我们想要的特定部分(提取)等。

**2.正则表达式的特点**

(1).**灵活性、逻辑性和功能性**非常的强。

(2).可以迅速地用**极简单**的方式达到字符串的复杂控制。

(3).实际开发一般都是直接复制写好的正则表达式,但是要求会使用正则表达式并且根据实际情况修改正则表达
式。比如用户名:  /^ [a-z0-9_-]{3,16}$/ 

**3.边界符**

正则表达式中的边界符(位置符)用来提示字符所处的位置,主要有两个字符。


边界符 | 说明
---|---
^ | 表示匹配行首的文本(以谁开始)
$ | 表示匹配行尾的文本(以谁结束)

如果^和$在一起,表示必须是**精确匹配**。

**4.量词符**

量词符用来设定某个模式出现的次数。

量词 | 说明
---|---
* | 重复零次或更多次
+ | 重复- -次或更多次
? | 重复零次或- -次
{n} | 重复n次
{n,} | 重复n次或更多次
{n,m} | 重复n到m次

**5.括号总结**

1. 大括号量词符:里面表示重复次数
2. 中括号字符集合:匹配方括号中的任意字符.
3. 小括号:表示优先级

在线测试网址: https://c.runoob.com/

**6.预定义类**

预定义类指的是某些常见模式的简写方式。常见预定义符如下：

预定类 | 说明
---|---
\d | 匹配0-9之间的任一数字, 相当于[09]
\D | 匹配所有0-9以外的字符，相当于[^0-9]
\w | 匹配任意的字母、数字和下划线，相当于[A-Za-z0-9_ ]
\W | 除所有字母、数字和下划线以外的字符,相当于[^A-Za-z0-9_ ]
\s | 匹配空格(包括换行符、制表符、空格符等)，相等于[ \trln\v\f]
\S | 匹配非空格的字符，相当于[^ \tlrln\wf]

**7.正则表达式参数**

语法：

    /表达式/[sl:itch]
    
switch(也称为修饰符)按照什么样的模式来匹配.有3种值:
- g:全局匹配
- i:忽略大小写
- gi:全局匹配+忽略大小写

**8.表单验证例子**：

    window . onload = function() {
    var regtel = /^1[3|4|5|7|8]\d{9}$/; //手机号码的正则表达式
    var regqq F /^[1-9]\d{4,}$/; // 10000
    var tel = document . querySelector( ' #tel' );
    //表单验证的函数
        function regexp(ele, reg) {
            ele.onblur = function() {
                if (reg. test(this.value)) {
                    // console.log( '正确的');
                    this. nextElementSibling.className = ' success';
                    this . nextElementSibling . innerHTML = '<i class="success_icon"></i>恭喜您输入正确';
                } else {
                    // console.1og('不正确');
                    this . nextElementSibling. className =error ' ;
                    this.nextElementSibling. innerHTML = '<i class="error_ icon"></i> 手机号码格式不正确，请重新输入';
                }
            }
        }
    }

#### 14.解构赋值
ES6中允许从数组中提取值,按照对应位置,对变量赋值。对象也可以实现解构。

##### 1.数组解构

    let[a,b,c]=[1,2,3];
    console.1og(a)  //1
    console.log (b)  //2
    console.1og (c)  //3
    
如果解构不成功，变量的值为undefined.

    let [foo] = [] ;
    let [bar, foo] = [1];  //1 , undefined
    
##### 2.对象解构

    let person = { name: ' zhangsan'，age: 20 };
    let { name, age } = person;
    console .1og (name) ; // ' zhangsan '
    console .1og(age); // 20
    let {name: myName, age: myAge} = person; // myName myAge属于别名
    console .1og (mykame) ; // ' zhangsan '
    console .1og (myAge) ; // 20

#### 15.箭头函数
ES6中新增的定义函数的方式。

    () => {}
    const fn = () => {}
    
函数体中只有一句代码， 且代码的执行结果就是返回值，可以省略大括号和返回值：

    function sum (num1, num2) {
    return numl + num2;
    }
    const sum = (numl, num2) => numl + num2;
    //在箭头函数中，如果形参只有一个形参外侧的小括号也是可以省略的
    const fn = v => {
    }
    
箭头函数不绑定this关键字，箭头函数中的this,指向的是函数定义位置的上下文this（this总是指向词法作用域）.

    const obj = { name: '张三 '}
    function fn () {
        console. log(this) ;
        return () => {
            console.1og (this)
        }
    }
    //由于this在箭头函数中已经按照词法作用域绑定了，所以，用call()或者apply()调用箭头函数时，无法对this进行绑定，即传入的第一个参数被忽略：
    const resFn = fn.call(obj) ; //忽略
    resFn() ;


#### 16.剩余参数
剩余参数语法允许我们将一个不定数量的参数表示为一个数组。

    function sum (first， ...args) {
    console.log (first) ; // 10
    console.log(args) ; // [20， 30]
    sum(10，20，30)


#### 17.Array的扩展方法
- 扩展运算符可以将数组或者对象转为用逗号分隔的参数列。


    let ary=[1, 2, 3];
    . . .ary //1, 2，3
    console.log(.. .ary) ;// 1 2 3

- 扩展运算符可以应用于合并数组。


    //方法一
    let ary1 = [1, 2，3];
    let ary2 = [3，4, 5];
    let ary3 = [...ary1, ...ary2] ;
    //方法二
    ary1.push(...ary2) ;
    
- 将类数组或可遍历对象转换为真正的数组


    let oDivs = document.getElementsByTagName ('div') ;
    oDivs = [. . .oDivs] ;
    
- 将类数组或可遍历对象转换为真正的数组（构造函数方法：Array.from())


    let arrayLike = {
        '0': 'a',
        '1': 'b',
        '2': 'c',
        length: 3
    };
    let arr2 = Array.from(arrayLike) ; // ['a', 'b', 'c']
    
Array.from()方法还可以接受第二个参数,作用类似于数组的map方法,用来对每个元素进行处理,将处理后的值放入返回的数组。

    let arrayLike = {
        "O": 1,
        "1": 2,
        "length": 2
    }
    let newAry = Array. from(aryLike, item => item *2) //[2 ,4 ]

- 用于找出第一个符合条件的数组成员, 如果没有找到返回undefined(实例方法: find())
 

    let ary = [{
        id: 1,
        name: '张三'
    }，{
        id: 2,
        name: '李四'
    }] ;
    let target = ary. find((item, index) => item.id == 2) ; //{id: 2,name: '李四'}

- 用于找出**第一个**符合条件的数组成员的位置， 如果没有找到返回-1(实例方法: findIndex())


    let ary = [1, 5，10，15];
    let index = ary. findIndex ((value, index) => value > 9) ;
    console.1og (index) ; // 2

- 表示某个数组是否包含给定的值,返回布尔值。(实例方法: includes())


    [1，2, 3].includes(2) // true 
    [1, 2，3].includes(4) // false
    
#### 18.String的扩展方法
模板字符串可以**解析变量**,也可以**换行**

    let result = {
        name: ' zhangsan'，
        age: 20, 
        sex: '男
    let html =`<div>
        <span>${result.name}</span>
        <span>${result.age}</span>
        <span>${result.sex}</span>
    </div>`;
    
实例方法: **startsWith()** 和**endsWith()**
- startsWith(): 表示参数字符串是否在原字符串的头部，返回布尔值
- endsWith(): 表示参数字符串是否在原字符串的尾部，返回布尔值


    let str = 'Hello world!' ;
    str.startswith('Hello') // true
    str.endswith('!')// true

实例方法: **repeat()**

repeat方法表示将原字符串重复n次，返回一个新字符串。

    'x'.repeat (3)// "xxx"
    'hello'.repeat(2) // "hellohello" 


