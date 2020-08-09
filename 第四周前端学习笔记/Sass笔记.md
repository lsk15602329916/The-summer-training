### 一.Sass变量
1. Sass 变量可以存储以下信息：
- 字符串
- 数字
- 颜色值
- 布尔值
- 列表
- null 值

2. Sass 变量使用 $ 符号。例：$btnColor: red

3. 默认变量：可以通过向变量值的结尾添加！default 标志来设置变量的默认值。如果在此之前变量已经赋值，那就不使用默认值，如果没有赋值，则使用默认值。

4. 多值变量：分为list类型和map类型，对应于JavaScript的数组和对象。
- list类型变量：以空格，逗号或者括号（多维数组用括号分割）来分割多个值，可用nth($list, n)函数来取值。
- map类型变量：以key-value成对定义的，其中value值又可以为普通变量或者list变量。格式为：$map(key1: value1, key2: value2, key3: value3)，可用map-get($map, $key)来取值。

5. Sass 作用域:
- Sass 变量的作用域只能在当前的层级上有效果.
- 使用 !global 关键词可以设置全局变量
- 注意：所有的全局变量我们一般定义在同一个文件，如：_globals.scss，然后我们使用 @include 来包含该文件。

6. 嵌套

- 选择器嵌套 语法：


    className{
        property: value;
    }

i. '&'指的是父元素选择器，经常用在筛选伪类选择器中。
ii. 使用嵌套的时候要注意尽量不要超过三层。

- 属性嵌套 语法与选择器嵌套相同。

注意：属性嵌套书写时，一定不要忘了属性后面所跟的**冒号**。

7. 继承
使用@extend命令可让任何元素继承其他样式定义好的属性和值。如果有好几个元素有共同的样式属性，这种情况可使用@extend命令。例：

    
    $colorMain = orange;
    .div {
      background: $colorMain;
      padding: 20px;
    }
    .div-01 {
      @extend .div;
      border: 2px solid #eee;
    }
    .div-02 {
      @extend .div;
      border: 2px solid #ccc;
    }
    
编译后css样式：


    .div, .div-01, .div-02 {
      background: orange;
      padding: 20px;
    }
    .div-01 {
      border: 2px solid #eee;
    }
    .div-02 {
      border: 2px solid #ccc;
    }

- 占位符：占位符与继承@extend命令一起使用，有些情况下，一些代码只是用来做扩展用，就可以用占位符选择器，以此来减少冗余代码。若上面例子中的.div更换为%div，生成的css样式中不会有.div样式。

8. @mixin 与 @include
- @mixin：定义一个可以在整个样式表中重复使用的样式。
- @include：将混入（mixin）引入到文档中。

i. 无参数混合宏(可以直接用继承代替)：

    @mixin center-block {
      margin-left: auto;
      margin-right: auto;
    }
    .div {
      @include center-block;
    }

编译后的css样式：

    .div {
      margin-left: auto;
      margin-right: auto;
    }

ii. 有参数的混合宏:

    @mixin border-radius($radius: 4px) {
      -moz-border-radius: $radius;
      -webkit-border-radius: $radius;
      -o-border-radius: $radius;
      -ms-border-radius: $radius;
      border-radius: $radius;
    }
    .div {
      @include border-radius(5px);
    }
    
编译后的css样式：

    .div {
      -moz-border-radius: 5px;
      -webkit-border-radius: 5px;
      -o-border-radius: 5px;
      -ms-border-radius: 5px;
      border-radius: 5px;
    }
    
iii. 多个参数混合宏

    //在字符串中使用，用#{}包含变量
    @mixin border-direction-radius($direction: top-left, $radius: 4px) {
      -moz-border-#{$direction}-radius: $radius;
      -webkit-border-#{$direction}-radius: $radius;
      -o-border-#{$direction}-radius: $radius;
      -ms-border-#{$direction}-radius: $radius;
      border-#{$direction}-radius: $radius;
    }
    .div {
      @include border-direction-radius(top-left, 5px)
    }

编译后的css样式：

    .div {
      -moz-border-top-left-radius: 5px;
      -webkit-border-top-left-radius: 5px;
      -o-border-top-left-radius: 5px;
      -ms-border-top-left-radius: 5px;
      border-top-left-radius: 5px;
    }

iv. 多组值参数

    //一个参数可以有多组值，如box-shadow，transition等，那么参数则需要在变量后加三个点表示，如：$variables… 。
    @mixin box-shadow ($shadow...){
      -moz-box-shadow: $shadow;
      -webkit-box-shadow: $shadow;
      -o-box-shadow: $shadow;
      -ms-box-shadow: $shadow;
      box-shadow: $shadow;
    }
    .div {
      @include box-shadow(1px 1px 2px 0px gray, 0px 1px 0px 1px lighten(gray, 0.5));
    }

编译后的css样式：

    .div {
      -moz-box-shadow: 1px 1px 2px 0px gray, 0px 1px 0px 1px #818181;
      -webkit-box-shadow: 1px 1px 2px 0px gray, 0px 1px 0px 1px #818181;
      -o-box-shadow: 1px 1px 2px 0px gray, 0px 1px 0px 1px #818181;
      -ms-box-shadow: 1px 1px 2px 0px gray, 0px 1px 0px 1px #818181;
      box-shadow: 1px 1px 2px 0px gray, 0px 1px 0px 1px #818181;
    }
    
9. 计算
- 加法：


    $widthContainer: 1200px;
    $widthLeft: 20%;
    $widthRight: 80%;
    .div {
      width: $widthLeft + $widthRight;
    }
    
编译后的css样式：

    .div {
      width: 100%;
    }
    
- 减法：与加法相似

