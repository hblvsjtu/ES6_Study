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
## [二、相关拓展](#2)
### [2.1 字符串的拓展](#2.1)
### [2.2 正则的拓展](#2.2) 
### [2.3 数值的拓展](#2.3) 
### [2.4 函数的拓展](#2.4)
### [2.5 数组的拓展](#2.5) 
### [2.6 对象的拓展](#2.6)
## [三、Set和Map数据结构](#3)
### [3.1 Set](#3.1)
### [3.2 Map](#3.2) 
### [3.3 数据结构的互相转换](#3.3) 
## [四、Proxy和Reflect](#4)
### [4.1 Proxy](#4.1)
### [4.2 Reflect](#4.2) 
## [五、遍历器Iterator](#5)
### [5.1 概念](#5.1)
### [5.2 部署Iterator接口](#5.2) 
### [5.3 默认的Iterator接口](#5.3)
## [六、Promise对象与Generator 函数与async函数](#6)
### [6.1 Promise对象](#6.1)
### [6.2 Generator 函数](#6.2)  
### [6.3 Generator 与协程与控制流管理](#6.3) 
### [6.4 Generator 异步应用](#6.4) 
### [6.5 async函数](#6.5) 
## [七、类和class](#7)
### [7.1 类的继承](#7.1)
### [7.2 class的使用](#7.2)
### [7.1 class的继承](#7.1) 
## [八、Module](#8)
### [8.1 概念](#8.1)
### [8.2 部署Iterator接口](#8.2) 
### [8.3 默认的Iterator接口](#8.3) 
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

        
------      
        

<h2 id='2'>二、相关拓展</h2>
<h3 id='2.1'>2.1 字符串的拓展</h3>  

#### 1) 2.1 字符串的拓展
> - JavaScript 允许采用\uxxxx形式表示一个字符，其中xxxx表示字符的 Unicode 码点。但是范围仅限于\u0000~\uFFFF之间
> - 超出范围必须使用两个双字节的形式表示，比如\u20BB7
> - ES6 对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。比如"\u{20BB7}"
        
                var a = "\u20BB7";
                10:26:45.183 undefined
                10:26:49.199 a
                10:26:49.202 "₻7"
                10:27:27.189 var b = "\u{20BB7}";
                10:27:27.190 undefined
                10:27:32.674 b;
                10:27:32.675 "𠮷"

        
------      
        

<h2 id='3'>三、Set和Map数据结构</h2>
<h3 id='3.1'>3.1 Set</h3>  

#### 1) 基本用法
> - 类似与数组，但是内部元素不可重复；
> - 由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值）；
> - 通过add()方法进行构建
> - 通过new Set(数组)的方式进行构建
        
                var a = [1, 2, 3, 4, 1, 2, 3, 4];
                var s = new Set();
                a.forEach(x=> s.add(x));
                for(let i of s) {
                    console.log(i);
                }

                16:35:50.504 VM2020:5 1
                16:35:50.504 VM2020:5 2
                16:35:50.504 VM2020:5 3
                16:35:50.504 VM2020:5 4
#### 2) 注意事项
> - 在Set中加入的两个NaN被认为是相同的值
        
                var set = new Set();
                var a = NaN;
                var b = NaN;
                set.add(a);
                set.add(b);
                set // Set {NaN}
                Set(1) {NaN}
                size: (...)
                __proto__ : Set
                [[Entries]]: Array(1) 0
                : NaN
                length: 1
> - 另外，两个对象总是不相等的。，空对象也是不相等的
        
                var set = new Set();
                set.add({});
                set.size // 1

                16:48:37.005 1
                16:48:42.525 set.add({});
                set.size // 2
                16:48:42.529 2       
#### 3) 实例属性和方法
> - add(value)：添加某个值，返回 Set 结构本身。
> - delete(value)：删除某个值，返回一个布尔值，表示删除是否成功。
> - has(value)：返回一个布尔值，表示该值是否为Set的成员。
> - clear()：清除所有成员，没有返回值。
> - Array.from方法可以将 Set 结构转为数组。
        
                var b = Array.from(s);
                16:52:52.746 undefined
                16:52:54.994 b
                16:52:54.997 (4) [1, 2, 3, 4]            
> - keys()：返回键名的遍历器
> - values()：返回键值的遍历器
> - entries()：返回键值对的遍历器
> - forEach()：使用回调函数遍历每个成员     
#### 4) WeakSet                   
> - 跟Set类似，但是它的成员只能是对象
> - WeakSet.prototype.add(value)：向 WeakSet 实例添加一个新成员。
> - WeakSet.prototype.delete(value)：清除 WeakSet 实例的指定成员。
> - WeakSet.prototype.has(value)：返回一个布尔值，表示某个值是否在
> - 没有size和forEach属性，所以无法进行遍历操作
> - WeakSet 的一个用处，是储存 DOM 节点，而不用担心这些节点从文档移除时，会引发内存泄漏。
> - 这是因为垃圾回收机制依赖引用计数，如果一个值的引用次数不为0，垃圾回收机制就不会释放这块内存。结束使用该值之后，有时会忘记取消引用，导致内存无法释放，进而可能会引发内存泄漏。WeakSet 里面的引用，都不计入垃圾回收机制，所以就不存在这个问题。因此，WeakSet 适合临时存放一组对象，以及存放跟对象绑定的信息。只要这些对象在外部消失，它在 WeakSet 里面的引用就会自动消失。 
                                           
<h3 id='3.2'>3.2 Map</h3>  

#### 1) 基本用法
> - 本质上是键值对的集合Hash集合，传统上只能用字符串作键
> - ES6拓宽了这个限制，键不仅可以是字符串，还可以是对象，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。
> - 键值对都是以两元素数组组成的
                
                const strMap = new Map()
                for (let [k,v] of strMap) {
                    obj[k] = v;
                }

                var map = new Map;
                var obj1 = {};
                var obj2 = {name: "lv",};
                map.set(obj1, obj2);
                map.get(obj1).name;
                17:11:59.151 "lv"
                map.has(obj1);
                17:14:49.988 true
                17:14:58.772 map.delete(obj1);
                17:14:58.774 true
                17:15:03.279 map.has(obj1);
                17:15:03.282 false
> - Map的参数还可以是有两个元素的组成键值对的数组
        
                var map = new Map([
                  ['name', '张三'],
                  ['title', 'Author']
                ]);
                17:18:54.038 undefined
                17:18:58.926 map;
                17:18:58.928 Map(2) {"name" => "张三", "title" => "Author"}
#### 2) 注意事项
> -  如果对同一个键多次赋值，后面的值将覆盖前面的值
> -  注意对象作为键的时候只能使用对象的引用，因为如果重新写一个对象他们的地址引用是不相同的
        
                var map = new Map([[{lv: "lvhongbin"}, {id: 116020910160}]]);
                17:34:48.843 map.get({lv: "lvhongbin"});
                17:34:48.844 undefined
> - 如果 Map 的键是一个简单类型的值（数字、字符串、布尔值），则只要两个值严格相等，Map 将其视为一个键，比如0和-0就是一个键，布尔值true和字符串true则是两个不同的键。另外，undefined和null也是两个不同的键。虽然NaN不严格相等于自身，但 Map 将其视为同一个键。
#### 3) 实例属性和方法
> - size属性返回 Map 结构的成员总数。   
> - set方法设置键名key对应的键值为value，然后返回整个 Map 结构。如果key已经有值，则键值会被更新，否则就新生成该键。
> - get方法读取key对应的键值，如果找不到key，返回undefined。
> - has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
> - delete方法删除某个键，返回true。如果删除失败，返回false。
> - clear方法清除所有成员，没有返回值。
> - keys()：返回键名的遍历器。
> - values()：返回键值的遍历器。
> - entries()：返回所有成员的遍历器。
> - forEach()：遍历 Map 的所有成员。 遍历的顺序就是插入的顺序
#### 4) WeakMap                   
> - 跟Map类似，但是它的键名只能是对象
> - WeakMap的键名所指向的对象，不计入垃圾回收机制。
> - WeakMap 就是为了解决这个问题而诞生的，它的键名所引用的对象都是弱引用，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的键名对象和所对应的键值对会自动消失，不用手动删除引用。
> - 没有遍历操作（即没有keys()、values()和entries()方法），也没有size属性
> - WeakMap只有四个方法可用：get()、set()、has()、delete()。
> - 为了防止出现不确定性，就统一规定不能取到键名。二是无法清空，即不支持clear方法
                                           
<h3 id='3.3'>3.3 数据结构的互相转换</h3>  

#### 1) Map 转为数组
> - 最方便的就是使用拓展运算符...
        
                const myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
                17:50:20.646 var yourMap = [...myMap];
                17:50:20.648 undefined
                17:50:36.661 yourMap instanceof Map;
                17:50:36.663 false
                17:50:41.956 yourMap instanceof Array;
                17:50:41.958 true
                yourMap;
                    (2) [Array(2), Array(2)]
                        0:(2) [true, 7]
                        1:Array(2)
                            0:{foo: 3}
                            1:["abc"]
                            length:2
                            __proto__:Array(0)
                            length:2
                        __proto__:Array(0)
#### 2) 数组转为Map
> - 将数组传入 Map 构造函数，就可以转为 Map。
        
------      
        
        
<h2 id='4'>四、Proxy和Reflect</h2>
<h3 id='4.1'>4.1 Proxy</h3>  
        
#### 1) 概述
> - 修改编程语言某些操作的默认行为，所以属于一种“元编程”（meta programming），即对编程语言进行编程。
> - var proxy = new Proxy(target, handler);
> - 相当于过滤器，他是一个构造函数，其中空对象是默认过滤的目标对象，get代表访问请求时进行过滤，同样是一个函数对象，参数分别是实际过滤的目标对象和属性
        
                var a ={time: 40, times:41,};
                var proxy = new Proxy(a, {
                  get: function(target, property) {
                    if(property !== "time") {
                        return target[property]+1;  
                    }else {
                        return target[property]+2;
                    }
                  }
                });

                proxy.time;
                21:34:29.455 42
                21:34:40.384 proxy.times;
                21:34:40.386 42
> - 要使proxy起作用，必须针对Proxy实例，而不是针对目标对象进行操作
> - 如果handler没有设置任何拦截，那就等同于直接通向原对象。
> - receiver指代拦截的实例对象，可选
        
                var target = {};
                var handler = {};
                var proxy = new Proxy(target, handler, receiver);
                proxy.a = 'b';
                target.a // "b"
> - proxy对象是obj对象的原型，obj对象本身并没有time属性，所以根据原型链，会在proxy对象上读取该属性，导致被拦截。
        
                var proxy = new Proxy({}, {
                  get: function(target, property) {
                    return 35;
                  }
                });

                let obj = Object.create(proxy);
                obj.time // 35
#### 2) 拦截种类
> - get(target, propKey, receiver)：拦截对象属性的读取，比如proxy.foo和proxy['foo']。
> - set(target, propKey, value, receiver)：拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。
> - has(target, propKey)：拦截propKey in proxy的操作，返回一个布尔值。
> - deleteProperty(target, propKey)：拦截delete proxy[propKey]的操作，返回一个布尔值。
> - ownKeys(target)：拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。
> - getOwnPropertyDescriptor(target, propKey)：拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象。
> - defineProperty(target, propKey, propDesc)：拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
> - preventExtensions(target)：拦截Object.preventExtensions(proxy)，返回一个布尔值。
> - getPrototypeOf(target)：拦截Object.getPrototypeOf(proxy)，返回一个对象。
> - isExtensible(target)：拦截Object.isExtensible(proxy)，返回一个布尔值。
> - setPrototypeOf(target, proto)：拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。如果目标对象是函数，那么还有两种额外操作可以拦截。
> - apply(target, object, args)：拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
> - construct(target, args)：拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。
        
<h3 id='4.2'>4.2 Reflect</h3>  
        
#### 1) 概述
> - 主要用来将一些明显属于语言内部的方法，如Object.defineProperty，放到Reflect对象中，目前来讲还没有完全转移，在Object对象和Reflect对象上共同存在，后面将完全转移到Reflect上
> - Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。这就让Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。
        
                Proxy(target, {
                  set: function(target, name, value, receiver) {
                    var success = Reflect.set(target,name, value, receiver);
                    if (success) {
                      log('property ' + name + ' on ' + target + ' set to ' + value);
                    }
                    return success;
                  }
                });        
#### 2) 静态方法
> - Reflect.apply(target, thisArg, args)
> - Reflect.construct(target, args)
> - Reflect.get(target, name, receiver)
> - Reflect.set(target, name, value, receiver)
> - Reflect.defineProperty(target, name, desc)
> - Reflect.deleteProperty(target, name)
> - Reflect.has(target, name)
> - Reflect.ownKeys(target)
> - Reflect.isExtensible(target)
> - Reflect.preventExtensions(target)
> - Reflect.getOwnPropertyDescriptor(target, name)
> - Reflect.getPrototypeOf(target)
> - Reflect.setPrototypeOf(target, prototype) 
        
