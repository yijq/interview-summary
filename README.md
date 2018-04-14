## 面试总结
这次(2018/03 - 2018/04)在深圳面试了多家公司，主要是前端方面的，想就面试中遇到的问题进行一次总结，方便自己以后查阅，下面都是面试中比较常见的问题，答案是我自己总结的，如果有疏忽的地方，欢迎提issue。test文件夹中是对于一些问题的测试代码。PS: 还没写完，这几天会慢慢地完善。

### 1. 常见的排序算法
程序员的面试肯定要考一些基本的算法的，虽然算法对于前端来说用到的地方比较少，但在面试中也时常遇到此类的题目。
```javascript
//冒泡排序：比较相邻的两个值，较大的放在后面
function bubbleSort(arr){
    for(let i = 0; i < arr.length - 1; i++){
        for(let j = 0; j < arr.length - 1 - i; j ++){
            if(arr[j] > arr[j + 1]){
                let temp;
                temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;·
            }
        }
    }
    return arr;
}

//快速排序：1.选择一个基准（随便选）2.将小于基准的元素放在左边，将大于基准的元素放在右边 3.对左边和右边的数组递归这个过程
const quickSort = function(arr) {
    if(arr.length <= 1) return arr;
    let pivotIndex = Math.floor( arr.length / 2 );
    let pivot = arr.splice(pivotIndex, 1)[0];//此处改变了原数组
    let left = [];
    let right = [];
    for(let i = 0; i < arr.length; i++) {
        if(arr[i] < pivot) {
            left.push(arr[i]);
        } else {
            right.push(arr[i])
        }
    }
    return quickSort(left).concat([pivot],quickSort(right));//注意需要将之前splice掉的部分concat上去
}

//桶排序: 

//选择排序：

//堆排序：

```

### 2. BFC的概念
块格式化上下文(Block Formatting Context, BFC)是web页面可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素交互限定的区域，下列方式可以创建块格式化上下文：
- 根元素或包含根元素的元素
- 浮动元素（元素的 float 不是 none）
- 绝对定位元素（元素的 position 为 absolute 或 fixed）
- 行内块元素（元素的 display 为 inline-block）
- 表格单元格（元素的 display为 table-cell，HTML表格单元格默认为该值）
- 表格标题（元素的 display 为 table-caption，HTML表格标题默认为该值）
- 匿名表格单元格元素（元素的 display为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 inline-table）
- overflow 值不为 visible 的块元素
- display 值为 flow-root 的元素
- contain 值为 layout、content或 strict 的元素
- 弹性元素（display为 flex 或 inline-flex元素的直接子元素）
- 网格元素（display为 grid 或 inline-grid 元素的直接子元素）
- 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
- column-span 为 all 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。

创建了块格式化上下文的元素中的所有内容都会被包含到该BFC中。
块格式化上下文对浮动定位（参见 float）与清除浮动（参见 clear）都很重要。浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。外边距折叠（Margin collapsing）也只会发生在属于同一BFC的块级元素之间