- 乘法：相乘时，如果值后面**都**带单位，会出现问题。


    .addition {
      width: 20px * 40px; //报错
      width: 20 * 40px;  //正确
    }

- 除法：跟乘法一样，只需要一个值带单位即可，并且表达式放在括号内，才能正常使用。


    .addition {
      width: 80% / 20%; //编译出的css样式无意义（看成字符串形式）
      width: (80% / 20);  //正确编译
    }

10. 函数
- 条件：@if和@else if控制命令


    $div: left;
    .div {
      @if $div == left {
        float: left;
      }@else if $div == right {
        float: right;
      }
    }
    
编译后的CSS样式：

    .div {
      float: left;
    }

- @for循环：两种形式：@for $variable from startNum **to** endNum和@for $variable from startNum **through** endNum，关键字to循环时不包括endNum这个数，through循环时包括endNum这个数。


    @for $i from 1 through 3 {
      .div-#{$i} {
        width: 100px * $i; 
      }
    }
    @for $i from 1 through 3 {
      .p-#{$i} {
        width: 100px * $i; 
      }
    }
    
编译后的css样式：

    .div-1 {
      width: 100px;
    }
    .div-2 {
      width: 200px;
    }
    .div-3 {
      width: 300px;
    }
    .p-1 {
      width: 100px;
    }
    .p-2 {
      width: 200px;
    }


- @each循环：基本语法是：@each $variable in list/map，list/map表示list类型或者map类型变量。

i. 循环list类型变量：

    @each $list in a,b,c {
      .#{$list} {
        background-image: url("img/#{$list}.png");
      }
    }

编译后css样式：

    .a {
      background-image: url("img/a.png");
    }
    .b {
      background-image: url("img/b.png");
    }
    .c {
      background-image: url("img/c.png");
    }
    
多组值的循环：

    $listData: (a, no-repeat, left top) (b, no-repeat, left bottom);
    @each $name, $repeatType, $sizeType in $listData {
      .#{$name} {
        background-image: url("img/#{$name}.png");
        background-repeat: $repeatType;
        background-position: $sizeType;
      }
    }
    
编译后的CSS样式：

    .a {
      background-image: url("img/a.png");
      background-repeat: no-repeat;
      background-position: left top;
    }
    .b {
      background-image: url("img/b.png");
      background-repeat: no-repeat;
      background-position: left bottom;
    }
    
ii. 循环map类型变量

    $headings: (h1: 20px, h2: 16px, h3: 13px);
    @each $header, $size in $headings {
      #{$header} {
        font-size: $size;
      }
    }
   
编译后的CSS样式：

    h1 {
      font-size: 20px;
    }
    h2 {
      font-size: 16px;
    }
    h3 {
      font-size: 13px;
    }
    
- @while指令，与JavaScript的while一样，只要后面的条件为true就会执行。


    $types: 4;
    $type-width: 20px;
    @while $types > 0 {
        .while-#{$types}
        {
            width: $type-width + $types;
        }
        $types: $types - 1;
    }
    
编译后的css样式：

    .while-4 {
      width: 24px;
    }
     
    .while-3 {
      width: 23px;
    }
     
    .while-2 {
      width: 22px;
    }
     
    .while-1 {
      width: 21px;
    }
    
##### 常见字符串函数：
1. unquote（$string）：删除字符串中的引号（无论单双）；

**注意**：只能删除字符串最前边和最后边的引号，没法去掉中间的引号；没有带引号则返回原字符串。

2. quote（$string）：给字符串添加引号；

**注意**：**1**. 若字符串本身带有引号，就不添加；**2**. 若字符串带有单引号，则跟换为双引号；**3**. 若双引号中有单引号，则不变，单引号不受影响；**4**. 若字符串中间有空格或者半块的单引号、双引号时，需要用单引号或双引号括起，不然编译会报错;**5**. 碰到特殊符号，比如： !、?、> 等，除中折号 - 和下划线_ 都需要使用双引号括起，否则编译器在进行编译的时候同样会报错

3. To-upper-case（$string）：将字符串小写字母转换为大写字母
4. To-lower-case（$string）：将字符串大写字母转换为小写字母

##### 常见数字函数:
Sass的数字函数和JavaScript的Math对象方法基本相似。

##### 列表函数：

1. length($list)：返回一个列表的长度值;
2. nth($list, $n)：返回一个列表中指定的某个标签值;
3. join($list1, $list2, [$separator])：将两个列给连接在一起，变成一个列表；
4. append($list1, $val, [$separator])：将某个值放在列表的最后；
5. zip($lists…)：将几个列表结合成一个多维的列表；
6. index($list, $value)：返回一个值在列表中的位置值。

##### Introspection函数：

1. type-of($value)：返回一个值的类型;
2. unit($number)：返回一个值的单位;
3. unitless($number)：判断一个值是否带有单位;
4. comparable($number-1, $number-2)：判断两个值是否可以做加、* 减和合并.

##### 三元条件函数
语法：if($condition,$if-true,$if-false);判断$condition，如果条件成立，则返回$if-true的结果，如果条件不成立，则返回$if-false的结果。

##### Maps的函数：

1. map-get($map,$key)：根据给定的 key 值，返回 map 中相关的值；
2. map-has-key($map,$key)：根据给定的 key 值判断 map 是否有对应的 value值，如果有返回 true，否则返回 false。
3. map-keys($map)：返回 map 中所有的 key。
4. map-values($map)：返回 map 中所有的 value。
5. map-merge($map1,$map2)：将两个 map 合并成一个新的 map。
6. map-remove($map,$key)：从 map 中删除一个 key，返回一个新 map。
7. keywords($args)：返回一个函数的参数，这个参数可以动态的设置 key 和 value。

##### 颜色函数：
具体看 https://www.runoob.com/sass/sass-functions.html