------      
        
        
<h2 id='5'>五、遍历器Iterator</h2>
<h3 id='5.1'>5.1 概念</h3>  
        
#### 1) 作用
> - 为数据结构提供访问接口
> - 为数据结构的成员排序
> - 新的遍历命令for ...of循环
#### 2) Iterator 接口
> - ES6 规定，默认的 Iterator 接口部署在数据结构的Symbol.iterator属性，或者说，一个数据结构只要具有Symbol.iterator属性，就可以认为是“可遍历的”（iterable）。 
> - ES6 的有些数据结构原生具备 Iterator 接口（比如数组），即不用任何处理，就可以被for...of循环遍历。原因在于，这些数据结构原生部署了Symbol.iterator属性（详见下文），另外一些数据结构没有（比如对象）。凡是部署了Symbol.iterator属性的数据结构，就称为部署了遍历器接口。调用这个接口，就会返回一个遍历器对象。
> - 遍历器对象包含很多属性，用得最多的就是next()属性，其次还有return()和throw()属性。每个属性都是函数，而且也必须返回一个对象
> - return() 可选部署 用在提前退出的时候的处理方法，一般是用来清理或者释放资源；
#### 3) next方法本质
> - 提供指针，返回数据结构当前成员的信息(包含value和done两个属性的对象，其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。)
        
                var it = makeIterator(['a', 'b']);
                it.next() // { value: "a", done: false }
                it.next() // { value: "b", done: false }
                it.next() // { value: undefined, done: true }

                function makeIterator(array) {
                  var nextIndex = 0;
                  return {
                    next: function() {
                      return nextIndex < array.length ?
                        {value: array[nextIndex++]} :
                        {done: true};
                    }
                  };
                }

        
<h3 id='5.2'>5.2 部署Iterator接口</h3>
        
#### 1) 为类部署Iterator接口
> - 一个对象如果要具备可被for...of循环调用的 Iterator 接口，就必须在Symbol.iterator的属性上部署遍历器生成方法（原型链上的对象具有该方法也可）
        
                class myIterator {
                    constructor(start, stop) {
                        this.start = start;
                        this.stop = stop;
                    }

                    [Symbol.iterator]() {return this;} 

                    next() {
                        return this.start < this.stop ? {value: this.start++, done: false}:{value: undefined, done: true};
                    }
                }
                11:32:49.291 undefined
                11:32:52.536 var myIterator1 = new myIterator(1,4);
                11:32:52.537 undefined
                11:32:54.871 myIterator1.next();
                11:32:54.875 {value: 1, done: false}
                11:32:56.612 myIterator1.next();
                11:32:56.615 {value: 2, done: false}
                11:32:59.178 myIterator1.next();
                11:32:59.181 {value: 3, done: false}
                11:33:00.849 myIterator1.next();
                11:33:00.850 {value: undefined, done: true}
                11:34:31.518 var myIterator2 = new myIterator(1,4);
                11:34:31.519 undefined
                11:35:09.374 for (let i of myIterator2) {
                    console.log(i + "; ");
                }
                11:35:09.373 VM1493:2 1; 
                11:35:09.374 VM1493:2 2; 
                11:35:09.374 VM1493:2 3; 

#### 2) 为普通对象部署Iterator接口
> - 关键 暴露Symbol.iterator属性的函数接口，返回一个对象，这个对象包含属性，这个属性就是next函数
                
                // 或者
                var myIterator = {
                    data: [],
                    length: 0,
                    now:0,
                    get: function(i) {
                        return this.data[i];
                    },
                    set:function(value) {
                        this.data[this.data.length] = value; 
                    },
                    [Symbol.iterator]: function() {
                        return this;
                    },
                    next: function() {
                        return this.now < this.length ? {value: this.data[this.now++], done: false}: {value: undefined, done: true};
                    },
                }
                14:38:47.650 undefined
                14:38:56.534 myIterator.set(10);
                14:38:56.535 undefined
                14:39:01.548 myIterator.set(20);
                14:39:01.549 undefined
                14:39:07.793 myIterator.set(30);
                14:39:07.797 undefined
                14:39:16.889 myIterator[Symbol.interator];
                14:39:16.891 undefined
                14:39:32.347 for(let i of myIterator) {
                    console.log(i);
                }
                14:39:32.347 VM1856:2 10
                14:39:32.348 VM1856:2 20
                14:39:32.348 VM1856:2 30

                //或者
                var myIterator = {
                    data: [],
                    length: 0,
                    now:0,
                    get: function(i) {
                        return this.data[i];
                    },
                    set:function(value) {
                        this.data[this.data.length] = value; 
                    },
                    [Symbol.iterator]: function() {
                        const self = this;
                        return {next() {return self.now < self.data.length ? {value: self.data[self.now++], done: false}: {value: undefined, done: true};}}
                    },
                };
                14:51:40.358 undefined
                14:51:44.435 myIterator.set(10);
                14:51:44.437 undefined
                14:51:47.041 myIterator.set(20);
                14:51:47.044 undefined
                14:51:49.181 myIterator.set(30);
                14:51:49.187 undefined
                14:51:52.518 myIterator.data;
                14:51:52.520 (3) [10, 20, 30]
                14:52:30.770 for(let i of myIterator) {
                    console.log(i);
                }
                14:52:30.770 VM1929:2 10
                14:52:30.771 VM1929:2 20
                14:52:30.771 VM1929:2 30
                14:52:30.777 undefined

#### 3) 为类数组部署Iterator接口
> - 比较简单，可以直接利用数组的Iterator接口
        
                NodeList.prototype[Symbol.iterator] = Array.prototype[Symbol.iterator];
                // 或者
                NodeList.prototype[Symbol.iterator] = [][Symbol.iterator];

                [...document.querySelectorAll('div')] // 可以执行了

                //其他类数组对象
                et iterable = {
                  0: 'a',
                  1: 'b',
                  2: 'c',
                  length: 3,
                  [Symbol.iterator]: Array.prototype[Symbol.iterator]
                };
                for (let item of iterable) {
                  console.log(item); // 'a', 'b', 'c'
                }
#### 4) 调用 Iterator 接口的场合
> - 解构赋值
> - 扩展运算符（...） 必须外面有方括号形成数组或者圆括号称为函数的参数
> - yield*后面跟的是一个可遍历的结构，它会调用该结构的遍历器接口。yield* [2,3,4];
> - 其他场合
>> - for...of
>> - Array.from()
>> - Map(), Set(), WeakMap(), WeakSet()（比如new Map([['a',1],['b',2]])）
>> - Promise.all()
>> - Promise.race()
        
<h3 id='5.3'>5.3 默认的Iterator接口</h3>  
        
#### 1) 数组的Iterator接口
        
                var arr = ['a', 'b', 'c'];
                var iter = arr[Symbol.iterator]();

                iter.next() // { value: 'a', done: false }
                iter.next() // { value: 'b', done: false }
                10:52:34.484 {value: "b", done: false}

                for (let value of arr) {
                    console.log(value + "; ");
                }
                10:57:36.063 VM1197:2 a; 
                10:57:36.063 VM1197:2 b; 
                10:57:36.063 VM1197:2 c; 

#### 2) 字符串的Iterator接口
        
                var str = "hello";
                for(let s of str) {
                    console.log(s);
                }
                15:54:31.687 VM1947:3 h
                15:54:31.687 VM1947:3 e
                15:54:31.687 VM1947:3 l
                15:54:31.687 VM1947:3 l
                15:54:31.687 VM1947:3 o

                var iterator = str[Symbol.iterator]();
                15:55:50.167 undefined
                15:55:58.937 iterator.next();
                15:55:58.939 {value: "h", done: false}
                15:56:01.313 iterator.next();
                15:56:01.315 {value: "e", done: false}
                15:56:02.000 iterator.next();
                15:56:02.004 {value: "l", done: false}
                15:56:02.548 iterator.next();
                15:56:02.549 {value: "l", done: false}
                15:56:03.637 iterator.next();
                15:56:03.643 {value: "o", done: false}
                15:56:08.099 iterator.next();
                15:56:08.102 {value: undefined, done: true}
        
------      
        
        
<h2 id='6'>六、Promise对象与Generator 函数与async函数</h2>
<h3 id='6.1'>6.1 Promise对象</h3>  
        
#### 1) 概述
> - Promise 是异步编程的一种解决方案，比传统的解决方案——回调函数和事件——更合理和更强大。
> - Promise对象不受外界的影响。它代表的是一个异步操作，包括三种状态：pending进行中，fulfilled已成功和rejected已失败。只有异步操作的结果可以决定或者改变当前的状态，其他的操作都不能改变
> - 状态的变换只有两种情况：
> - pending进行中 -> fulfilled已成功 或者 pending进行中 -> fulfilled已成功
> - 一旦状态改变后就不会再改变了，此时就称为resolved已定型
> - 如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。
> - 这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
#### 2) 优缺点
> - 有了Promise对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise对象提供统一的接口，使得控制异步操作更加容易。
> - 无法取消Promise，一旦新建它就会立即执行，无法中途取消。
> - 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
> - 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
#### 3) 基本用法
> -  then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数。
        
                // 这Promise的新建主要用来存放执行条件和传递参数的
                const promise = new Promise(function(resolve, reject) {
                    // ... some code
                    if (/* 异步操作成功 */){
                        resolve(value);
                    } else {
                        reject(error);
                    }
                });  
    
                // 具体定义resolve和reject函数
                promise.then(function(value) {
                  // success
                }, function(error) {
                  // failure
                });     
> - Promise 新建后立即执行，所以首先输出的是Promise。然后，then方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出
        
                var findGF = function(name, height) {
                    return new Promise(function(resolve, reject) {
                                            console.log(`I want to find a GF...`);  
                                            if(height >= 155 && height <= 165) {
                                                resolve(name);
                                            }else {
                                                reject(name);
                                            }
                                        }) 
                };

                var resolve = function(name) {
                    console.log(`I have found a GF ${name} !`);
                };

                var reject = function(name) {
                    console.log(`I have refused ${name} !`);
                };

                findGF("Lily", 160).then(resolve, reject);
                findGF("Belly", 168).then(resolve, reject);
                console.log(`I am looking for a GF...`);
                10:24:50.539 VM2507:3 I want to find a GF...
                10:24:50.539 VM2507:3 I want to find a GF...
                10:24:50.539 VM2507:22 I am looking for a GF...
                10:24:50.539 VM2507:13 I have found a GF Lily !
                10:24:50.539 VM2507:17 I have refused Belly !
> - then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。 
        
                getJSON("/posts.json").then(function(json) {
                  return json.post;
                }).then(function(post) {
                  // ...
                });
> - then方法的链式操作
> - 奇怪的是链式操作只能返回一个参数，加入你要返回多个参数的话，需要传递一个数组或者一个对象
                
                // 返回Promise实例版本
                var findBeadtiful = new Promise(function(resolve, error) {
                    resolve("Beadtiful ");
                });
                var findCute = new Promise(function(resolve, error) {
                    resolve("Cute ");
                });
                var findWarm = new Promise(function(resolve, error) {
                    resolve(["Warm", "Sunny"]);
                });

                var girlType = function(str) {
                    console.log(str);
                }

                var findGF = function() {

                    console.log("I want to find a girl who is ");
                    findBeadtiful.then(function(str) {
                                            console.log(str);
                                            return findCute;
                                        })
                                 .then(function(str) {
                                            console.log(str);
                                            return findWarm;
                                        })
                                 .then(function(str) {
                                            console.log(`${str[0]} and ${str[1]}`);
                                        });
                }

                findGF();
                21:03:41.207 VM3451:17 I want to find a girl who is 
                21:03:41.208 VM3451:19 Beadtiful 
                21:03:41.208 VM3451:23 Cute 
                21:03:41.209 VM3451:27 Warm and Sunny  

                // 返回参数版本
                var findBeadtiful = new Promise(function(resolve, error) {
                    resolve("Beadtiful ");
                });
                /*
                    var findCute = new Promise(function(resolve, error) {
                        resolve();
                    });
                    var findWarm = new Promise(function(resolve, error) {
                        resolve();
                    });
                */

                var girlType = function(str) {
                    console.log(str);
                }

                var findGF = function() {

                    console.log("I want to find a girl who is ");
                    findBeadtiful.then(function(str) {
                                            console.log(str);
                                            return "Cute ";
                                        })
                                 .then(function(str) {
                                            console.log(str);
                                            return ["Warm", "Sunny"];
                                        })
                                 .then(function(str) {
                                            console.log(`${str[0]} and ${str[1]}`);
                                        });
                }

                findGF();
                21:09:11.529 VM3453:19 I want to find a girl who is 
                21:09:11.529 VM3453:21 Beadtiful 
                21:09:11.530 VM3453:25 Cute 
                21:09:11.531 VM3453:29 Warm and Sunny                                                         
