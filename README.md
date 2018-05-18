# ES6_Study
  
### 作者：冰红茶  
### 参考博客：[《ECMAScript 6 入门》 阮一峰](http://es6.ruanyifeng.com/#README)   

------    
    
   之前看完《JavaScript权威指南》后，觉得自己有点基础了，没曾料想世界变化得那么快，2015年6月ES6横空出世，带来了很多的新特性和语法糖，为了跟上时代的发展，上手ES6是必须的。我知道阮一峰老师是这一方面的大佬，后面就跟据阮老师的博客做一些自己的学习心得^ _^
  
## 目录

## [一、块级作用域，变量声明，顶层对象与解构](#1)
### [1.1 块级作用域](#1.1)
### [1.2 let和const](#1.2) 
### [1.3 顶层对象](#1.3) 
### [1.4 数组的解构](#1.4)

------ 		
        

<h2 id='1'>一、块级作用域，变量声明，顶层对象与解构</h2>
<h3 id='1.1'>1.1 块级作用域</h3>  

#### 1) 块级作用域 
> - 外层代码块不受内层代码块的影响；
> - let使得块级作用域的出现，使得用来定义作用域的立即执行函数表达式（IIFE）英雄没有用武之地了。
> - ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。但是浏览器为了兼容以前的老代码，还是支持在块级作用域之中声明函数，但函数的声明被提升到块级作用域上上面去了
> - ES6明确规定，函数可以在块级作用域中声明函数，但是该函数的声明行为类似于let，不能在块级作用域之外引用。但是真正到ES6浏览器的时候，执行的时候是首先在块级作用域前面做一个不带赋值的声明（即undefined），然后再在块级作用域中进行定义
> - 如果实在要用的话，最好采用函数表达式的方式，及把声明和定义都放在一起，而且还要把大括号显式写出来
        
                if(true) {
                    var f = function() {}
                }

        
<h3 id='1.2'>1.2 let和const</h3>  
        
