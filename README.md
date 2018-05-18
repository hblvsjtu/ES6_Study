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
## [二、类class](#2)
### [2.1 简介](#2.1)
### [2.2 let和const](#2.2) 
### [2.3 顶层对象](#2.3) 
### [2.4 数组的解构](#2.4)
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

        
------      
        

<h2 id='2'>二、类class</h2>
<h3 id='2.1'>2.1 继承 详细看<a href="https://www.cnblogs.com/humin/p/4556820.html">幻天芒的博客</a></h3>  

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

        
<h3 id='2.2'>2.2 class的使用</h3>  
        
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
> - 第一种方法：从命名上，私有方法或者属性名字前面加上下划线。但是，这种命名是不保险的，在类的外部，还是可以调用到这个方法。
> - 第二种方法：把私有方法移出模块，采用call,apply或者bind等方法进行调用
> - 引入一个新的前缀#表示私有属性和私有方法
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
#### 7) Symbol
> - ES5 的对象属性名都是字符串，这容易造成属性名的冲突，Symbol可以用来避免名字的冲突
> - symbol是基本类型，实现唯一标识
> - 通过调用symbol(name)创建symbol，Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分。
> - 我们创建一个字段，仅为知道对应symbol的人能访问，使用symbol很有用
> - symbol不会出现在for..in结果中
> - 使用symbol(name）创建的symbol，总是不同，即使name相同。如果希望相同名称的symbol相等，则使用全局注册
> - symbol.for(name)返回给定名称的全局symbol，多次调用返回相同symbol
> - Javascript有系统symbol，通过Symbol.*访问。我们能使用他们去修改一些内置行为。
        
                let s = Symbol();
                typeof s;
                21:52:41.244 "symbol"

                let s1 = Symbol('foo');
                let s2 = Symbol('bar');
                s1 // Symbol(foo)

                21:55:05.722 Symbol(foo)
                21:55:12.370 s2 // Symbol(bar)
                21:55:12.372 Symbol(bar)
                21:55:22.628 s1.toString() // "Symbol(foo)"
                21:55:22.630 "Symbol(foo)"
                21:55:25.162 s2.toString() // "Symbol(bar)"
                21:55:25.164 "Symbol(bar)"
> - Symbol函数的参数只是表示对当前 Symbol 值的描述，因此相同参数的Symbol函数的返回值是不相等的。
        
                // 没有参数的情况
                let s1 = Symbol();
                let s2 = Symbol();
                s1 === s2 // false
                // 有参数的情况
                let s1 = Symbol('foo');
                let s2 = Symbol('foo');
                s1 === s2 // false
> - 作为属性名的Symbol 大多需要用到方括号。保证不会出现同名的属性，防止某一个键被不小心改写或覆盖。因为点运算符后面总是字符串，所以不会读取mySymbol作为标识名所指代的那个值，导致a的属性名实际上是一个字符串，而不是一个 Symbol 值。所以不能使用.号运算符
        
                let mySymbol = Symbol();

                // 第一种写法
                let a = {};
                a[mySymbol] = 'Hello!';

                // 第二种写法
                let a = {
                  [mySymbol]: 'Hello!'
                };

                // 第三种写法
                let a = {};
                Object.defineProperty(a, mySymbol, { value: 'Hello!' });

                // 以上写法都得到同样结果
                a[mySymbol] // "Hello!"

                let s = Symbol();
                let obj = {
                  [s]: function (arg) {console.log(arg);}
                };
                obj[s](123);
                22:09:15.222 VM626:4 123
> - 魔术字符串 多次出现，与代码形成“强耦合”，不利于将来的修改和维护。常用的消除魔术字符串的方法，就是把它写成一个变量。
> - 
#### 8) Generator 方法 [MDN web docs](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/yield)
> - [rv] = yield [expression];
> - expression 定义通过迭代器协议从生成器函数返回的值。如果省略，则返回undefined。
> - rv 返回传递给生成器的next()方法的可选值，以恢复其执行。
> - yield关键字使生成器函数执行暂停，yield关键字后面的表达式的值返回给生成器的调用者。它可以被认为是一个基于生成器的版本的return关键字。
> - yield关键字实际返回一个IteratorResult对象，它有两个属性，value和done。value属性是对yield表达式求值的结果，而done是false，表示生成器函数尚未完全完成。
> - 一旦在 yield 表达式处暂停，除非外部调用生成器的 next() 方法，否则生成器的代码将不能继续执行。每次调用生成器的next()方法时，生成器都会恢复执行，直到达到以下某个值：
> - 某个方法之前加上星号（*），就表示该方法是一个 Generator 函数。
        
                function* countAppleSales () {
                    var saleList = [3, 7, 5];
                    for (var i = 0; i < saleList.length; i++) {
                        yield saleList[i];
                    }
                }
                21:19:07.675 undefined
                21:19:27.967 var appleStore = countAppleSales(); // Generator { }
                21:19:27.973 undefined
                21:19:54.048 console.log(appleStore.next()); // { value: 3, done: false }
                21:19:54.048 VM199:1 {value: 3, done: false}
                21:19:54.053 undefined
                21:20:43.808 console.log(appleStore.next()); // { value: 7, done: false }
                21:20:43.808 VM201:1 {value: 7, done: false}
                21:20:43.811 undefined
                21:20:50.062 console.log(appleStore.next()); // { value: 5, done: false }
                21:20:50.062 VM203:1 {value: 5, done: false}
                21:20:50.064 undefined
                21:21:12.515 console.log(appleStore.next()); // { value: undefined, done: true }
                21:21:12.514 VM205:1 {value: undefined, done: true}
                21:21:12.516 undefined
> - 