#### 3) Promise.prototype.catch()
> - Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。
> - 一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。
> - 第二种写法要好于第一种写法，理由是第二种写法可以捕获前面then方法执行中的错误，也更接近同步的写法（try/catch）。因此，建议总是使用catch方法，而不使用then方法的第二个参数。
        
                // bad
                promise
                  .then(function(data) {
                    // success
                  }, function(err) {
                    // error
                  });

                // good
                promise
                  .then(function(data) { //cb
                    // success
                  })
                  .catch(function(err) {
                    // error
                  });                        
> - 跟传统的try/catch代码块不同的是，如果没有使用catch方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。
> - 一般总是建议，Promise 对象后面要跟catch方法，这样可以处理 Promise 内部发生的错误。catch方法返回的还是一个 Promise 对象，因此后面还可以接着调用then方法。
        
                const someAsyncThing = function() {
                  return new Promise(function(resolve, reject) {
                    // 下面一行会报错，因为x没有声明
                    resolve(x + 2);
                  });
                };

                someAsyncThing()
                .catch(function(error) {
                  console.log('oh no', error);
                })
                .then(function() {
                  console.log('carry on');
                });
                // oh no [ReferenceError: x is not defined]
                // carry on
#### 4) Promise.prototype.finally() 
> - finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。
#### 5) Promise.all()
> - const p = Promise.all([p1, p2, p3]);
> - 只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
> - 只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
> - 相当于并运算
#### 6) Promise.race()
> - const p = Promise.race([p1, p2, p3]);
> - 相当于或运算
#### 7) Promise.resolve()
> - 如果参数是 Promise 实例，那么Promise.resolve将不做任何修改、原封不动地返回这个实例。
> - 如果参数是一个数是一个thenable对象（thenable对象指的是具有then方法的对象），Promise.resolve方法会将这个对象转为 Promise 对象，然后就立即执行thenable对象的then方法。 
        
                let thenable = {
                  then: function(resolve, reject) {
                    resolve(42);
                  }
                };

                let p1 = Promise.resolve(thenable);
                p1.then(function(value) {
                  console.log(value);  // 42
                });
> - 如果参数不是具有then方法的对象，或根本就不是对象。则Promise.resolve方法返回一个新的 Promise 对象，状态为resolved。由于字符串Hello不属于异步操作（判断方法是字符串对象不具有 then 方法），返回 Promise 实例的状态从一生成就是resolved，所以回调函数会立即执行
#### 8) Promise.reject() 
        
                const p = Promise.reject('出错了');
                // 等同于
                const p = new Promise((resolve, reject) => reject('出错了'))

                p.then(null, function (s) {
                  console.log(s)
                });
                // 出错了

        
<h3 id='6.2'>6.2 Generator 函数</h3>
        
#### 1) 简介
> - Generator 函数是 ES6 提供的一种异步编程解决方案
> - Generator 函数是一个状态机，封装了多个内部状态。
> - 执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。
> - 形式上，Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。
> - 然后，Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象
> - 必须调用遍历器对象的next方法，使得指针移向下一个状态（这个状态是由value和done两个属性组成的对象）。Generator 函数是分段执行的，yield表达式是暂停执行的标记，而next方法可以恢复执行，到return的时，函数返回结束；
#### 2）用法
> - 每次调用的时候，要确保迭代器对象的引用地址一致，否则会重新开始状态
> - 但是，函数f是一个 Generator 函数，就变成只有调用next方法时，函数f才会执行。
> - 另外需要注意，yield表达式只能用在 Generator 函数里面，用在其他地方都会报错。
        
                function* findGF() {
                    yield "Beautiful";
                    yield "Heignt > 155";
                    yield "Female";
                    return "Done!"
                }
                16:10:55.090 undefined
                16:12:39.543 findGF().next();
                16:12:39.555 {value: "Beautiful", done: false}
                16:12:42.730 findGF().next();
                16:12:42.745 {value: "Beautiful", done: false}
                16:12:49.044 findGF().next();
                16:12:49.058 {value: "Beautiful", done: false}
                16:17:01.231 var f = findGF();
                16:17:01.238 undefined
                16:17:10.390 f.next();
                16:17:10.399 {value: "Beautiful", done: false}
                16:17:12.153 f.next();
                16:17:12.162 {value: "Heignt > 155", done: false}
                16:17:13.927 f.next();
                16:17:13.934 {value: "Female", done: false}
                16:17:15.789 f.next();
                16:17:15.797 {value: "Done!", done: true}
> - yield表达式如果用在另一个表达式之中，必须放在圆括号里面。
        
                function* demo() {
                    console.log('Hello' + yield); // SyntaxError
                    console.log('Hello' + yield 123); // SyntaxError

                    console.log('Hello' + (yield)); // OK
                    console.log('Hello' + (yield 123)); // OK
                }
> - yield表达式用作函数参数或放在赋值表达式的右边，可以不加括号。
        
                function* demo() {
                    foo(yield 'a', yield 'b'); // OK
                    let input = yield; // OK
                }
#### 3）next()方法
> - next()方法是有参数的，它的参数是对应于上一个yield的值，然后给出对应yield的值
> - 由于第一个next()方法没有对应于上一个yield的值，那么上一个yield的值则由generator的实例构建时所放进的参数；
> - next()方法加入不带参数，那么对应于上一个yield的值为undefined
> - yield的值最好不要跟上一个yield的值产生依赖，连锁依赖不能超过1个
> - 最好参数先求出来，然后在再用yield产生断点
        
                function fibonacci(n) {
                        return n<2 ? n: fibonacci(n-1) + fibonacci(n-2);
                }

                var f = fibonacci(10);
#### 4）for...of方法
> - 直接忽略return返回的值，因为return语句对应的是{value: , done: true}
> - 如果要在迭代保留yield的值，一个必要的条件是yield的参数保持不变
        
                // 由于a[0]和a[1]需要依赖yield，然而next()方法并没有给出参数数，所以会出现NaNcy
                function* fibonacci(n) {
                    var a = [];
                    a[0] = yield 0;
                    a[1] = yield 1;
                    for(let i = 2; i < n; i++ ) {
                        a[i] = (a[i-1] + a[i-2]);
                        yield a[i];
                    }
                }

                for(let i of fibonacci(10)) {
                    console.log(i);
                }
                10:56:12.692 VM2685:2 0
                10:56:12.693 VM2685:2 1
                10:56:12.693 VM2685:2 NaN
                10:56:12.693 VM2685:2 NaN
                10:56:12.694 VM2685:2 NaN
                10:56:12.694 VM2685:2 NaN
                10:56:12.694 VM2685:2 NaN
                10:56:12.694 VM2685:2 NaN
                10:56:12.694 VM2685:2 NaN
                10:56:12.695 VM2685:2 NaN
    
                // yield的参数不产生依赖
                function* fibonacci(n) {
                    var a = [];
                    for(let i = 0; i < n; i++ ) {
                        a[i] = i<2 ? i: (a[i-1] + a[i-2]);
                        yield a[i];
                    }
                }
                11:35:59.323 undefined
                11:36:03.639 for(let i of fibonacci(10)) {
                    console.log(i);
                }
                11:36:03.640 VM2738:2 0
                11:36:03.640 VM2738:2 1
                11:36:03.640 VM2738:2 1
                11:36:03.640 VM2738:2 2
                11:36:03.640 VM2738:2 3
                11:36:03.640 VM2738:2 5
                11:36:03.640 VM2738:2 8
                11:36:03.640 VM2738:2 13
                11:36:03.640 VM2738:2 21
                11:36:03.640 VM2738:2 34

                // 把for循环解开后得到
                function* fibonacci(n) {
                    let a = [];
                    a[0] = 0;
                    console.log(`Now a[${0}] = ${a[0]}`);
                    yield a[0];
                    a[1] = 1;
                    console.log(`Now a[${1}] = ${a[1]}`);
                    yield a[1];
                    a[2] = a[1] + a[0];
                    console.log(`Now a[${2}] = ${a[2]}`);
                    yield a[2];
                }
                12:22:27.859 undefined
                12:22:30.248 var f = fibonacci(10);
                12:22:30.258 undefined
                12:22:39.418 f.next();
                12:22:39.420 VM3002:4 Now a[0] = 0
                12:22:39.434 {value: 0, done: false}
                12:22:58.604 f.next();
                12:22:58.606 VM3002:7 Now a[1] = 1
                12:22:58.619 {value: 1, done: false}
                12:23:00.630 f.next();
                12:23:00.631 VM3002:10 Now a[2] = 1
                12:23:00.647 {value: 1, done: false}
#### 5）Generator.prototype.throw() 
> - throw方法可以接受一个参数，该参数会被catch语句接收，建议抛出Error对象的实例。
> - catch错误后可以继续执行下一个yield
        
               var g = function* () {
                  try {
                    yield;
                  } catch (e) {
                    console.log(e);
                  }
                };

                var i = g();
                i.next();
                i.throw(new Error('出错了！'));
        
                function* g() {
                    yield 1;
                    console.log('throwing an exception');
                    throw new Error('generator broke!');
                    yield 2;
                    yield 3;
                }

                // Find a perfect GF 体内捕获错误
                function* findGF(height, color) {
                    yield "start to find a GF!";
                    try {
                        console.log("Check the girl's height");
                        yield height;   
                    }catch(e) {
                        if(height < 155) {
                            console.log("The girl is too short!");
                        }
                        if(height > 165) {
                            console.log("The girl is too high!");
                        }
                        yield "The girl's height is not Good";
                    }

                    try {
                        console.log("Check the girl's color");
                        yield color;   
                    }catch(e) {
                        console.log("The girl is too black!");
                        yield "The girl's color is not Good";
                    }
                    
                    return height > 155 && height < 165 && color !== "black" ? "The GF is perfect!" : "The girl is not perfect!"
                }

                var isGFPerfect = function(f) {
                    
                    // start to find a GF!
                    console.log(f.next());

                    let height = f.next().value;

                    // Check the girl's height
                    if(height < 155 || height > 165) {
                        f.throw(new Error());
                    }else {
                        console.log("The girl's height is Good");
                    }

                    // Check the girl's color
                    if(f.next().value == "black") {
                        f.throw(new Error());
                    }else {
                        console.log("The girl's color is Good");
                    }

                    // Finish the check
                    console.log(f.next());
                }
                15:55:56.605 undefined
                15:56:40.495 var Lily = findGF(153, "black");
                var Belly = findGF(156, "black");
                var Lisa = findGF(156, "white");
                15:56:40.514 undefined
                15:57:18.226 isGFPerfect(Lily);

                15:57:18.227 VM3145:30 {value: "start to find a GF!", done: false}
                15:57:18.228 VM3145:4 Check the girl's height
                15:57:18.228 VM3145:8 The girl is too short!
                15:57:18.228 VM3145:17 Check the girl's color
                15:57:18.228 VM3145:20 The girl is too black!
                15:57:18.228 VM3145:49 {value: "The girl is not perfect!", done: true}
                15:57:18.266 undefined
                15:57:25.889 isGFPerfect(Belly);
                15:57:25.890 VM3145:30 {value: "start to find a GF!", done: false}
                15:57:25.891 VM3145:4 Check the girl's height
                15:57:25.891 VM3145:38 The girl's height is Good
                15:57:25.892 VM3145:17 Check the girl's color
                15:57:25.893 VM3145:20 The girl is too black!
                15:57:25.893 VM3145:49 {value: "The girl is not perfect!", done: true}
                15:57:25.950 undefined
                15:57:30.004 isGFPerfect(Lisa);
                15:57:30.005 VM3145:30 {value: "start to find a GF!", done: false}
                15:57:30.006 VM3145:4 Check the girl's height
                15:57:30.006 VM3145:38 The girl's height is Good
                15:57:30.006 VM3145:17 Check the girl's color
                15:57:30.007 VM3145:45 The girl's color is Good
                15:57:30.007 VM3145:49 {value: "The GF is perfect!", done: true}
                15:57:30.084 undefined
                
> - 除了体内捕获错误，还可以体外捕获错误，但是一旦 Generator 执行过程中抛出错误，且没有被内部捕获，就不会再执行下去了。如果此后还调用next方法，将返回一个value属性等于undefined、done属性等于true的对象，即 JavaScript 引擎认为这个 Generator 已经运行结束了。
        
                // Find a perfect GF 体内捕获错误
                function* findGF(height, color) {
                    yield "start to find a GF!";
                    yield "Check the girl's height"; 
                    if(height < 155) {
                        throw("The girl is too short!");
                    }
                    if(height > 165) {
                        throw("The girl is too high!");
                    }

                    yield "Check the girl's color"; 
                    if(height > 165) {
                         throw("The girl is too black!");
                    }     

                    return height > 155 && height < 165 && color !== "black" ? "The GF is perfect!" : "The girl is not perfect!"
                }

                var isGFPerfect = function(f) {

                    // start to find a GF!
                    console.log(f.next());
                    
                    // Check the girl's height
                    console.log(f.next());
                    try {
                        f.next()
                    }catch(e) {
                         console.log(e);
                    }

                    // Check the girl's color
                    console.log(f.next());
                    try {
                        f.next()
                    }catch(e) {
                         console.log(e);
                    }

                    // Finish the check
                    console.log(f.next());
                }
                16:22:20.642 undefined
                16:22:40.485 var Lily = findGF(153, "black");

                16:22:40.492 undefined
                16:22:51.811 isGFPerfect(Lily);
                16:22:51.813 VM3163:23 {value: "start to find a GF!", done: false}
                16:22:51.814 VM3163:26 {value: "Check the girl's height", done: false}
                16:22:51.814 VM3163:30 The girl is too short!
                16:22:51.815 VM3163:34 {value: undefined, done: true}
                16:22:51.815 VM3163:42 {value: undefined, done: true}done: truevalue: undefined__proto__: Object
                16:22:51.867 undefined              