#### 1) let 
> - 使变量作用域限制在块级作用域内，包括循环体；
> - 时间逆流，让函数作用在刚发生的瞬间；
> - 原因[“上面代码中，变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算。”](http://es6.ruanyifeng.com/#docs/let)
> - 使声明提前作用失效，变量的声明必须要在使用之前；
> - “暂时性死区”（temporal dead zone，简称 TDZ），在块级作用域中如果改变全局变量的值，后面又把这个全局变量前面加上let，那么由于let会使得该变量绑定在该块级作用域中，那么此时改变变量的值就是错误的，究其根本还是提前声明失效的问题，在块级作用域内let声明之前都属于死区，typeof也不能提前用
> - let不允许在相同作用域内，重复声明同一个变量。重复声明要加大括号
> - 应用场景：避免用来计数的循环变量泄露为全局变量；避免内层变量可能会覆盖外层变量。
#### 2) const  
> - const的出现需要变量立即赋值，而且一旦赋值后面就再也不能改变值的大小
> - 如果只声明不赋值就会报错
> - 而且const的作用域跟let一样，只在块级作用域中起作用
> - 变量声明也不提升，存在暂时性死区
> - const保证的不是值的不变，而是引用地址的不变
> - 所以不能通过把一个数组赋值给一个常量数组，但是赋值给一个普通数组则是可以的
<h3 id='1.3'>1.3 顶层对象</h3>  

#### 1) window对象和global对象
> - window对象是浏览器等用户代理的顶层对象，而global对象则是像Node等客户端的顶层对象
> - 顶层变量无法在编译时报出变量未声明的错误，理论上window对象是不应该跟global对象挂钩的
> - ES6出现的let，const和class命令也是为了适应这种脱离顶层变量的趋势
#### 2) 如何取得顶层对象
> - 浏览器里面，顶层对象是window，但 Node 和 Web Worker 没有window。
> - 浏览器和 Web Worker 里面，self也指向顶层对象，但是 Node 没有self。
> - Node 里面，顶层对象是global，但其他环境都不支持。
> - 你说用this嘛，但是this在严格模式下会返回undefine，普通模式下在Node 模块和 ES6 模块中，this返回的是当前模块。
> - 不管是严格模式，还是普通模式，new Function('return this')()，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全政策），那么eval、new Function这些方法都可能无法使用。
> - 这就非常的awkward了
        
                // 方法一
                // 首先判断浏览器环境，然后判断Node环境，最后判断其他环境
                (typeof window !== 'undefined'
                   ? window
                   : (typeof process === 'object' &&
                      typeof require === 'function' &&
                      typeof global === 'object')
                     ? global
                     : this);

                // 方法二
                var getGlobal = function () {
                  if (typeof self !== 'undefined') { return self; }
                  if (typeof window !== 'undefined') { return window; }
                  if (typeof global !== 'undefined') { return global; }
                  throw new Error('unable to locate global object');
                }; 
<h3 id='1.4'>1.4 数组的解构</h3>  

#### 1) 简介与注意事项
> - 解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
> - 变量声明语句过程中避免使用圆括号，否则会报错 
> - 函数参数也属于变量声明，因此不能带有圆括号。
> - 赋值语句的模式也不能使用圆括号，所谓赋值语句的模式，其实就是等号左边的部分
        
                // 量声明语句报错
                let [(a)] = [1];
                let {x: (c)} = {};
                let ({x: c}) = {};
                let {(x: c)} = {};
                let {(x): c} = {};
                let { o: ({ p: p }) } = { o: { p: 2 } };

                // 函数参数报错
                function f([(z)]) { return z; }
                function f([z,(x)]) { return x; }

                // 赋值语句的模式报错
                ({ p: a }) = { p: 42 };
                ([a]) = [5];
> - 可以使用圆括号的情况只有一种：赋值语句的非模式部分，可以使用圆括号。
        
                [(b)] = [3]; // 正确
                ({ p: (d) } = {}); // 正确
                [(parseInt.prop)] = [3]; // 正确
> - 
#### 2) 解构数组
> -  其实是一种遍历的变量赋值，默认值是undefined，如果赋值的是null，则默认值就不会生效
        
                var value = [100, 200, 300];
                let [one, two, three] = value;
                console.log(`${one}, ${two}, ${three}`);
                16:20:01.337 VM1269:3 100, 200, 300

                //交换变量
                var a = "A";
                var b = "B";
                let [a,b] = [b,a];
                16:36:57.829 (2) ["B", "A"]
> - 右边的值必须要具备Iterator接口，或者转换为对象后具备Iterator接口，否则会报错
        
                // 报错 转换为对象后不具备Iterator接口
                let [foo] = 1;
                let [foo] = false;
                let [foo] = NaN;
                let [foo] = undefined;
                let [foo] = null;
                // 没有Iterator接口
                let [foo] = {};
> - 惰性求值 表达式只有在用到的时候才会生效，左括号里面赋值应该是默认值
        
                // 完全没有赋值的效果
                var f = function() {
                    console.log("立即生效");
                }
                var g = function() {
                    console.log("缓慢生效");
                }
                let [y=f()] = [g()];
                console.log(y);
                10:50:31.868 VM1788:5 缓慢生效
                10:50:31.869 VM1788:2 立即生效
                10:50:31.869 VM1788:8 undefined

                // 函数压根不生效
                var f = function() {
                    console.log("立即生效");
                }
                var g = function() {
                    console.log("缓慢生效");
                }
                let [y=f()] = [g];
                console.log(y);
                10:48:05.518 VM1701:8 ƒ () {
                    console.log("缓慢生效");
                }
#### 3) 解构对象
> - 跟据同名属性赋值 默认值是undefined，如果赋值的是null，则默认值就不会生效 
        
                var value = {father: "lv", mother: "zhong"};
                var{father, mother} = value;
                console.log(`${father}, ${mother}`);
                16:28:17.278 VM1376:3 lv, zhong 
#### 4) 解构字符串
> - 字符串也相当于一个数组
        
                const [a, b, c, d, e] = 'hello';
                a // "h"
                b // "e"
                c // "l"
                d // "l"
                e // "o"

                //字符串也有类似数组的length属性
                let {length : len} = 'hello';
                len // 5
#### 5) 解构数值和布尔值
> - 解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
        
                var {toString: x} = 123;
                console.log(x === Number.prototype.toString);

                var {toString: y} = true;
                console.log(y === Boolean.prototype.toString); 

                var {name: z} = function() {};
                console.log(z === Function.prototype.name);

                11:08:05.995 VM1923:2 true
                11:08:05.995 VM1923:5 true
                11:08:05.995 VM1923:8 true