### 3. CSS实现多行文字居中
这个题目经常被问到，如果之前没有了解，一时半会很难想出解决方法，这里提供几种解决方案
```html
<div class="middle-box">
    <div class="middle-inner">
        <p>
            <span class="suc-tip">
                这里是文字内容，这里是文字内容，这里是文字内容，这里是文字内容
            </span> <br/>
            <span class="suc-link">
                哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈哈
            </span>
        </p>
        <p style="display:none;">
            <span class="suc-tip">
                这里是文字内容，这里是文字内容，这里是文字内容，这里是
            </span>
        </p>
    </div>
</div>
```
```css
/*这种方法的关键是 display:table*/
.middle-box{
    display: table;
}
.middle-inner{
    display: table-cell;
    verticla-align: middle;
}

/*兼容IE*/
<!--[if lt IE 8]>
<style>
.middle-inner {
    position: absolute; 
    top:50%;
}
.middle-inner p {
    position: relative;
    top: -50%;
}
</style>
<![endif]-->

```
```css
/*利用flex布局实现垂直居中，将flex-direction设置为column定义主轴为垂直方向，可能存在兼容问题*/
.middle-inner{
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    aligin-items: center;
}

```
```css
/*利用“精灵元素”(ghost element)技术实现垂直居中，即在父容器内放一个 100%高度的伪元素，让文本和伪元素垂直对齐，从而达到垂直居中的目的*/

.middle-inner{
    positon: relative;
}

.middle-inner::before {
    content: " ";
    display: inline-block;
    height: 100%;
    width: 1%;
    vertical-align: middle;
}

.middle-inner p {
    display: inline-block;
    vertical-align: middle;
    width: 20rem;
}

```
参考：[css居中问题总结](https://www.v2ex.com/t/441154?r=ichiha#reply0)

### 4. AMD和CMD
- AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。
- CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。
- 类似的还有 CommonJS Modules/2.0 规范，是 BravoJS 在推广过程中对模块定义的规范化产出。

这些规范的目的都是为了 JavaScript 的**模块化开发**，特别是在浏览器端的。
目前这些规范的实现都能达成**浏览器端模块化开发**的目的。

区别：
1. 对于依赖的模块，AMD 是提前执行，CMD 是延迟执行。不过 RequireJS 从 2.0 开始，也改成可以延迟执行（根据写法不同，处理方式不同）。CMD 推崇 as lazy as possible.
2. CMD 推崇**依赖就近**，AMD 推崇**依赖前置**。看代码：
```javascript
// CMD
define(function(require, exports, module){
    var a = require('/a');
    a.doSomething();
    //省略100行代码
    var b = require('/b');
    b.doSomething();
})
```
```javascript
//AMD
define(['./a', './b'],function(a, b) {
    a.doSomething();
    //省略100行代码
    b.doSomething();
})
```
3. AMD的api默认是**一个当多个用**，CMD的apiy严格区分，推崇**单一原则**;

参考：https://github.com/seajs/seajs/issues/277 ， https://www.zhihu.com/question/20351507

### 5. import, export, require, module.exports的用法
import与export主要用于前端开发中，仅少数浏览器实现。暂时可以通过webpack，babel转换器使用。require，exports和module.exports主要使用在nodejs中，符合CommonJs规范。
- import : import语句用于导入由另一个模块导出的绑定。具体用法参见[import MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import) ;

- export : export语句用于在创建JavaScript模块时，从模块中导出函数、对象及原始值，以便其他程序能够通过`import`语句来使用它们。具体用法参见[export MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/expor) ;

- require : 用于引入模块，通常用于nodejs中。

### 6. rem适配
rem是根据根元素(html)的font-size来计算大小的，可以用来设置font-size,margin,padding,width,height等的大小，主要用于移动端的适配，原理是根据屏幕的大小来动态地设置根元素地字体大小，从而使不同大小地屏幕有不同大小地字体，常用地方法有：
1. 利用css的media query(媒体查询)来动态地设置根元素地字体大小：
```css
@media (min-device-width : 375px) and (max-device-width : 667px) and (-webkit-min-device-pixel-ratio : 2){
      html{font-size: 37.5px;}
}
```
2. 利用javascript脚本来动态地计算根元素地字体大小：
```javascript
document.getElementsByTagName('html')[0].style.fontSize = window.innerWidth / 10 + 'px';
```

### 7. 前端开发在android和ios中常见的坑
这个需要较多的移动端开发经验才能有一定的总结，我遇到的不多，但这方面的问题也常常被问到，下面是这方面知识的参考
[AlloyTeam移动端常见问题的解决](https://github.com/AlloyTeam/Mars/tree/master/issues)

### 8. 前端页面中1px的问题
前端中1px的问题是指，一直以来我们实现边框的方法都是设置 border: 1px solid #ccc，但是在retina屏上因为设备像素比的不同，边框在移动设备上的表现也不相同：1px可能会被渲染成1.5px, 2px, 2.5px, 3px....，在用户体验上略差，所以现在要解决的问题就是在retina屏幕实现1px边框。主要的解决方案是，window.devicePixelRatio（dpr）的值动态改变viewport的缩放  
参考：[再谈Retina下1px的解决方案](https://www.w3cplus.com/css/fix-1px-for-retina.html)

### 9. git常用命令
git是比较常用的版本控制的工具，类似的还有svn，不过两者平时用的时候都差不多。如果git不是很熟，强烈建议花一段完整的时间看看这个[Git - book](https://git-scm.com/book/zh/v2)，最好跟着实际操作一下，大概一个下午就可以搞定。面试官一般是想了解你在之前的工作中对于版本控制和团队协作的一些经验，关于如何协作，是根据团队的组成和规模来确定的，并不是固定的。我之前的公司比较小，用的模式是，团队的每个成员都有一个自己的开发分支，还有一个总的开发分支develop，个人的开发分支完成一个功能后就合并到develop分支中，对于一些需要临时紧急修改的bug，可以开一个hotfix分支，用于正式发布的分支是master分支。

### 10. 闭包及其应用，构造函数和继承，原型
- 闭包
    闭包是函数和声明该函数的词法环境的组合。详细一点的说法，函数可以访问声明该函数的词法环境中的变量，并且还可以保证函数需要使用的变量不会被销毁。常见的应用有：
```javascript
//用闭包模拟私有方法
var makeCounter = function() {
  var privateCounter = 0;
  function changeBy(val) {
    privateCounter += val;
  }
  return {
    increment: function() {
      changeBy(1);
    },
    decrement: function() {
      changeBy(-1);
    },
    value: function() {
      return privateCounter;
    }
  }  
};

var Counter1 = makeCounter();
var Counter2 = makeCounter();
console.log(Counter1.value()); /* logs 0 */
Counter1.increment();
Counter1.increment();
console.log(Counter1.value()); /* logs 2 */
Counter1.decrement();
console.log(Counter1.value()); /* logs 1 */
console.log(Counter2.value()); /* logs 0 */

//一个常见的错误：循环地为多个DOM元素绑定事件，结果只有最后的一个元素绑定上了，可以利用闭包来创建私有作用域来解决，也可以使用let关键字来解决。
```
详情参见：[闭包 MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)  
- 构造函数
```javascript
//构造函数
function Cat(name, age, color) {
    this.name = name;
    this.age = age;
    this.color = color;
    this.sayHello = function() {
        console.log('Wang wang!')
    };
}


var myCat = new Cat('娘口三三', 1000, 'white');

//关于new关键字执行时发生了什么（这个问题问得很多，JS高级程序设计 里面说的很清楚，建议去看看）
/*
new关键字执行时有4个步骤：
1. 创建一个新对象
2. 将构造函数的作用域赋给新对象（因此this就指向了这个新对象）
3. 执行构造函数中的代码（为这个新对象添加属性）
4. 返回新对象（如果函数中没有return关键字，就返回这个新对象；如果有新对象，就返回return后面的内容）
*/
```

- 原型链与继承
```javascript
/*
原型链
JavaScript只有一种结构：对象。每个对象都有一个私有属性（称为 [[prototype]] ，挂载在 __prototype__ 属性上），它指向它的原型对象（prootype）。该prototype又有一个自己的protoype，层层向上直到一个对象的原型为null，根据定义，null没有原型，并作为这个原型链中的最后一个环节。
*/

/*
基于原型链的继承
JavaScript对象是动态的属性"包"（指其自己的属性），JavaScript有一个指向一个原型对象的链。当试图访问一个对象属性时，它不仅仅在该对象上搜寻，还会搜寻该对象的原型，以及原型的原型，依次层层向上搜索，直到找到一个名字匹配的属性或达到原型链的末端。
*/

// 让我们假设我们有一个对象 o, 其有自己的属性 a 和 b：
// {a: 1, b: 2}
// o 的 [[Prototype]] 有属性 b 和 c：
// {b: 3, c: 4}
// 最后, o.[[Prototype]].[[Prototype]] 是 null.
// 这就是原型链的末尾，即 null，
// 根据定义，null 没有[[Prototype]].
// 综上，整个原型链如下: 
// {a:1, b:2} ---> {b:3, c:4} ---> null

console.log(o.a); // 1
// a是o的自身属性吗？是的，该属性的值为1

console.log(o.b); // 2
// b是o的自身属性吗？是的，该属性的值为2
// 原型上也有一个'b'属性,但是它不会被访问到.这种情况称为"属性遮蔽 (property shadowing)"

console.log(o.c); // 4
// c是o的自身属性吗？不是，那看看原型上有没有
// c是o.[[Prototype]]的属性吗？是的，该属性的值为4

console.log(o.d); // undefined
// d是o的自身属性吗？不是,那看看原型上有没有
// d是o.[[Prototype]]的属性吗？不是，那看看它的原型上有没有
// o.[[Prototype]].[[Prototype]] 为 null，停止搜索
// 没有d属性，返回undefined
```
参考: [MDN 原型与继承](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)


### 11. flex布局和grid布局
这两种比较新的布局方式面试时也问的比较多，其中flex问得最多。  
CSS网格布局(grid布局)和弹性盒布局(flex布局)的主要区别在于弹性盒布局是为一维布局服务的（沿横向或纵向的），而网格布局是为二维布局服务的（同时沿着横向和纵向）。这两个规格有一些相同的特性。  
具体得用法参考：[flex布局教程](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html?^%$),  
[grid布局教程](http://blog.jirengu.com/?p=990),
[grid和flex MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Grid_Layout/Relationship_of_Grid_Layout)

### 12. 浏览器从url输入到渲染成页面经历了哪些过程
这个问题大概有三分之一得面试官问到，我一般是想到什么说什么，比较考察计算机基础的，这里有一个特别详细的版本，可以看看，在脑海中有个印象就可以了：  
[what-happens-when-zh_CN](https://github.com/skyline75489/what-happens-when-zh_CN)

### 13. HTML5的新特性
这个几乎是一个烂大街的问题了，网上有很多人都总结了自己的答案，但都只是二手或以上的资料，看第一手的资料才能对知识有更好的理解，MDN文档献上：
[HTML5 MDN](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/HTML)

### 14. CSS3中的transition, animation

### 15. 对nodejs的理解

### 16. react的生命周期及其优化

### 17. flux架构的状态管理的原理及使用

### 18. vue的生命周期，双向数据绑定，父子组件传值



## 一些具体地题目：
1. 使用原生js定义一个函数，通过正则从cookie中取出key为ivweb。
```javascript
const getIvwebInCookie = () => {
    return document.cookie.replace(/(?:(?:^|.*;\s*)ivweb\s*\=\s*([^;]*).*$)|^.*$/, "$1");
}
```

2. css实现不定宽高的水平垂直居中，兼容IE6。
```css
.center-box {
    position: relative;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

3. 定义一个函数function on(elem, type, handle){}实现事件的绑定，要求至少兼容IE8，并且handle函数的this指向elem。
```javascript
function on(elem, type, handle){
    elem.addEventListener(type, handle.bind(this),false);
    return elem;
}
```

4. 写出tcp三次握手的过程，并画出握手的流程图。