#### 6）Generator.prototype.return() 
> - Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。
> - 如果return方法调用时，不提供参数，则返回值的value属性为undefined。
        
                function* gen() {
                    yield 1;
                    yield 2;
                    yield 3;
                }

                var g = gen();

                g.next()        // { value: 1, done: false }
                g.return('foo') // { value: "foo", done: true }
                g.next()        // { value: undefined, done: true } 

#### 7）yield* 表达式
> - 如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的。
> - 如果你想在一个Generator函数里面执行另一个Generator函数
        
                function* inner() {
                  yield 'hello!';
                }

                function* outer1() {
                  yield 'open';
                  yield inner();
                  yield 'close';
                }

                var gen = outer1()
                gen.next().value // "open"
                gen.next().value // 返回一个遍历器对象
                gen.next().value // "close"

                function* outer2() {
                  yield 'open'
                  yield* inner()
                  yield 'close'
                }

                var gen = outer2()
                gen.next().value // "open"
                gen.next().value // "hello!"
                gen.next().value // "close"
> - 实际上，任何数据结构只要有 Iterator 接口，就可以被yield*遍历
        
                var read = (function* () {
                    yield 'hello';
                    yield* 'hello';
                })();

                read.next().value // "hello"
                16:43:43.955 "hello"
                16:43:49.434 read.next().value // "h"
                16:43:49.439 "h"
                16:43:52.899 read.next().value 
                16:43:52.911 "e"
                16:43:54.450 read.next().value 
                16:43:54.460 "l"
                16:43:55.747 read.next().value 
                16:43:55.751 "l"
                16:43:56.634 read.next().value 
                16:43:56.650 "o"
                16:43:58.177 read.next().value 
                16:43:58.184 undefined
#### 8）Generator 与状态机
        
                var clock = function* () {
                  while (true) {
                    console.log('Tick!');
                    yield;
                    console.log('Tock!');
                    yield;
                  }
                };                                                               

        
<h3 id='6.3'>6.3 Generator 与协程与控制流管理</h3>  
        
#### 1) 协程与子例程的区别 
> - 传统的“子例程”（subroutine）采用堆栈式“后进先出”的执行方式，只有当调用的子函数完全执行完毕，才会结束执行父函数。
> - 协程则更偏向于类似进程和线程的管理，则单核的情况下，一个线程执行到一半后暂停，然后另外一个线程接着运行一段时间后暂停，类似这样的相互运行一段时间后交出控制权，营造一种多线程执行的假象。这种可以并行执行、交换执行权的线程（或函数），就称为协程。
> - 从实现上看，在内存中，子例程只使用一个栈（stack），而协程是同时存在多个栈，但只有一个栈是在运行状态，也就是说，协程是以多占用内存为代价，实现多任务的并行。
#### 2) 协程与普通线程的区别 
> - 协程与普通的线程很相似，都有自己的执行上下文、可以分享全局变量。
> - 它们的不同之处在于，同一时间可以有多个线程处于运行状态，但是运行的协程只能有一个，其他协程都处于暂停状态。此外，普通的线程是抢先式的，到底哪个线程优先得到资源，必须由运行环境决定，但是协程是合作式的，执行权由协程自己分配。
#### 3) Generator 与上下文 
> - Generator执行产生的上下文环境，一旦遇到yield命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行next命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。
#### 4) 控制流管理
> - 异步操作的同步化表达
> - 这种写法的好处是所有findGF过程的逻辑，都被封装在一个函数，按部就班非常清晰，而且可以隐藏细节；
> - 
                // 返回Promise实例版本
                var findBeadtiful = new Promise(function(resolve, error) {
                    resolve("Beadtiful ");
                });
                /*
                    var findCute = new Promise(function(resolve, error) {
                        resolve();
                    });
                    var findWarm = new Promise(function(resolve, error) {
                        resolve();
                    });
                */

                var girlType = function(str) {
                    console.log(str);
                }

                var findGF = function() {

                    console.log("I want to find a girl who is ");
                    findBeadtiful.then(function(str) {
                                            console.log(str);
                                            return "Cute ";
                                        })
                                 .then(function(str) {
                                            console.log(str);
                                            return ["Warm", "Sunny"];
                                        })
                                 .then(function(str) {
                                            console.log(`${str[0]} and ${str[1]}`);
                                        });
                }

                findGF();
                21:09:11.529 VM3453:19 I want to find a girl who is 
                21:09:11.529 VM3453:21 Beadtiful 
                21:09:11.530 VM3453:25 Cute 
                21:09:11.531 VM3453:29 Warm and Sunny

                // Generator版本
                var findGF =function* () {
                    console.log("Start!");
                    yield "Looking for a girl in a park!"
                    yield "Looking for a girl in a garden!"
                    console.log("I have found a girl!");
                }
                18:00:18.700 undefined
                18:00:41.939 var Lily =  findGF();
                18:00:41.947 undefined
                18:00:58.674 Lily.next();
                18:00:58.678 VM3310:2 Start!
                18:00:58.697 {value: "Looking for a girl in a park!", done: false}
                18:01:11.184 Lily.next();
                18:01:11.200 {value: "Looking for a girl in a garden!", done: false}
                18:01:14.506 Lily.next();
                18:01:14.508 VM3310:5 I have found a girl!
                18:01:14.531 {value: undefined, done: true}

                                                          
<h3 id='6.4'>6.4 Generator 异步应用</h3>  
        
#### 1) 异步编程简介与的传统方法 
> - 所谓"异步"，简单说就是一个任务不是连续完成的，可以理解成该任务被人为分成两段，先执行第一段，然后转而执行其他任务，等做好了准备，再回过头执行第二段。
> - 传统方法
>> - 回调函数
>> - 事件监听
>> - 发布/订阅
>> - Promise对象 
#### 2) 回调函数
> - 上面代码中，readFile函数的第三个参数，就是回调函数，也就是任务的第二段。等到操作系统返回了/etc/passwd这个文件以后，回调函数才会执行。
> - 一个有趣的问题是，为什么 Node 约定，回调函数的第一个参数，必须是错误对象err（如果没有错误，该参数就是null）？
> - 原因是执行分成两段，第一段执行完以后，任务所在的上下文环境就已经结束了。在这以后抛出的错误，原来的上下文环境已经无法捕捉，只能当作参数，传入第二段。
        
                fs.readFile('/etc/passwd', 'utf-8', function (err, data) {
                    if (err) throw err;
                    console.log(data);
                });
#### 3) Promise对象
> - 链式结构避免多重嵌套
> - Promise 的最大问题是代码冗余，原来的任务被 Promise 包装了一下，不管什么操作，一眼看去都是一堆then，原来的语义变得很不清楚。
        
                var readFile = require('fs-readfile-promise');
                readFile(fileA)
                .then(function (data) {
                  console.log(data.toString());
                })
                .then(function () {
                  return readFile(fileB);
                })
                .then(function (data) {
                  console.log(data.toString());
                })
                .catch(function (err) {
                  console.log(err);
                });
> - Generator的同步编程
        
                var readFile = function* () {

                    // read fileA
                    try {
                        yield fs.readFile(fileA, 'utf-8', function (err, data) {});
                    }catch(e) {
                        yield e;
                    }

                    // read fileB
                    try {
                        yield fs.readFile(fileB, 'utf-8', function (err, data) {});
                    }catch(e) {
                        yield e;
                    }    
                }

                21:59:35.897 undefined
                21:59:39.451 var f = readFile(); 
                21:59:39.465 undefined
                21:59:53.765 console.log(f.next());  
                21:59:53.766 VM3498:1 {value: ReferenceError: fs is not defined
                    at readFile (<anonymous>:5:3)
                    at readFile.next (<anonymous…, done: false}
                21:59:53.781 undefined
                21:59:59.052 console.log(f.next());
                21:59:59.053 VM3499:1 {value: ReferenceError: fs is not defined
                    at readFile (<anonymous>:12:3)
                    at readFile.next (<anonymou…, done: false}
                21:59:59.059 undefined
                22:00:07.273 console.log(f.next()); 
                22:00:07.274 VM3501:1 {value: undefined, done: true}
#### 4) Thunk函数转换器
> - 参数的求职策略
>> - 传值调用（call by value） 参数传入函数之前已经求出结果 C语言便采用这种策略，JavaScript 语言是也是传值调用
>> - 传名调用（call by name） 参数传入函数之后已经求出结果，即只在执行时求值，这样做的好处是不在一些不需要用到参数的情况下消耗性能，比如著名的Thunk函数
> - Array.prototype.slice.call(arguments)能将具有length属性的对象转成数组
        
                var a={length:2,0:'first',1:'second'};
                Array.prototype.slice.call(a);//  ["first", "second"]
                  
                var a={length:2};
                Array.prototype.slice.call(a);//  [undefined, undefined]
> - 目的是将一个大函数中的参数以及回调函数分离开来    
        
                // ES5版本
                var Thunk = function(fn){
                    
                    // 分离参数
                    return function (){
                        var args = Array.prototype.slice.call(arguments);

                        // 分离回调函数
                        return function (callback){
                            args.push(callback);

                            //执行操作
                            return fn.apply(this, args);
                        }
                    };
                };

                // ES6版本
                const Thunk = function(fn) {

                    // 分离参数
                    return function (...args) {

                        // 分离回调函数
                        return function (callback) {

                            //执行操作
                            return fn.call(this, ...args, callback);
                        }
                    };
                };

                //检测
                function f(a, cb) {
                  cb(a);
                }
                const ft = Thunk(f);

                ft(1)(console.log) // 1
#### 5) Generator 函数的流程管理
> - Generator 函数gen会自动执行
        
                function* gen() {
                    // ...
                }

                var g = gen();
                var res = g.next();

                // 假如没有完成的话done的值为flase；
                while(!res.done){
                    console.log(res.value);
                    res = g.next();
                }
#### 5) Thunk 函数的自动流程管理
> - 上面代码的run函数，就是一个 Generator 函数的自动执行器。内部的next函数就是 Thunk 的回调函数。next函数先将指针移到 Generator 函数的下一步（gen.next方法），然后判断 Generator 函数是否结束（result.done属性），如果没结束，就将next函数再传入 Thunk 函数（result.value属性），否则就直接退出。 
> - 前提是每一个异步操作，都要是 Thunk 函数，也就是说，跟在yield命令后面的必须是 Thunk 函数。     
                function run(fn) {
                    var gen = fn();

                    function next(err, data) {
                        var result = gen.next(data);
                        if (result.done) return;
                        result.value(next);
                    }

                    next();
                }

                var g = function* (){

                    // var readFileThunk = Thunk(fs.readFile);
                    // readFileThunk(fileA)(callback);
                    // 读完函数之后就执行callback函数
                    var f1 = yield readFileThunk('fileA');
                    var f2 = yield readFileThunk('fileB');
                    // ...
                    var fn = yield readFileThunk('fileN');
                };

                run(g);

> - generator版本执行器executor 同步执行
                // 原始函数
                var findGF = function* (args, callback) {
                    yield "Start speaking my girl's name!";
                    console.log(`My girl's name is ${args};`);
                    yield "End!";
                    yield callback;
                }

                // Trump转换器
                var translator = function() {
                    return function(args) {
                        return function(callback) {
                            return findGF(args, callback);
                        }
                    }
                }


                // generator版本执行器executor
                var execute = function(generator) {
                    let Generator = generator();
                    let result = Generator.next();
                    while(!result.done) {

                        // 把构造器放进回调函数中
                        let findGFGenerator = result.value(Generator);
                        findGFGenerator.next();
                        findGFGenerator.next();

                        //回调 g函数构造器
                        result = findGFGenerator.next().value;

                        //g函数构造器指针向下一格
                        result = result.next();
                    }
                }

                //定义generator
                var changeGF = function*() {
                    let t = translator();
                    yield t("Lily");
                    yield t("Belly");
                    yield t("Lisa");
                    yield t("Amy");
                }

                //执行操作
                execute(changeGF);

                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                console.log(`fib(18) = ${fib(18)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                11:13:50.949 VM464:4 My girl's name is Lily;
                11:13:50.949 VM464:4 My girl's name is Belly;
                11:13:50.949 VM464:4 My girl's name is Lisa;
                11:13:50.949 VM464:4 My girl's name is Amy;
                11:13:50.949 VM464:54 fib(18) = 2584
                11:13:50.950 VM464:55 随便写点什么东西，看看是不是提前运行了
                11:13:50.950 VM464:56 fib(5) = 5
                11:13:50.950 VM464:57 fib(5) = 5
                11:13:50.950 VM464:58 fib(5) = 5
                11:13:50.950 VM464:59 fib(5) = 5
                11:13:50.950 VM464:60 fib(5) = 5
                11:13:50.950 VM464:61 fib(5) = 5
                11:13:50.950 VM464:62 fib(5) = 5
                11:13:50.950 VM464:63 fib(5) = 5
                11:13:50.950 VM464:64 fib(5) = 5

> - Promise版本执行器executor，关键在于把resolveF和promise.then()方法互相耦合 异步执行 
                
                // 原始函数
                var findGF = function* (args, callback) {
                    yield "Start speaking my girl's name!";
                    console.log(`My girl's name is ${args};`);
                    yield "End!";
                    yield callback;
                }

                // Trump转换器
                var translator = function() {
                    return function(args) {
                        return function(callback) {
                            return findGF(args, callback);
                        }
                    }
                }

                // Promise版本执行器executor
                var execute = function(generator) {

                    let g = generator();
                    let result = g.next();
                    let resolveF = function(Generator) {
                         // 把构造器放进回调函数中
                        let findGFGenerator = result.value(Generator);
                        findGFGenerator.next();
                        findGFGenerator.next();

                        //回调 g函数构造器
                        result = findGFGenerator.next().value;

                        //g函数构造器指针向下一格
                        result = result.next();
                        if(result.done) {
                            throw "done";
                        }else {
                            next();
                        }
                        return Generator;
                    }
                    let promise = new Promise(function(resolve, error) {resolve(g);});

                    let next = function() {
                        promise = promise.then(resolveF).catch(function(e) {console.log("done!");});        
                    };
                    resolveF(g);
                }

                //定义generator
                var changeGF = function*() {
                    let t = translator();
                    yield t("Lily");
                    yield t("Belly");
                    yield t("Lisa");
                    yield t("Amy");
                    yield t("Janny");
                }

                execute(changeGF);

                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                console.log(`fib(18) = ${fib(18)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);


                11:11:27.685 VM462:4 My girl's name is Lily;
                11:11:27.686 VM462:65 fib(18) = 2584
                11:11:27.686 VM462:66 随便写点什么东西，看看是不是提前运行了
                11:11:27.686 VM462:67 fib(5) = 5
                11:11:27.686 VM462:68 fib(5) = 5
                11:11:27.686 VM462:69 fib(5) = 5
                11:11:27.686 VM462:70 fib(5) = 5
                11:11:27.686 VM462:71 fib(5) = 5
                11:11:27.686 VM462:72 fib(5) = 5
                11:11:27.686 VM462:73 fib(5) = 5
                11:11:27.686 VM462:74 fib(5) = 5
                11:11:27.687 VM462:75 fib(5) = 5
                11:11:27.687 VM462:4 My girl's name is Belly;
                11:11:27.687 VM462:4 My girl's name is Lisa;
                11:11:27.687 VM462:4 My girl's name is Amy;
                11:11:27.687 VM462:4 My girl's name is Janny;
                11:11:27.688 VM462:44 done!
#### 6) co处理并发的异步操作
> - co 支持并发的异步操作，即允许某些操作同时进行，等到它们全部完成，才进行下一步。这时，要把并发的操作都放在数组或对象里面，跟在yield语句后面。 
> - 需要generator和promise的配合
> - 需要用到Promise.race(）或者Promise.all()的方法
> - co模块其实就是一个执行器，以下co的源码：
        
                function co(gen) {
                    var ctx = this;

                    return new Promise(function(resolve, reject) {
                        if (typeof gen === 'function') gen = gen.call(ctx);
                        if (!gen || typeof gen.next !== 'function') return resolve(gen);

                        onFulfilled();
                        function onFulfilled(res) {
                            var ret;
                              try {
                                ret = gen.next(res);
                              } catch (e) {
                                return reject(e);
                              }
                              next(ret);
                        }
                    });
                }

                // toPromise到底是什么鬼？
                function next(ret) {

                    // 检查当前是否为 Generator 函数的最后一步，如果是就返回。
                    if (ret.done) return resolve(ret.value);

                    //确保每一步的返回值，是 Promise 对象。
                    var value = toPromise.call(ctx, ret.value);

                    // 使用then方法，为返回值加上回调函数，然后通过onFulfilled函数再次调用next函数。
                    if (value && isPromise(value)) return value.then(onFulfilled, onRejected);

                    // 在参数不符合要求的情况下（参数非 Thunk 函数和 Promise 对象），将 Promise 对象的状态改为rejected，从而终止执行。
                    return onRejected(
                        new TypeError(
                              'You may only yield a function, promise, generator, array, or object, '
                              + 'but the following object was passed: "'
                              + String(ret.value)
                              + '"'
                            )
                        );
                }

        
<h3 id='6.5'>6.5 async函数</h3>
        
#### 1) 简介
> - ES2017 标准引入了 async 函数，使得异步操作变得更加方便。
> - 实质上是Generator 函数的语法糖。
#### 2) 基本用法
> - async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。
> - 其实async函数返回的是一个promise对象，然后async函数的返回值就是then()方法里面函数的参数； 
> - async函数返回的 Promise 对象，必须等到内部所有await命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到return语句或者抛出错误。也就是说，只有async函数内部的异步操作执行完，才会执行then方法指定的回调函数。
> - 正常情况下，await命令后面是一个 Promise 对象。如果不是，会被转成一个 Promise 对象，然后立即执行resolve()方法 ，也就是说await命令后面的命令就成为了resolve()方法并被立即执行，相当于then()方法
> - await命令后面的 Promise 对象如果变为reject状态，则reject的参数会被catch方法的回调函数接收到。如果是里面的catch接收到，那么就会继续运行，如果是被外面的catch捕捉到，那么就会停止运行，
> - await命令后面的 Promise 对象如果变为resolve状态，但是单纯的resolve状态并不能被接收到，需要与return配合。
> - 如果是单纯的出错，throw 前不需要await，抛出的错误将由then()方法的reject回调函数接收，此时异步操作会被中断，如果不想被中断，需要用到try...catch结构
        
                var findGF = async function(wealth) {
                    let a = await (function() {console.log("The girl is beautiful;");})();
                    let b = await (function() {console.log("The girl is cute;");})();
                    let c = await (function() {console.log("The girl is warm;");})();
                    
                    if(wealth < 0) {
                        try{
                            throw new Error("Oh no! 小伙子你是负资产啊，滚粗！");
                        }catch(e) {
                            console.log(e.toString());
                            return "问世间情为何物，直教人生死相许";
                        }
                    }else if(wealth < 100) {
                        console.log("awkward！小伙子太穷了，被拒绝！");
                        let d = await Promise.reject("问世间情为何物，直教人生死相许");
                    }else {
                        let f = await Promise.resolve("窈窕淑女，君子好逑！愿有情人终成眷属");
                        return f;
                    }
                }
                findGF(101).then(resolve,reject).catch(function(str) {console.log(str);}); 

                var resolve = function(str) {
                    console.log(str);
                }

                var reject = function(str) {
                    console.log(str);
                }

                21:50:34.282 VM42960:2 The girl is beautiful;
                21:50:34.283 VM42960:3 The girl is cute;
                21:50:34.283 VM42960:4 The girl is warm;
                21:50:34.283 VM42960:23 窈窕淑女，君子好逑！愿有情人终成眷属
                21:50:34.299 Promise {<resolved>: undefined}
                21:50:48.651 findGF(99).then(resolve,reject).catch(function(str) {console.log(str);});
                21:50:48.651 VM42960:2 The girl is beautiful;
                21:50:48.651 VM42960:3 The girl is cute;
                21:50:48.651 VM42960:4 The girl is warm;
                21:50:48.651 VM42960:14 awkward！小伙子太穷了，被拒绝！
                21:50:48.652 VM42960:27 问世间情为何物，直教人生死相许
                21:50:48.663 Promise {<resolved>: undefined}
                21:50:53.571 findGF(-100).then(resolve,reject).catch(function(str) {console.log(str);});
                21:50:53.571 VM42960:2 The girl is beautiful;
                21:50:53.572 VM42960:3 The girl is cute;
                21:50:53.572 VM42960:4 The girl is warm;
                21:50:53.572 VM42960:10 Error: Oh no! 小伙子你是负资产啊，滚粗！
                21:50:53.572 VM42960:23 问世间情为何物，直教人生死相许
                21:50:53.583 Promise {<resolved>: undefined}
> - 复杂的版本
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var findGF = async function(wealth) {
                    let a = await (function() {let a = fib(10); console.log("The girl is beautiful;");return a;})();
                    let b = await (function() {let a = fib(20); console.log("The girl is cute;");return a;})();
                    let c = await (function() {let a = fib(30); console.log("The girl is warm;");return a;})();

                    if(wealth < 0) {
                        try{
                            throw new Error("Oh no! 小伙子你是负资产啊，滚粗！"+ a + b + c);
                        }catch(e) {
                            console.log(e.toString());
                            return "问世间情为何物，直教人生死相许"+ a + b + c;
                        }
                    }else if(wealth < 100) {
                        console.log("awkward！小伙子太穷了，被拒绝！");
                        let d = await Promise.reject("问世间情为何物，直教人生死相许"+ a + b + c);
                    }else {
                        let f = await Promise.resolve("窈窕淑女，君子好逑！愿有情人终成眷属"+ a + b + c);
                        return f;
                    }
                }

                var resolve = function(str) {
                    console.log(str);
                }

                var reject = function(str) {
                    console.log(str);
                }

                findGF(101).then(resolve,reject).catch(function(str) {console.log(str);});

                      
                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF(99).then(resolve,reject).catch(function(str) {console.log(str);});


                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF(-100).then(resolve,reject).catch(function(str) {console.log(str);});
                12:51:41.783 VM647:6 The girl is beautiful;
                12:51:41.783 VM647:38 fib(10) = 55
                12:51:41.783 VM647:39 随便写点什么东西，看看是不是提前运行了
                12:51:41.783 VM647:40 fib(5) = 5
                12:51:41.784 VM647:41 fib(5) = 5
                12:51:41.784 VM647:42 fib(5) = 5
                12:51:41.784 VM647:43 fib(5) = 5
                12:51:41.784 VM647:44 fib(5) = 5
                12:51:41.784 VM647:45 fib(5) = 5
                12:51:41.784 VM647:46 fib(5) = 5
                12:51:41.784 VM647:47 fib(5) = 5
                12:51:41.784 VM647:48 fib(5) = 5
                12:51:41.784 VM647:6 The girl is beautiful;
                12:51:41.786 VM647:54 fib(20) = 6765
                12:51:41.786 VM647:55 随便写点什么东西，看看是不是提前运行了
                12:51:41.786 VM647:56 fib(5) = 5
                12:51:41.786 VM647:57 fib(5) = 5
                12:51:41.786 VM647:58 fib(5) = 5
                12:51:41.786 VM647:59 fib(5) = 5
                12:51:41.786 VM647:60 fib(5) = 5
                12:51:41.786 VM647:61 fib(5) = 5
                12:51:41.786 VM647:62 fib(5) = 5
                12:51:41.786 VM647:63 fib(5) = 5
                12:51:41.786 VM647:64 fib(5) = 5
                12:51:41.786 VM647:6 The girl is beautiful;
                12:51:41.787 VM647:7 The girl is cute;
                12:51:41.787 VM647:7 The girl is cute;
                12:51:41.787 VM647:7 The girl is cute;
                12:51:41.801 VM647:8 The girl is warm;
                12:51:41.815 VM647:8 The girl is warm;
                12:51:41.829 VM647:8 The girl is warm;
                12:51:41.829 VM647:18 awkward！小伙子太穷了，被拒绝！
                12:51:41.829 VM647:14 Error: Oh no! 小伙子你是负资产啊，滚粗！556765832040
                12:51:41.829 VM647:27 问世间情为何物，直教人生死相许556765832040
                12:51:41.830 VM647:27 窈窕淑女，君子好逑！愿有情人终成眷属556765832040
                12:51:41.830 VM647:31 
> - 体现异步的版本
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = function() {
                    let a = fib(3);
                    console.log("I am learning JS!");
                    return a;
                }

                var kitFit = function() {
                    let a = fib(2);
                    console.log("I am doing some exercise!");
                    return a;
                }

                var makeMoney = function() {
                    let a = fib(1);
                    console.log("I am earning money!");
                    return a;
                }

                var fight = function* () {
                    yield makeMoney;
                    yield kitFit;
                    yield studyJS;
                }

                var findGF = async function(name) {

                    var f = fight();
                    await f.next().value();
                    await f.next().value();
                    await f.next().value();
                    return `I love ${name}!`;
                }

                findGF("Lily").then(function(str) {console.log(str);});

                console.log(`fib(18) = ${fib(18)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);


                11:08:25.087 VM460:19 I am earning money!
                11:08:25.087 VM460:40 fib(18) = 2584
                11:08:25.088 VM460:41 随便写点什么东西，看看是不是提前运行了
                11:08:25.088 VM460:42 fib(5) = 5
                11:08:25.088 VM460:43 fib(5) = 5
                11:08:25.088 VM460:44 fib(5) = 5
                11:08:25.088 VM460:45 fib(5) = 5
                11:08:25.088 VM460:46 fib(5) = 5
                11:08:25.088 VM460:47 fib(5) = 5
                11:08:25.088 VM460:48 fib(5) = 5
                11:08:25.088 VM460:49 fib(5) = 5
                11:08:25.088 VM460:50 fib(5) = 5
                11:08:25.088 VM460:13 I am doing some exercise!
                11:08:25.088 VM460:7 I am learning JS!
                11:08:25.088 VM460:38 I love Lily!
> - 
#### 3) 并发异步执行
> - Promise.all([{}, {}, {}])版本 
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = function() {
                    let a = fib(10);
                    console.log("I am learning JS!");
                    return a;
                }

                var kitFit = function() {
                    let a = fib(20);
                    console.log("I am doing some exercise!");
                    return a;
                }

                var makeMoney = function() {
                    let a = fib(30);
                    console.log("I am earning money!");
                    return a;
                }

                var findGF = async function(name) {
                    let a = await Promise.all([ kitFit(), makeMoney(), studyJS()]);
                    await console.log("finghting~" + " " + a[0] + " " + a[1] + " " + a[2]);
                    return Promise.resolve(`${name} I love you!`);
                }

                findGF("Lily").then(function(str) {console.log(str);});

                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF("Llsa").then(function(str) {console.log(str);});

                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);


                12:07:11.467 VM620:13 I am doing some exercise!
                12:07:11.481 VM620:19 I am earning money!
                12:07:11.481 VM620:7 I am learning JS!
                12:07:11.481 VM620:32 fib(10) = 55
                12:07:11.481 VM620:33 随便写点什么东西，看看是不是提前运行了
                12:07:11.481 VM620:34 fib(5) = 5
                12:07:11.482 VM620:35 fib(5) = 5
                12:07:11.482 VM620:36 fib(5) = 5
                12:07:11.482 VM620:37 fib(5) = 5
                12:07:11.482 VM620:38 fib(5) = 5
                12:07:11.482 VM620:39 fib(5) = 5
                12:07:11.482 VM620:40 fib(5) = 5
                12:07:11.482 VM620:41 fib(5) = 5
                12:07:11.482 VM620:42 fib(5) = 5
                12:07:11.482 VM620:13 I am doing some exercise!
                12:07:11.497 VM620:19 I am earning money!
                12:07:11.497 VM620:7 I am learning JS!
                12:07:11.498 VM620:47 fib(20) = 6765
                12:07:11.498 VM620:48 随便写点什么东西，看看是不是提前运行了
                12:07:11.498 VM620:49 fib(5) = 5
                12:07:11.498 VM620:50 fib(5) = 5
                12:07:11.498 VM620:51 fib(5) = 5
                12:07:11.498 VM620:52 fib(5) = 5
                12:07:11.498 VM620:53 fib(5) = 5
                12:07:11.498 VM620:54 fib(5) = 5
                12:07:11.498 VM620:55 fib(5) = 5
                12:07:11.498 VM620:56 fib(5) = 5
                12:07:11.498 VM620:57 fib(5) = 5
                12:07:11.498 VM620:25 finghting~ 6765 832040 55
                12:07:11.498 VM620:25 finghting~ 6765 832040 55
                12:07:11.499 VM620:29 Lily I love you!
                12:07:11.499 VM620:44 Llsa I love you!

> - for...of版本，本质上跟Promise.all([{}, {}, {}])版本相同，for...of是进行一部异步的通用方法；
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = function() {
                    let a = fib(10);
                    console.log("I am learning JS!");
                    return a;
                }

                var kitFit = function() {
                    let a = fib(20);
                    console.log("I am doing some exercise!");
                    return a;
                }

                var makeMoney = function() {
                    let a = fib(30);
                    console.log("I am earning money!");
                    return a;
                }

                var findGF = async function(name) {
                    let func = [kitFit, makeMoney, studyJS];
                    let a = [];
                    for(let f of func) {
                        a.push(await f());
                    }
                    await console.log("finghting~" + " " + a[0] + " " + a[1] + " " + a[2]);
                    return Promise.resolve(`${name} I love you!`);
                }

                findGF("Lily").then(function(str) {console.log(str);});

                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF("Llsa").then(function(str) {console.log(str);});

                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);


                12:09:28.366 VM621:13 I am doing some exercise!
                12:09:28.366 VM621:36 fib(10) = 55
                12:09:28.366 VM621:37 随便写点什么东西，看看是不是提前运行了
                12:09:28.366 VM621:38 fib(5) = 5
                12:09:28.366 VM621:39 fib(5) = 5
                12:09:28.366 VM621:40 fib(5) = 5
                12:09:28.366 VM621:41 fib(5) = 5
                12:09:28.366 VM621:42 fib(5) = 5
                12:09:28.366 VM621:43 fib(5) = 5
                12:09:28.366 VM621:44 fib(5) = 5
                12:09:28.366 VM621:45 fib(5) = 5
                12:09:28.366 VM621:46 fib(5) = 5
                12:09:28.367 VM621:13 I am doing some exercise!
                12:09:28.367 VM621:51 fib(20) = 6765
                12:09:28.367 VM621:52 随便写点什么东西，看看是不是提前运行了
                12:09:28.367 VM621:53 fib(5) = 5
                12:09:28.367 VM621:54 fib(5) = 5
                12:09:28.367 VM621:55 fib(5) = 5
                12:09:28.367 VM621:56 fib(5) = 5
                12:09:28.367 VM621:57 fib(5) = 5
                12:09:28.367 VM621:58 fib(5) = 5
                12:09:28.367 VM621:59 fib(5) = 5
                12:09:28.367 VM621:60 fib(5) = 5
                12:09:28.367 VM621:61 fib(5) = 5
                12:09:28.381 VM621:19 I am earning money!
                12:09:28.397 VM621:19 I am earning money!
                12:09:28.397 VM621:7 I am learning JS!
                12:09:28.397 VM621:7 I am learning JS!
                12:09:28.397 VM621:29 finghting~ 6765 832040 55
                12:09:28.397 VM621:29 finghting~ 6765 832040 55
                12:09:28.397 VM621:33 Lily I love you!
                12:09:28.397 VM621:48 Llsa I love you!

> - Promise.race()版本
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = function() {
                    let a = fib(10);
                    console.log("I am learning JS!");
                    return a;
                }

                var kitFit = function() {
                    let a = fib(20);
                    console.log("I am doing some exercise!");
                    return a;
                }

                var makeMoney = function() {
                    let a = fib(30);
                    console.log("I am earning money!");
                    return a;
                }

                var findGF = async function(name) {
                    let a = await Promise.race([makeMoney(), kitFit(), studyJS()]);
                    await console.log("finghting~" + " " + a);
                    return Promise.resolve(`${name} I love you!`);
                }

                findGF("Lily").then(function(str) {console.log(str);});

                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF("Llsa").then(function(str) {console.log(str);});

                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);


                12:11:10.941 VM622:19 I am earning money!
                12:11:10.942 VM622:13 I am doing some exercise!
                12:11:10.942 VM622:7 I am learning JS!
                12:11:10.942 VM622:32 fib(10) = 55
                12:11:10.942 VM622:33 随便写点什么东西，看看是不是提前运行了
                12:11:10.942 VM622:34 fib(5) = 5
                12:11:10.942 VM622:35 fib(5) = 5
                12:11:10.942 VM622:36 fib(5) = 5
                12:11:10.942 VM622:37 fib(5) = 5
                12:11:10.942 VM622:38 fib(5) = 5
                12:11:10.942 VM622:39 fib(5) = 5
                12:11:10.942 VM622:40 fib(5) = 5
                12:11:10.942 VM622:41 fib(5) = 5
                12:11:10.942 VM622:42 fib(5) = 5
                12:11:10.956 VM622:19 I am earning money!
                12:11:10.956 VM622:13 I am doing some exercise!
                12:11:10.956 VM622:7 I am learning JS!
                12:11:10.956 VM622:47 fib(20) = 6765
                12:11:10.956 VM622:48 随便写点什么东西，看看是不是提前运行了
                12:11:10.957 VM622:49 fib(5) = 5
                12:11:10.957 VM622:50 fib(5) = 5
                12:11:10.957 VM622:51 fib(5) = 5
                12:11:10.957 VM622:52 fib(5) = 5
                12:11:10.957 VM622:53 fib(5) = 5
                12:11:10.957 VM622:54 fib(5) = 5
                12:11:10.957 VM622:55 fib(5) = 5
                12:11:10.957 VM622:56 fib(5) = 5
                12:11:10.957 VM622:57 fib(5) = 5
                12:11:10.957 VM622:25 finghting~ 832040
                12:11:10.957 VM622:25 finghting~ 832040
                12:11:10.957 VM622:29 Lily I love you!
                12:11:10.957 VM622:44 Llsa I love you!

     
#### 4) 异步遍历器
> - 一般来讲，generator的next方法必须是同步的，只要调用就必须立刻返回值。也就是说，一旦执行next方法，就必须同步地得到value和done这两个属性。
> - 如果遍历指针正好指向同步操作当然没有问题，但对于异步操作，就不太合适了。
> - 目前的解决方法是，Generator 函数里面的异步操作，返回一个 Thunk 函数或者 Promise 对象，即value属性是一个 Thunk 函数或者 Promise 对象，等待以后返回真正的值，而done属性则还是同步产生的。
> - 为了做到这一点，关键是让next()方法返回Promise对象，并把它放在await命令后面。
> - 由于next方法用await处理，就不必使用then方法了，整个流程已经很接近同步处理了。
> - 同步遍历器是部署在Symbol.iterator属性上的
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = new Promise(function(resolve, reject) {
                                            let a = fib(30);
                                            console.log("I am learning JS!");
                                            resolve(`fib(30) = ${a} and I am learning JS!`)} );
                var kitFit = new Promise(function(resolve, reject) {
                                            let a = fib(20);
                                            console.log("I am doing some exercise!");
                                            resolve(`fib(20) = ${a} and I am doing some exercise!`)} );
                var makeMoney = new Promise(function(resolve, reject) {
                                            let a = fib(10);
                                            console.log("I am earning money!");
                                            resolve(`fib(10) = ${a} and I am earning money!`)} );

                var fight = function* () {
                    yield makeMoney;
                    yield kitFit;
                    yield studyJS;
                }

                var findGF = async function(name) {

                    var f = fight();
                    //for await (const x of f) {
                    //  console.log(x);
                    //}
                    await f.next().value.then(function(val) {console.log(val);});
                    await f.next().value.then(function(val) {console.log(val);});
                    await f.next().value.then(function(val) {console.log(val);});
                    return `I love ${name}!`;
                }

                findGF("Lily").then(function(str) {console.log(str);});

                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF("Llsa").then(function(str) {console.log(str);});

                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                12:18:54.884 VM629:7 I am learning JS!
                12:18:54.884 VM629:11 I am doing some exercise!
                12:18:54.884 VM629:15 I am earning money!
                12:18:54.884 VM629:39 fib(10) = 55
                12:18:54.885 VM629:40 随便写点什么东西，看看是不是提前运行了
                12:18:54.885 VM629:41 fib(5) = 5
                12:18:54.885 VM629:42 fib(5) = 5
                12:18:54.885 VM629:43 fib(5) = 5
                12:18:54.885 VM629:44 fib(5) = 5
                12:18:54.885 VM629:45 fib(5) = 5
                12:18:54.885 VM629:46 fib(5) = 5
                12:18:54.885 VM629:47 fib(5) = 5
                12:18:54.885 VM629:48 fib(5) = 5
                12:18:54.885 VM629:49 fib(5) = 5
                12:18:54.885 VM629:54 fib(20) = 6765
                12:18:54.885 VM629:55 随便写点什么东西，看看是不是提前运行了
                12:18:54.885 VM629:56 fib(5) = 5
                12:18:54.885 VM629:57 fib(5) = 5
                12:18:54.885 VM629:58 fib(5) = 5
                12:18:54.885 VM629:59 fib(5) = 5
                12:18:54.886 VM629:60 fib(5) = 5
                12:18:54.886 VM629:61 fib(5) = 5
                12:18:54.886 VM629:62 fib(5) = 5
                12:18:54.886 VM629:63 fib(5) = 5
                12:18:54.886 VM629:64 fib(5) = 5
                12:18:54.886 VM629:30 fib(10) = 55 and I am earning money!
                12:18:54.886 VM629:30 fib(10) = 55 and I am earning money!
                12:18:54.886 VM629:31 fib(20) = 6765 and I am doing some exercise!
                12:18:54.886 VM629:31 fib(20) = 6765 and I am doing some exercise!
                12:18:54.886 VM629:32 fib(30) = 832040 and I am learning JS!
                12:18:54.886 VM629:32 fib(30) = 832040 and I am learning JS!
                12:18:54.886 VM629:36 I love Lily!
                12:18:54.887 VM629:51 I love Llsa!
#### 5) for await...of 
> - for...of循环用于遍历同步的 Iterator 接口。新引入的for await...of循环，则是用于遍历异步的 Iterator 接口。
> - 每个子值代表的是迭代器的值实现方法后返回参数，如果没有方法直接返回该数值；
        
                var fib = function(n) {
                    return n<2 ? n : fib(n-1)+fib(n-2);
                }

                var studyJS = new Promise(function(resolve, reject) {
                                            let a = fib(30);
                                            console.log("I am learning JS!");
                                            resolve(`fib(30) = ${a} and I am learning JS!`)} );
                var kitFit = new Promise(function(resolve, reject) {
                                            let a = fib(20);
                                            console.log("I am doing some exercise!");
                                            resolve(`fib(20) = ${a} and I am doing some exercise!`)} );
                var makeMoney = new Promise(function(resolve, reject) {
                                            let a = fib(10);
                                            console.log("I am earning money!");
                                            resolve(`fib(10) = ${a} and I am earning money!`)} );

                var fight = function* () {
                    yield makeMoney;
                    yield kitFit;
                    yield studyJS;
                }

                var findGF = async function(name) {

                    var f = fight();
                    for await (const x of f) {
                      console.log(x);
                    }
                    // await f.next().value.then(function(val) {console.log(val);});
                    // await f.next().value.then(function(val) {console.log(val);});
                    // await f.next().value.then(function(val) {console.log(val);});
                    return `I love ${name}!`;
                }

                findGF("Lily").then(function(str) {console.log(str);});

                // 第一组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(10) = ${fib(10)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);

                findGF("Llsa").then(function(str) {console.log(str);});

                // 第二组 随便写点什么东西，看看是不是提前运行了
                console.log(`fib(20) = ${fib(20)}`);
                console.log("随便写点什么东西，看看是不是提前运行了");
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                console.log(`fib(5) = ${fib(5)}`);
                12:20:31.201 VM630:7 I am learning JS!
                12:20:31.202 VM630:11 I am doing some exercise!
                12:20:31.202 VM630:15 I am earning money!
                12:20:31.202 VM630:39 fib(10) = 55
                12:20:31.202 VM630:40 随便写点什么东西，看看是不是提前运行了
                12:20:31.203 VM630:41 fib(5) = 5
                12:20:31.203 VM630:42 fib(5) = 5
                12:20:31.203 VM630:43 fib(5) = 5
                12:20:31.203 VM630:44 fib(5) = 5
                12:20:31.203 VM630:45 fib(5) = 5
                12:20:31.203 VM630:46 fib(5) = 5
                12:20:31.203 VM630:47 fib(5) = 5
                12:20:31.203 VM630:48 fib(5) = 5
                12:20:31.203 VM630:49 fib(5) = 5
                12:20:31.204 VM630:54 fib(20) = 6765
                12:20:31.204 VM630:55 随便写点什么东西，看看是不是提前运行了
                12:20:31.204 VM630:56 fib(5) = 5
                12:20:31.204 VM630:57 fib(5) = 5
                12:20:31.204 VM630:58 fib(5) = 5
                12:20:31.204 VM630:59 fib(5) = 5
                12:20:31.204 VM630:60 fib(5) = 5
                12:20:31.204 VM630:61 fib(5) = 5
                12:20:31.204 VM630:62 fib(5) = 5
                12:20:31.205 VM630:63 fib(5) = 5
                12:20:31.205 VM630:64 fib(5) = 5
                12:20:31.205 VM630:28 fib(10) = 55 and I am earning money!
                12:20:31.205 VM630:28 fib(10) = 55 and I am earning money!
                12:20:31.205 VM630:28 fib(20) = 6765 and I am doing some exercise!
                12:20:31.205 VM630:28 fib(20) = 6765 and I am doing some exercise!
                12:20:31.205 VM630:28 fib(30) = 832040 and I am learning JS!
                12:20:31.205 VM630:28 fib(30) = 832040 and I am learning JS!
                12:20:31.206 VM630:36 I love Lily!
                12:20:31.206 VM630:51 I love Llsa!
#### 6) 比较各种同步异步的方法
> - 普通函数、async 函数（异步执行，因为await返回的是promise对象）、Generator 函数（同步执行）和异步 Generator 函数(与promise对象结合，或者执行器是promise)。请注意区分每种函数的不同之处。基本上，如果是一系列按照顺序执行的异步操作（比如读取文件，然后写入新内容，再存入硬盘），可以使用 async 函数；如果是一系列产生相同数据结构的异步操作（比如一行一行读取文件），可以使用异步 Generator 函数。

------      
        

<h2 id='7'>七、类和class</h2>
<h3 id='7.1'>7.1 继承 详细看<a href="https://www.cnblogs.com/humin/p/4556820.html">幻天芒的博客</a></h3>  

#### 1) 原型链继承 
> - 原型链继承 核心： 将父类的实例作为子类的原型
> - 特点：
>> - 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
>> - 父类新增原型方法/原型属性，子类都能访问到
>> - 简单，易于实现
> - 缺点
>> - 要想为子类新增属性和方法，必须要在new Animal()这样的语句之后执行，不能放到构造器中
>> - 无法实现多继承
>> - 来自原型对象的引用属性是所有实例共享的（详细请看附录代码： 示例1）
>> - 创建子类实例时，无法向父类构造函数传参
        
                var _prototype_ = {
                    constructor: _construct_,
                    toString: function() {
                        console.log("name: " + this.name + "id: " + this.id);
                    },
                };
                var _construct_ = function(name, id) {
                    this.name = name;
                    this.id = id;

                    // Static method
                    this.sing = function() {
                        console.log(this.name + " sing a song!");
                    };
                }
                _construct_.prototype = _prototype_;
                
                // 原型链继承 核心： 将父类的实例作为子类的原型
                var boy = new _construct_("lvhongbin", 116020910160);
                boy.toString();
                boy.sing();

                var son = function(sex) {
                    this.sex = sex;
                    //static method
                    this.mySex = function() {
                        console.log( "My sex is " + this.sex);
                    }
                }

                son.prototype = new _construct_("lvhongchao", 10);
                var jack = new son("male");
                jack.toString();
                jack.sing();
                jack.mySex();
                son.prototype = new _construct_("lvhongbin", 10);
                var lily = new son("female");
                lily.toString();
                lily.sing();
                lily.mySex();

                //结果
                14:50:50.932 VM97:4 name: lvhongbinid: 116020910160
                14:50:50.932 VM97:13 lvhongbin sing a song!
                14:50:50.932 VM97:4 name: lvhongchaoid: 10
                14:50:50.933 VM97:13 lvhongchao sing a song!
                14:50:50.933 VM97:26 My sex is male
                14:50:50.944 undefined
                14:53:46.569 jack instanceof son;
                14:53:46.571 true
                14:54:18.001 jack instanceof _construct_;
                14:54:18.004 true
                14:54:39.756 jack instanceof Array;
                14:54:39.757 false
                jack instanceof _prototype_;
                14:55:13.831 VM122:1 Uncaught TypeError: Right-hand side of 'instanceof' is not callable
                    at <anonymous>:1:6              
                16:43:08.160 VM149:4 name: lvhongbinid: 10
                16:43:08.160 VM149:13 lvhongbin sing a song!
                16:43:08.160 VM149:27 My sex is female
                console.log(jack.sing() == lily.sing());
                16:45:14.047 VM149:13 lvhongchao sing a song!
                16:45:14.047 VM149:13 lvhongbin sing a song!
                16:45:14.047 VM163:1 true
                16:45:14.053 undefined
                16:46:32.184 console.log(jack.sing == lily.sing);
                16:46:32.184 VM169:1 false
                16:46:32.189 undefined
                16:46:43.463 console.log(jack.toString == lily.toString);
                16:46:43.463 VM175:1 true
#### 2) 类继承/构造继承 
> - 用父类的构造函数来增强子类实例，等于是复制父类的实例属性给子类（没用到原型）
> - 特点：
>> - 解决了1中，子类实例共享父类引用属性的问题
>> - 创建子类实例时，可以向父类传递参数
>> - 可以实现多继承（call多个父类对象）
> - 缺点：
>> - 实例并不是父类的实例，只是子类的实例
>> - 只能继承父类的实例属性和方法，不能继承原型属性/方法
>> - 无法实现函数复用，每个子类都有父类实例函数的副本，影响性能
        
                var _prototype_ = {
                    constructor: _construct_,
                    toString: function() {
                        console.log("name: " + this.name + "id: " + this.id);
                    },
                };
                var _construct_ = function(name, id) {
                    this.name = name;
                    this.id = id;

                    // Static method
                    this.sing = function() {
                        console.log(this.name + " sing a song!");
                    };
                }
                _construct_.prototype = _prototype_;
                
                // 构造继承
                var boy = new _construct_("lvhongbin", 116020910160);
                boy.toString();
                boy.sing();

                var son = function(name, id, sex) {
                    _construct_.call(this, name, id);
                    this.sex = sex;
                    //static method
                    this.mySex = function() {
                        console.log( "My sex is " + this.sex);
                    }
                }

                var jack = new son("lvhongchao", 10,"male");
                jack.toString();
                jack.sing();
                jack.mySex();

                15:26:06.952 VM129:13 name: lvhongbinid: 116020910160
                15:26:06.952 VM129:13 lvhongbin sing a song!
                15:28:24.199 "[object Object]" //父类原型的方法无法继承
                15:26:06.952 VM129:13 lvhongchao sing a song!
                15:26:06.952 VM129:28 My sex is male
                15:26:06.968 undefined
                15:26:46.851 jack instanceof son;
                15:26:46.853 true
                15:27:01.446 jack instanceof _construct_;
                15:27:01.448 false
                15:27:09.249 jack instanceof _prototype_;
                15:27:09.250 VM135:1 Uncaught TypeError: Right-hand side of 'instanceof' is not callable
                    at <anonymous>:1:6
#### 3) 实例继承 
> - 为父类实例添加新特性，作为子类实例返回
> - 特点：
>> - 不限制调用方式，不管是new 子类()还是子类(),返回的对象具有相同的效果
> - 缺点：
>> - 实例是父类的实例，不是子类的实例
>> - 不支持多继承
        
                var _prototype_ = {
                    constructor: _construct_,
                    toString: function() {
                        console.log("name: " + this.name + "id: " + this.id);
                    },
                };
                var _construct_ = function(name, id) {
                    this.name = name;
                    this.id = id;

                    // Static method
                    this.sing = function() {
                        console.log(this.name + " sing a song!");
                    };
                }
                _construct_.prototype = _prototype_;
                
                var boy = new _construct_("lvhongbin", 116020910160);
                boy.toString();
                boy.sing();
                
                // 实例继承
                var boy = function(name, id, sex) {
                    var instance = new _construct_(name, id); 
                    instance.sex = sex;
                    //static method
                    instance.mySex = function() {
                        console.log( "My sex is " + this.sex);
                    }
                    return instance;
                }

                var jack = new boy("lvhongchao", 10,"male");
                jack.toString();
                jack.sing();
                jack.mySex();

                //结果
                name: lvhongbinid: 116020910160
                15:42:59.082 VM139:13 lvhongbin sing a song!
                15:42:59.082 VM139:4 name: lvhongchaoid: 10
                15:42:59.082 VM139:13 lvhongchao sing a song!
                15:42:59.083 VM139:28 My sex is male
                15:42:59.093 undefined
                15:43:27.109 jack instanceof son;
                15:43:27.111 false
                15:43:58.535 jack instanceof _construct_;
                15:43:58.538 true
                15:44:07.190 jack instanceof _prototype_;
                15:44:07.193 VM145:1 Uncaught TypeError: Right-hand side of 'instanceof' is not callable
                    at <anonymous>:1:6
#### 4) 拷贝继承 
> - 特点：
>> - 支持多继承
> - 缺点：
>> - 效率较低，内存占用高（因为要拷贝父类的属性）
>> - 无法获取父类不可枚举的方法（不可枚举方法，不能使用for in 访问到）
        
                function Cat(name){
                  var animal = new Animal();
                  for(var p in animal){
                    Cat.prototype[p] = animal[p];
                  }
                  Cat.prototype.name = name || 'Tom';
                }

                // Test Code
                var cat = new Cat();
                console.log(cat.name);
                console.log(cat.sleep());
                console.log(cat instanceof Animal); // false
                console.log(cat instanceof Cat); // true
#### 5) 组合继承 
> - 通过调用父类构造，继承父类的属性并保留传参的优点，然后通过将父类实例作为子类原型，实现函数复用
> - 特点：
>> - 弥补了方式2的缺陷，可以继承实例属性/方法，也可以继承原型属性/方法
>> - 既是子类的实例，也是父类的实例
>> - 不存在引用属性共享问题
>> - 可传参
>> - 函数可复用
> - 缺点：
>> - 调用了两次父类构造函数，生成了两份实例（子类实例将子类原型上的那份屏蔽了）会比较好性能
        
                function Cat(name){
                  Animal.call(this);
                  this.name = name || 'Tom';
                }

                // 参数为空 代表 父类的实例和父类的原型对象的关系了  
                Cat.prototype = new Animal();

                // 感谢 @学无止境c 的提醒，组合继承也是需要修复构造函数指向的。

                Cat.prototype.constructor = Cat;

                // Test Code
                var cat = new Cat();
                console.log(cat.name);
                console.log(cat.sleep());
                console.log(cat instanceof Animal); // true
                console.log(cat instanceof Cat); // true
#### 6) 寄生组合继承 
> - 通过寄生方式，砍掉父类的实例属性，这样，在调用两次父类的构造的时候，就不会初始化两次实例方法/属性，避免的组合继承的缺点
        
                function Cat(name){
                  Animal.call(this);
                  this.name = name || 'Tom';
                }
                (function(){
                  // 创建一个没有实例方法的类
                  var Super = function(){};
                  Super.prototype = Animal.prototype;
                  //将实例作为子类的原型
                  Cat.prototype = new Super();
                })();

                // Test Code
                var cat = new Cat();
                console.log(cat.name);
                console.log(cat.sleep());
                console.log(cat instanceof Animal); // true
                console.log(cat instanceof Cat); //true

        
<h3 id='7.2'>7.2 class的使用</h3>  
        
#### 1) class模板 
> - toString方法是Point类内部定义的方法，它是不可枚举
        class Person {
            constructor(name, id) {
                this.name = name;
                this.id = id;
            }

            toString() {
                console.log("name: " + this.name + "id: " + this.id);
            }

            sing() {
                console.log(this.name + " sing a song!");
            }
        }

        var boy = new Person("lvhongbin", 116020910160);
        boy.toString();
        boy.sing();
        16:58:31.072 VM177:8 name: lvhongbinid: 116020910160
        16:58:31.072 VM177:12 lvhongbin sing a song!
        16:58:31.085 undefined
        16:59:07.335 boy instanceof Person;
        16:59:07.337 true
        Object.keys(Person.prototype)
        // []
        17:03:09.890 []
        17:03:15.278 Object.getOwnPropertyNames(Person.prototype)
        // ["constructor","toString"]
        17:03:15.282 (3) ["constructor", "toString", "sing"]
#### 2) 关于Object的相关用法 
> - Object.getOwnPropertyNames(Person.prototype) 自家的所有属性
> - Object.keys(Person.prototype) 自家的不可配置属性
> - Object.getPrototypeOf 获取实例对象的原型
#### 3) class关键字 
> - 需要注意的是，这个类的名字是MyClass而不是Me，Me只在 Class 的内部代码可用，指代当前类。
        
                const Person = class Me {
                  getClassName() {
                    return Me.name;
                  }
                };
                let Lv = new Person();
                Lv.getClassName();
                20:31:17.631 "Me"
#### 4) 不存在变量提升 
> -  字类必须在父类后面进行定义
        
                new Foo(); // ReferenceError
                class Foo {}
                20:35:11.688 VM193:1 Uncaught ReferenceError: Foo is not defined
                    at <anonymous>:1:1
#### 5) 私有方法和私有属性  
> - 私有方法 可惜ES6并没有提供相应的方法，我们只能从民间搜集
>> - 第一种方法：从命名上，私有方法或者属性名字前面加上下划线。但是，这种命名是不保险的，在类的外部，还是可以调用到这个方法。
>> - 第二种方法：把私有方法移出模块，采用call,apply或者bind等方法进行调用，这时候外部便不知道该方法的具体细节
        
                class Widget {
                  foo (baz) {
                    bar.call(this, baz);
                  }

                  // ...
                }

                // 模块被移除类外
                function bar(baz) {
                  return this.snaf = baz;
                }
>> - 采用Symbol的属性
> - 私有属性 用前缀#表示，他既属于一个操作符又属于属性名的一部分
> - 这种写法不仅可以写私有属性，还可以用来写私有方法。
> - 但是这个方法只是一个提案，并没有实施，并没有什么用

#### 6) get和set 
> - 属性有对应的存值函数和取值函数，因此赋值和读取行为都被自定义了。
        
                class MyClass {
                    constructor() {
                        // ...
                    }
                    get prop() {
                        return 'getter';
                    }
                    set prop(value) {
                        console.log('setter: '+value);
                    }
                }

                let inst = new MyClass();

                inst.prop = 123;
                // setter: 123

                inst.prop
                // 'getter'     
#### 7) this的指向
> - 默认指向类的实例
> - 但是，如果将这个方法提取出来单独使用，this会指向该方法运行时所在的环境，因为找不到print方法而导致报错。
> - 一个比较简单的解决方法是，在构造方法中绑定this，这样就不会找不到print方法了。
> - 比如：
        
                class Logger {
                    constructor() {
                        this.printName = this.printName.bind(this);
                    }

                    // ...
                }
> - 第二种方法是使用箭头函数，利用了箭头函数自动绑定this的特性
        
                class Logger {
                    constructor() {
                        this.printName = (name = 'there') => {
                            this.print(`Hello ${name}`);
                        };
                    }

                  // ...
                }
> - 第三种方法 使用Proxy，获取方法的时候，自动绑定this。
                
                function selfish (target) {

                    //采用弱Map的方法，用完即弃
                    const cache = new WeakMap();
                    const handler = {
                        get (target, key) {

                        // 先获取原来的对象属性
                        const value = Reflect.get(target, key);
                        if (typeof value !== 'function') {
                            return value;
                        }
                        if (!cache.has(value)) {
                            cache.set(value, value.bind(target));
                        }
                        return cache.get(value);
                        }
                    };
                    const proxy = new Proxy(target, handler);
                    return proxy;
                }

                    const logger = selfish(new Logger());
#### 8) class的静态方法
> - 类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。
> - 如果静态方法包含this关键字，这个this指的是类，而不是实例。
> - ES6 明确规定，Class 内部只有静态方法，没有静态属性。
        
                class Foo {
                  static bar () {
                    this.baz();
                  }
                  static baz () {
                    console.log('hello');
                  }
                  baz () {
                    console.log('world');
                  }
                }

                Foo.bar() // hello
> - 目前的静态属性仍然是一个提案
 
#### 9) 其他 
> - name 属性 
> - 类和模块的内部，默认就是严格模式，所以不需要使用use strict指定运行模式。只要你的代码写在类或模块之中，就只有严格模式可用。
> - new.target 属性 表示用new方法创建的实例，比如阮老师给出的这个例子，只有通过继承才能使用
        
                class Shape {
                  constructor() {
                    if (new.target === Shape) {
                      throw new Error('本类不能实例化');
                    }
                  }
                }

                class Rectangle extends Shape {
                  constructor(length, width) {
                    super();
                    // ...
                  }
                }

                var x = new Shape();  // 报错
                var y = new Rectangle(3, 4);  // 正确
> - Class 的 Generator 方法，某个方法之前加上星号（*）
        
                class FindGF {

                    constructor(args) {
                        this.args = args;
                    };

                    * generator() {
                        for(let arg of this.args) {
                            yield arg;
                        };
                    };

                    /* 采用self方法进行this的绑定
                        [Symbol.iterator]() {
                            let self = this;
                            self.index = 0;
                            return { next() {
                                return (self.index < self.args.length) ? {value: self.args[self.index++], done: false}: {value: undefined, done: true};
                                }
                            }
                        };
                     */

                     //采用bind方法进行this的绑定
                        [Symbol.iterator]() {
                            this.index = 0;
                            let next = function() {
                                return (this.index < this.args.length) ? {value: this.args[this.index++], done: false}: {value: undefined, done: true};
                            }
                            next = next.bind(this);
                            return {next};
                        };
                }

                var fight = [];
                fight.push("I am learning JS!");
                fight.push("I am doing some exercise!");
                fight.push("I am earning money!");
                var f = new FindGF(fight);
                for(let arg of f.generator()) {
                    console.log(arg);
                }
                for(let arg of f) {
                    console.log(arg);
                }

                16:46:06.734 VM1798:45 I am learning JS!
                16:46:06.734 VM1798:45 I am doing some exercise!
                16:46:06.734 VM1798:45 I am earning money!
                16:46:06.734 VM1798:48 I am learning JS!
                16:46:06.734 VM1798:48 I am doing some exercise!
                16:46:06.734 VM1798:48 I am earning money!


<h3 id='7.3'>7.3 class的继承</h3>

#### 1) 简介 
> - Class 可以通过extends关键字实现继承，这比 ES5 的通过修改原型链实现继承，要清晰和方便很多。
> - 子类必须在constructor方法中调用super方法，否则新建实例时会报错。这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。如果不调用super方法，子类就得不到this对象。
> - super关键字，它在这里表示父类的构造函数
> - 父类的静态方法，也会被子类继承。
        
                class Father {
    
                constructor(age) {
                    this.fatherAge = age;
                    this.height = 170;
                    console.log(`${new.target.name} age is ${this.height}`);
                };

                static hobby(item) {
                    console.log(`My hobby is ${item}`);
                };
            }

            class Son extends Father {
                
                constructor(fatherAge,sonAge) {
                    super(fatherAge);
                    this.sonAge = sonAge;
                };

                add() {
                    return this.fatherAge + this.sonAge;
                };
            }

            Son.hobby("fishing");
            var son = new Son(49, 25);
            son.add();

            17:54:05.909 VM2431:10 My hobby is fishing
            17:54:05.910 VM2431:6 Son age is 170
            17:54:05.919 74
#### 2) super关键字 
> - 两种含义：一种是代表父类的构造函数，另一种是当作对象被使用 
> - 作为父类的构造函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。并且必须出现在子类的构造函数中；
> - 此时super()相当于A.prototype.constructor.call(this)，内部的this依然指的是子类
> - 如果super作为对象，用在静态方法之中，这时super将指向父类的静态函数对象，而不是父类的原型对象。因为构造函数也好，静态方法也好，都在一开始编译的时候就生成了，在这过程中super只指向的父类i
> - > - 如果super作为对象，用在普通方法中，则指向父类的普通方法
> - 如果super作为对象，用在构造函数之中，这时super将指向子类，相当于为子类属性赋值；
> - 或者说，只有用在构造函数或者是静态方法中，super才指向父类
        
                class Parent {
                    static myMethod(msg) {
                        console.log('static', msg);
                    }

                    myMethod(msg) {
                        console.log('instance', msg);
                    }
                }

                class Child extends Parent {
                    static myMethod(msg) {
                        super.myMethod(msg);
                    }

                    myMethod(msg) {
                        super.myMethod(msg);
                    }
                }

                Child.myMethod(1); // static 1

                var child = new Child();
                child.myMethod(2); // instance 2
> - 如果无法识别super()是否作为对象被使用，此时JS就会报错，比如console.log(super)；
#### 3) 类的 prototype 属性和__proto__属性
> - 大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性
> - 实例对象有__proto__属性，而构造函数拥有prototype属性
> - 实例对象的__proto__属性 == 构造函数的prototype属性
> - __proto__是每个对象都有的一个属性，而prototype是函数才会有的属性!!! 
> - 使用Object.getPrototypeOf()代替__proto__!!!
> - prototype和__proto__都指向原型对象，任意一个函数（包括构造函数）都有一个prototype属性，指向该函数的原型对象，同样任意一个构造函数实例化的对象，都有一个__proto__属性（__proto__并非标准属性，ECMA-262第5版将该属性或指针称为[[Prototype]]，可通过Object.getPrototypeOf()标准方法访问该属性），指向构造函数的原型对象。
> - 继承本质上就是以子类以父类的实例对象为原型
        
                Object.setPrototypeOf(B.prototype, A.prototype);
                // 等同于
                B.prototype.__proto__ = A.prototype;

                Object.setPrototypeOf(B, A);
                // 等同于
                B.__proto__ = A;
> - __proto__属性 可称为隐式原型，一个对象的隐式原型指向构造该对象的构造函数的原型，这也保证了实例能够访问在构造函数原型中定义的属性和方法。
#### 4) Mixin 模式的实现
> - Mixin 指的是多个对象合成一个新的对象，新对象具有各个组成成员的接口。
> - 实质上就是复制方法和原型
        
                function mix(...mixins) {
                    class Mix {}

                    for (let mixin of mixins) {
                        copyProperties(Mix, mixin); // 拷贝实例属性
                        copyProperties(Mix.prototype, mixin.prototype); // 拷贝原型属性
                    }

                    return Mix;
                }

                function copyProperties(target, source) {
                    for (let key of Reflect.ownKeys(source)) {
                        if ( key !== "constructor"
                            & key !== "prototype"
                            && key !== "name"
                        ) {
                            let desc = Object.getOwnPropertyDescriptor(source, key);
                            Object.defineProperty(target, key, desc);
                        }
                    }
                }

                class DistributedEdit extends mix(Loggable, Serializable) {
                  // ...
                }