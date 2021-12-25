# ES的介绍

+ ES是ECMAScript的简称，是脚本语言的规范，而平时经常编写的JavaScript，是ECMAScript的一种实现，所以ES新特性其实指的是JavaScript的新特性。
+ ES版本有es5(2009)、es6（2015）、es7（2016）、es8（2017）、es9（2018）、es10（2019）、es11 （2020）每年都会有新版本

## let

+ 声明变量

  ```js
  let a;
  let b,c;//声明多个
  let d = 10;
  let e = 12, f = 13, g = 14;//声明多个并付初始值
  ```

+ 不能重复声明

  ```js
  // let star = '罗志祥';
  // let star = '小猪'; Uncaught SyntaxError: Identifier 'star' has already been declared
  ```

+ 块级作用域

  ```js
  //if else  while  for都是代码块级
  // {
  //   let girl = '周扬青'
  // }
  // console.log(girl);Uncaught ReferenceError: girl is not defined
  // if (true) {
  //   let girl = '周扬青';
  // }
  // console.log(girl);Uncaught ReferenceError: girl is not defined
  ```

+ 不存在变量提升

  ```js
  // console.log(song) //undefined var定义变量提升的表现 如果是let则会报错
  // var song = '恋爱达人'; 
  //console.log(song) //Uncaught ReferenceError: Cannot access 'song' before initialization
  //let song = '恋爱达人';
  ```

+ 不影响作用域链

  ```js
  {
      let school = '尚硅谷';
      function fn() {
          console.log(school);//尚硅谷  可以沿着作用域链向上走
      }
      fn();
  }
  ```


## let的经典案例

```js
let items = document.getElementsByClassName('item');
for (var i = 0; i < items.length; i++) {//将var换成let就正常了
    items[i].onclick = function() {
        // this.style.background = 'red';//正常  可以切换
        items[i].style.background = 'red';
    }
}
console.log(i);//3
/**
         * let定义的变量会有块级作用域  每一次循环后的点击{}块里的i都是唯一的
         * var定义的变量没有块级作用域  每一次都循环的点击{}块里的i都会被篡改 导致最后点击的时候 i其实已经变成3了
        */
```

## const

+ 声明常量

  ```js
  const SCHOOLE = '尚硅谷';
  ```

+  一定要赋初始值

  ```js
  const A;//报错
  ```

+ 常量的值不能修改

  ```js
  const SCHOOLE = '尚硅谷';
  SCHOOLE = 'shangguigu'；//报错
  ```

+ 块级作用域

+ 对于数组和对象的元素的修改，不算对常量的修改，因为引用类型 修改值后 存储的地址不会变

  ```js
  const TEAM= ['James', 'Kobe', 'Wade'];
  TEAM.push('Cury');//不会报错
  ```

## 解构赋值

+ 数组的解构

  ```js
  const F4 = ['小沈阳', '赵四', '刘能', '宋小宝'];
         //let [xiao, zhao, liu, song] = F4;
         //console.log(xiao, zhao, liu, song);//小沈阳 赵四 刘能 宋小宝
         let [xiao, liu, song] = F4;
         console.log(xiao, liu, song);//小沈阳 赵四 刘能
  ```

+ 对象的解构

  ```js
  const player = {
            name: 'James',
            age: '37',
            baskboar: function() {
                console.log('我可以打篮球');
            }
        }
        const {name} = player;
        console.log(name);//James
        const {baskboar} = player;
        baskboar();//我可以打篮球
  ```


## 模板字符串

​	模板字符串是用``来定义的

+ 模板字符串内容中可以直接出现换行符

  ```js
  let dom = `<ul>
                  <li>小沈阳</li>
                  <li>宋小宝</li>
                  <li>刘能</li>
                </ul>`
  ```

+ 变量拼接

  ```js
  let lovest = '小沈阳';
  let outStr = `${lovest}是我最喜欢的喜剧演员`;
  ```

## 简化对象的写法

```js
let name = '尚硅谷';
let change = function() {
    console.log('我们可以改变你!');
}
const school = {
    name,//简化后可以直接写一个变量 代表键和值都是这个变量
    change,
    // improve: function() {//正常写
    //   console.log('我们可以提高你的技能');
    // }
    improve() {//简化后
        console.log('我们可以提高你的技能');
    }
}
```

## 箭头函数

+ 箭头函数的简写

  + 省略小括号；当形参只有一个的时候

    ```js
    let add = n => {
            return n + n;
        }
        console.log(add(9));//18
    ```

  + 省略花括号；当代吗只有一条语句的时候 此时return也必须省略  语句的执行结果就是函数的返回值

    ```js
    let pow = n => {//正常写
           return n * n;
         }
         console.log(pow(9));//81
         let pow2 = n => n * n;//简写
         console.log(pow2(9));//81
    ```

+ 箭头函数中this

  ```js
  //箭头函数中实际是没有自己this的 它的this是静态的  是声明时所在的作用域下的this 不能更改
  window.name = '尚硅谷';
  let getName = function() {
      console.log(this.name);
  }
  let getName2 = () => {
      console.log(this.name);
  }
  getName();//尚硅谷
  getName2();//尚硅谷
  
  const school = {
      name: 'ATGUIGU'
  }
  //1.通过call修改this
  getName.call(school);//ATGUIGU
  getName2.call(school);//尚硅谷 箭头函数中的this并没有修改为school 还是指向声明时的作用域  window
  ```

+ 不能做为构造函数的实例化对象

  ```js
  // let Person = (name, age) => {
  //   this.name = name;
  //   this.age = age;
  // };
  // let me = new Person('拓', 30);
  // console.log(me);//Uncaught TypeError: Person is not a constructor  报错
  ```

+ 没有argument变量（argument是普通函数中保存实参的变量）

  ```js
  // let fn2 = function(a,b) {
  //   console.log(arguments);
  // }
  // fn2(2,'拓')//Arguments(2) [2, '拓', callee: ƒ, Symbol(Symbol.iterator): ƒ]
  
  // let fn2 = (a,b) => {
  //   console.log(arguments);
  // }
  // fn2(2, '拓');//Uncaught ReferenceError: arguments is not defined
  ```


## reset参数

+ ES6引入reset参数，用于获取参数的实参，用来替代ES5中的arguments

+ ES5获取函数的实参

  ```js
  //ES5 获取实参的方式
  function date() {
      console.log(arguments);//Arguments(3) ['白芷', '阿娇', '思慧', callee: ƒ, Symbol(Symbol.iterator): ƒ]
  }
  date('白芷','阿娇','思慧');
  ```

+ reset参数（如果参数是多个的时候 reset参数要放在最后一个）

  ```js
  //reset参数 如果是多个参数的时候 reset参数要放在最后一个
  function date1(...args) {//args是个变量 可以随意换
      console.log(args);//数组  [1, 2, 3, 4, 5]
  }
  date1(1,2,3,4,5);
  ```

## 扩展运算符

+ 数组的合并

  ```js
  //1.数组的合并
  const kuaizi = ['王太利', '肖央'];
  const fenghuang = ['曾毅', '玲花'];
  const zuixuanxiaopingguo = [...kuaizi, ...fenghuang];
  console.log(zuixuanxiaopingguo);//['王太利', '肖央', '曾毅', '玲花'];
  ```

+ 数组的克隆

  ```js
   //2.数组的克隆
  const sanzhihua = ['E', 'G', 'M'];
  const sankecao =[...sanzhihua];
  console.log(sankecao);//['E', 'G', 'M'];如果数组里边有引用数据则为浅拷贝
  ```

+ 将伪数组转为真正的数组

  ```js
  //3.将伪数组转为真正的数组
  const divs = document.querySelectorAll('div');//Nodlist 伪数组
  const divArr = [...divs];//转换为数组
  ```


## Symbol基本使用

​	ES6引入一种新的原始数据类型Symbol,表示独一无二的值。它是javascript的第七种数据类型，是一种类似于字符串的数据类型。

Symbol的特点

+ Symbol的值是唯一的，用来解决命名冲突的问题

+ Symbol的值不能与其他数据进行运算

+ Symbol定义的对象属性不能使用for...in...循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

  ```js
   //1.创建Symbol
          let s = Symbol();
          console.log(s, typeof s);//Symbol() 'symbol'
          let s2 = Symbol('尚硅谷');
          console.log(s2);//Symbol(尚硅谷)
          let s3 = Symbol('尚硅谷');
          s2 === s3;//false
  
          //Symbol.for创建
          let s4 = Symbol.for('尚硅谷');
          console.log(s4);//Symbol(尚硅谷)
  
          //不能与其他数据进行运算
          // let result = s + 100;
          // let result = s > 100;
          // let result = s + s;
  
          /**
              数据类型：undefined string symbol object null number boolean
              object中又function array Date ...   
          */
  
          //2.symbol创建对象属性
          //向对象中添加方法 up down
          /**
          如果要向game对象中添加up 和 down的方法，但是不知道这个对象中是否存在up和down的属性 如果存在的话会被复写，这时候就用到了symbol
          */
          let game = {
              up: '上',
              down: '下'
          }
          let methods = {
              up: Symbol(),
              down: Symbol()
          }
          game[methods.up] = function() {
              console.log('我可以向上')
          }
          game[methods.down] = function() {
              console.log('我可以向下');
          }
          console.log(game);//{up: '上', down: '下', Symbol(): ƒ, Symbol(): ƒ}
          game[methods.up]();//我可以向上
          //另一种方法
          let say = Symbol();
          let youxi = {
              name: "狼人杀",
              [say]: function() {
                  console.log("我可以发言")
              },
              [Symbol('zibao')]: function() {
                  console.log('我可以自爆');
              }
          }
          console.log(youxi);//{name: '狼人杀', Symbol(say): ƒ, Symbol(zibao): ƒ}
          youxi[say]();//我可以发言
  ```


## 迭代器(iterator)接口for...of

​	迭代器(iterator)是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署iterator接口，就可以完成遍历操作。

+ ES6创造了一种新的遍历命令for...of循环，iterator接口主要提供for...of消费
+ 原生具备iterator接口的数据(可用for...of遍历)
  + Array
  + Arguments
  + Set
  + Map
  + String
  + TypedArray
  + NodeList

+ 工作原理
  + 创建一个指针对象，指向当前数据结构的起始位置
  + 第一次调用对象的next方法，指针自动指向数据结构的第一个成员
  + 接下来不断调用next方法，指针一直往后移，直到移向最后一个成员
  + 每次调用next方法返回一个包含value和done属性的对象

```js
<script>
        /**
         * 迭代器(iterator)是一种接口，为各种不同的数据结构提供统一的访问机制。
         * 任何数据结构只要部署iteration接口，就可以完成遍历操作
         * ES6创造了一种新的遍历命令for...of循环，iterator接口主要提供for...of消费
         * 原生具备iterator接口的数据(可用for...of遍历)
         * Array
         * Araguments
         * Set
         * Map
         * String
         * TypedArray
         * NodeList
         * 
        */
        //声明一个数组
        const xiyou = ['唐僧', '孙悟空', '猪八戒', '沙僧'];
        //使用 for...of遍历数组
        for(let v of xiyou){
            console.log(v);//唐僧 孙悟空  猪八戒  沙僧
        }
        console.log(xiyou);//prototype中有Symbol(Symbol.iterator): ƒ values()
        console.log(xiyou[Symbol.iterator]());//Prototype上有一个next方法
        /**
         * 原理：
         *  由Symbol(Symbol.iterator)这个函数来创建一个对象，并指向当前数据解构的起始位置
         *  对象身上有一个next方法  第一次调用next方法，指针自动指向属性据结构的第一个成员
         *  接下来不断调用next方法，指针一直往后移动，直到指向最后一个成员
         *  每调用next方法回返回一个包含value和done属性对象
         * 
        */
    </script>
```

## 自定义iterator遍历对象

```js
 <script>
        /**
         * 需求：需要遍历banji这个对象并且返回stus中的元素
        */
       //声明一个对象
       const banji = {
           name: "终极一班",
           stus: [
               "xiaoming",
               "xiaoning",
               "xiaotian",
               "knigint"
           ],
           [Symbol.iterator]() {
               let index = 0;
               let _this = this;
             return {
                 next: function() {
                    if (index < _this.stus.length) {
                        const result = {value: _this.stus[index], done: false}
                        index++;
                        return result;
                    } else {
                        return {value: undefined, done: true};
                    }
                 }
             }
           }
       }
       //遍历这个对象
       for (let v of banji) {
            console.log(v);//"xiaoming","xiaoning", "xiaotian","knigint"
       }
    </script>
```

## 生成器

+ 生成器的函数与声明

  ```js
  //生成器是ES6提供的一种异步编程的方案，语法行为与传统函数完全不通
  //生成器其实就是一个特殊的函数
  //原生的异步处理方法是回调 会造成回调地狱  生成器可以解决这种缺陷
  
  //生成器的语法
  function * gen() {//生成器里可以写yield 相当于把代码分隔为一块一块
      console.log(123);
      yield '一只没有耳朵';
      yield '一只没有尾巴';
      yield '真奇怪';
  }
  let interator = gen();
  console.log(interator);//原型上有next()方法  是一个interator接口 可以直接调next()执行
  interator.next();//123
  console.log(interator.next());//{value: '一只没有尾巴', done: false}  调用next()对象返回的值
  for(let v of gen()) {//因为gen()生成器上有interator迭代器的next()方法  所以可以用for...of
      console.log(v);//'一只没有耳朵' '一只没有尾巴' '真奇怪'
  }
  ```

+ 生成器的函数参数

  ```js
  function * gen(arg) {
      console.log(arg);//AAA
      let one = yield 111;
      console.log(one);//BBB 第二个next的实参就是第一个yield的返回值
      let two = yield 222;
      console.log(two);//CCC 第三个个next的实参就是第二个yield的返回值
      let three = yield 333;
      console.log(three);//DDD 第四个个next的实参就是第三个yield的返回值
  }
  //执行获取迭代器对象
  let interator = gen('AAA');
  console.log(interator.next());//{value: 111, done: false}
  console.log(interator.next('BBB'));//{value: 222, done: false}
  console.log(interator.next('CCC'));//{value: 333, done: false}
  console.log(interator.next('DDD'));//{value: undefined, done: true}
  ```

+ 生成器的实例1

  ```js
  //异步编程 js是单线程的
  //1s后控制台输出111 2s后输出222 3s后输出333
  
  //回调地狱
  // setTimeout(() => {
  //     console.log(111);
  //     setTimeout(() => {
  //         console.log(222);
  //         setTimeout(() => {
  //             console.log(333);
  //         }, 3000)
  //     }, 2000)
  // }, 1000)
  
  //换成生成器函数异步编程
  function one() {
      setTimeout(() => {
          console.log(111);
          interator.next();
      },1000)
  }
  function two() {
      setTimeout(() => {
          console.log(222);
          interator.next();
      },2000)
  }
  function three() {
      setTimeout(() => {
          console.log(333);
          interator.next();
      },3000)
  }
  
  function * gen() {
      yield one();
      yield two();
      yield three();
  }
  let interator = gen();
  interator.next();
  ```

+ 生成器的实例2

  ```js
  //模拟获取  用户数据  订单数据  商品数据 后面的以来前面的数据
  function getUsers() {
      setTimeout(() => {
          let data = '用户数据';
          //调用next方法 并且将数据传入
          interator.next(data);//第二次调用next data参数将作为第一个yield的返回值
      },1000)
  }
  function getOrders() {
      setTimeout(() => {
          let data = '订单数据';
          interator.next(data);
      },1000)
  }
  function getGoods() {
      setTimeout(() => {
          let data = '商品数据';
          interator.next(data);
      },1000)
  }
  
  
  function * gen() {
      let users = yield getUsers();
      console.log(users);
      let orders = yield getOrders();
      console.log(orders);
      let goods = yield getGoods();
      console.log(goods);
  }
  let interator = gen();
  interator.next();
  ```


## Promise

+ Promise的基本语法

  ```js
   //Promise是ES6引入的异步编程的新解决方案。语法Promise是一个构造函数，用来封装异步操作并可以获取其成功或失败的结果
  let p = new Promise(function(reslove, reject) {
      setTimeout(() => {
          let data = '数据库请求回的数据';
          reslove(data);
          // let err = '数据库请求失败';
          // reject(err);
      },1000)
  })
  p.then(function(value){
      console.log(value);//1s后控制台输出 '数据库请求回的数据'
  },function(err){
      // console.log(err);//1s后控制台输出 '数据库请求失败'
  })
  ```

+ Promise封装AJAX请求

  ```js
  <script>
      //接口地址：https://api.apiopen.top/getJoke
      const p = new Promise((resolve, reject) => {
          //1.创建对象
          const xhr = new XMLHttpRequest();
          //2.初始化
          xhr.open("GET", 'https://api.apiopen.top/getJoke');
          //3.发送
          xhr.send();
          //4.绑定事件 处理响应结果
          xhr.onreadystatechange = function () {
              //判断
              if (xhr.readyState === 4) {
                  //判断响应码状态 200-299
                  if (xhr.status >= 200 && xhr.status < 300) {
                      // console.log(xhr.response);
                      resolve(xhr.response);
                  } else {
                      //如果失败
                      // console.log(xhr.status);
                      reject(xhr.status);
                  }
              }
          }
      })
      p.then((value)=>{
          console.log(value);
      },(err)=>{
          console.log(err);
      })
  </script>
  ```

## Set集合

+ Set集合

  ```js
  //ES6提供了新的数据结构Set（集合）。它类似于数组，但成员的值都是唯一的，集合实现了interator接口，所以可以使用【扩展运算符】和【for...of...】进行遍历
  
  //声明一个 set
  let s = new Set();
  let s2 = new Set(['小沈阳', '宋小宝', '王小利', '小沈阳']);
  console.log(s2);//Set(3) {'小沈阳', '宋小宝', '王小利'} 只有一个小沈阳  会自动去重
  
  //元素个数
  console.log(s2.size);//3
  //添加新元素
  s2.add('赵四');
  console.log(s2);//Set(4) {'小沈阳', '宋小宝', '王小利', '赵四'}
  //删除元素
  s2.delete('赵四');
  console.log(s2);//Set(3) {'小沈阳', '宋小宝', '王小利'}
  //检测
  console.log(s2.has('小沈阳'));//true
  //清空
  s2.clear();
  console.log(s2);//Set(0) {size: 0}
  ```

+ Set集合实例

  ```js
  let arr1 = [2, 3, 4, 5, 3, 4, 6, 7];
  let arr2 = [2, 3, 5, 8, 5, 3];
  //去重
  const result = [...new Set(arr1)];
  console.log(result);//[2, 3, 4, 5, 6, 7]
  //交集
  const result1 = [...new Set(arr1)].filter(item => new Set(arr2).has(item));
  console.log(result1);//[2, 3, 5]
  //并集
  const result2 = [...new Set([...arr1, ...arr2])];
  console.log(result2);//[2, 3, 4, 5, 6, 7, 8]
  //差集
  const result3 = [...new Set(arr1)].filter(item => !new Set(arr2).has(item));
  console.log(result3);//[4, 6, 7]
  ```

## Map集合介绍和API

```js
 /**
         * ES6提供了Map数据结构。它类似与对象，也是键值对的集合。但是"键"的范围不限于字符串，各种类型的值（包括对象）都可以当作键。Map也实现了iterator接口，所以可以使用【扩展运算符】和【for...of...】进行遍历。
         * 
        */

//声明Map
let m = new Map();

//添加元素
m.set('name', '小沈阳');
m.set('change', function() {
    console.log('我们可以改变你们！！！');
});
m.set('key',['北京', '上海', '深圳']);
console.log(m);//Map(3) {'name' => '小沈阳', 'change' => ƒ, 'key' => Array(3)}

//size
console.log(m.size);//3
//删除
m.delete('name');
console.log(m);//Map(2) {'change' => ƒ, 'key' => Array(3)}
//获取
console.log(m.get('key'));//['北京', '上海', '深圳']
//清空
m.clear();
console.log(m);//Map(0) {size: 0}

for(v of m) {
    console.log(v);//['change', ƒ]  ['key', Array(3)]
}
```

## class类

+ class类的介绍

  ```js
  /**
           * ES6提供了更接近传统语言的写法，引入了class(类)这个概念，作为对象的模板。通过class关键字，可以定义类。基本上ES6的class可以看作只是一个语法糖，它的绝大部分功能，ES5都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。
           * 1) class声明类
           * 2) constructor定义构造函数初始化
           * 3) extends继承父类
           * 4) super调用父级构造方法
           * 5) static 定义静态方法和属性
           * 6) 父类方法可以重写
          */
         
  
  //ES5定义类 一手机类为例
  function Phone1(brand, price) {
      this.brand = brand;
      this.price = price;
  }
  //添加方法
  Phone1.prototype.call = function() {
      console.log('我可以打电话!!!');
  }
  //实例化对象
  const Huawei = new Phone1('华为', 5999);
  Huawei.call();//我可以打电话!!!
  console.log(Huawei);//Phone1 {brand: '华为', price: 5999} 原型上有call方法
  
  //ES6定义类
  class Phone2 {
      //构造函数 名字固定不能修改 实例化的时候调用
      constructor(brand, price) {
          this.brand = brand;
          this.price = price;
      }
      //定义方法
      call() {
          console.log('我可以打电话!!!');
      }
  }
  let xiaomi11 = new Phone2('小米11', '3999');
  console.log(xiaomi11);//Phone2 {brand: '小米11', price: '3999'} 原型上有call方法
  ```

+ 类的静态方法 static

  ```js
   //ES5中
  function Phone1() {
  
  }
  Phone1.name = 'onePlus';
  Phone1.prototype.size = '6.6';
  Phone1.change = function() {
      console.log('我可以改变世间!!!');
  }
  const onplus = new Phone1();
  console.log(onplus);//Phone1 {}
  console.log(onplus.name);//undefined
  console.log(onplus.size);//6.6  原型上的方法才可以是实例化出来的对象的方法
  
  //ES6
  class Phone2 {
      //静态属性
      static name = '手机';
      static change() {
          console.log('我可以改变世间!!!');
      }
  }
  let nokia = new Phone2();
  console.log(nokia.name);//undefined
  console.log(Phone2.name);//手机
  ```

+ ES5类的继承

  ```js
  //手机类
  function Phone(brand, price) {
      this.brand = brand;
      this.price = price;
  }
  Phone.prototype.call = function() {
      console.log('我可以打电话!!!');
  }
  
  //智能手机类
  function SmartPhone(brand, price, color, size) {
      Phone.call(this, brand, price);
      this.color = color;
      this.size = size;
  }
  //设置子级的构造函数原理
  SmartPhone.prototype = new Phone;
  SmartPhone.prototype.constructor = SmartPhone;//指向SmartPhone的原型
  
  //声明子类的函数
  SmartPhone.prototype.photo = function() {
      console.log('我可以拍照');
  }
  const chuizi = new SmartPhone('锤子', 2499, '黑色', '5.5inch');
  console.log(chuizi);//SmartPhone {brand: '锤子', price: 2499, color: '黑色', size: '5.5inch'}
  ```

+ ES6类的继承extends

  ```js
   //父类
  class Phone {
      constructor(brand, price) {
          this.brand = brand;
          this.price = price;
      }
      call() {
          console.log('我可以打电话!!!');
      }
  }
  //子类
  class SmartPhone extends Phone {
      constructor(brand, price, color, size) {
          super(brand, price);//初始化父类中的constructor 相当于ES5中的Phone.call(this, brand, price);
          this.color = color;
          this.size = size;
      }
      photo() {
          console.log('我可以拍照!!!');
      }
  }
  const xiaomi = new SmartPhone('小米', '3999', '黑色', '6.6inch');
  console.log(xiaomi);//{brand: '小米', price: '3999', color: '黑色', size: '6.6inch'}
  xiaomi.call();//我可以打电话!!!
  xiaomi.photo();//我可以拍照!!!
  ```

+ 子类对父类方法的重写

  ```js
  //目前如果在子类中重写了父类的方法发 调用的一定是子类的  没办法调用父类中的同名方法
  //父类
  class Phone {
      constructor(brand, price) {
          this.brand = brand;
          this.price = price;
      }
      call() {
          console.log('我可以打电话!!!');
      }
  }
  //子类
  class SmartPhone extends Phone {
      constructor(brand, price, color, size) {
          super(brand, price);//初始化父类中的constructor 相当于ES5中的Phone.call(this, brand, price);
          this.color = color;
          this.size = size;
      }
      photo() {
          console.log('我可以拍照!!!');
      }
      //重写父类方法
      call() {
          console.log('我可以视频通话!!!');
      }
  }
  const xiaomi = new SmartPhone('小米', '3999', '黑色', '6.6inch');
  console.log(xiaomi);//{brand: '小米', price: '3999', color: '黑色', size: '6.6inch'}
  xiaomi.call();//我可以视频通话!!!
  xiaomi.photo();//我可以拍照!!!
  ```

+ class类中的getter和setter

  ```js
  //get和set
  class Phone {
      get price() {//当作一个属性来用
          console.log('价格属性被读取了!!!');//价格属性被读取了!!!
          return 'i love you'
      }
      set price(value) {
          console.log('价格被属性修改了');
      }
  }
  //实例化类
  const s = new Phone();
  console.log(s.price);//i love you price中的返回值作为s.price的值
  s.price = 'free';//价格被属性修改了  一定是赋值 不能s.price('free') 因为不是函数是属性
  ```

## ES6的数值扩展

```js
//1、Number.EPSILON是JavaScript表示的最小精度
//EPSILON属性的值接近于2.2204460492503130808472633361816E-16  二的负25次方
console.log(0.1+0.2 == 0.3);//false  因为计算机转换为二进制后计算 最后计算的值是0.30000000000000004

//2.二进制和八进制
let er = 0b1010;
let ba = 0o777;
let shi = 100;
let shiliu = 0xff;

//3.Number.isFinite 检测一个数值是否为有限数
console.log(Number.isFinite(100));//true
console.log(Number.isFinite('0'));false

//4.Number.isNaN 检测一个数值是否为NaN
console.log(Number.isNaN(123));//false
console.log(Number.isNaN(NaN));//true

//5.Number.parseInt Number.parseFloat字符串转整数(数字开头)
console.log(Number.parseInt('528shenqi'));//528
console.log(Number.parseFloat('3.1415926商会大厦'));//3.1415926

//6.Number.isInteger判断一个数是否为整数
console.log(Number.isInteger(5));//true
console.log(Number.isInteger(2.5));//false

//7.Math.trunc 将数字的小数部分抹掉
console.log(Math.trunc(3.2));//3

//8.Math.sign 判断一个数到底为正数 零 负数
console.log(Math.sign(100));//1
console.log(Math.sign(0));//0
console.log(Math.sign(-20));//-1
```

## ES6对象的扩展方法

```js
//1. Object.is 判断两个值是否完全相等
/**
         * Object.is类似与===全等  区别点就是NaN===NaN false; Object.is(NaN, NaN); true;
         * 我们知道NaN跟任意数比都是不相等的
        */
console.log(Object.is(120, 121));//false
console.log(Object.is(120, 120));//true
console.log(Object.is(NaN, NaN));//true

//2.Object.assign 对象的合并 如果对象的属性是基本类型则可理解为深拷贝  对象属性是引用类型则为浅拷贝 拷贝的是引用地址
/**
         * Object.assgin与赋值的区别就是 它对属性为基本类型的对象有深拷贝的功能 合并后改变新对象属性的值不会影响旧对象  但是赋值会改变
        */
const obj1 = {
    name: 'kobe',
    height: '1.9m'
};
const obj2 = {
    name: 'James',
    sex: 'man'
}
console.log(Object.assign(obj1, obj2));//{name: 'James', height: '1.9m', sex: 'man'}
```

## 模块化

```js
<!-- 
ES6之前推出的模块化规范产品
1)CommonJS => NodeJS Browserify
2)AMD => requireJS
3)CMD => seaJS
ES6的模块化有export和import
    -->
    <script type="module">
        /**
     * import * as xxx from xxx
     *  是导入所有as后面的是给起一个别名
     *  如果用的是默认到出export default 导入后需加default
     *   暴露的方法有三种  分别暴露  统一暴露  默认暴露
     * 
     *    导入的方法有  通用导入  解构赋值  简单形式(只只针对默认暴露 export default{})
    */
                /**
                 * 通用导入
                */
        //引入m1.js模块内容
        import * as m1 from './src/js/m1.js';
        m1.teach();//我可以逗大家笑
        //引入m2.js模块内容
        import * as m2 from './src/js/m2.js';
        m2.play();//我可以演电视剧
        //引入m3.js模块内容
        import * as m3 from './src/js/m3.js';
        m3.default.change();//我们可以改变你

        		/**
                 * 解构赋值
                */
        import {name, teach} from './src/js/m1.js';
        import {name as abcd, play} from './src/js/m2.js';//因为name会重名所以起个别名abcd
        import {default as m33} from './src/js/m3.js';
        console.log(name,abcd,m33);//小沈阳 宋小宝 {school: '清华', change: ƒ}
        teach();//我可以逗大家笑
        play();//我可以演电视剧
        m33.change();//我们可以改变你

        		/**
                * 简便形式  针对默认暴露 export default
               */
        import m333 from './src/js/m3.js';
        console.log(m333);//{school: '清华', change: ƒ}
        m333.change();//我们可以改变你

    </script>
```

## ES7新增特性

ES7新增两个特性

+ Array.includes判断数组中是否存在元素

+ **幂运算  相当于Math.pow();

  ```js
   		/**
           * ES7新增两个特性
           *  1）Array.includes判断数组中是否存在元素
           *  2）**幂运算  相当于Math.pow();
          */
          const mingzhu = ['西游记', '三国', '红楼梦', '水浒'];
          //判断
          console.log(mingzhu.includes('西游记'));//true
          console.log(mingzhu.includes('金瓶梅'));//false
  
          console.log(2**10);//1024
          console.log(Math.pow(2, 10));//1024
  ```

## ES8的async和await

 /**

​     \* async和await两种语法结合可以让异步代码像同步代码一样

​     \* async函数的返回值为promise对象

​     \* promise对象的结果由async函数执行的返回值决定

​     \* 

​     \* await必须写在async函数中

​     \* await右侧的表达式一般为promise对象

​     \* await返回的是promise成功值

​     \* await的promise失败了，就会抛出异常，需要通过try...catch捕获处理

​    */

```js
//async函数
async function fn() {
    return '这是个promise对象';//当返回的不是一个promise对象的时候 默认为promise的成功值
    //    return new Promise((resolve, reject) => {
    //        resolve('成功的数据');
    //    })
}
const result = fn();//async返回的是promise对象
result.then(value => {
    console.log(value);//这是个promise对象
}, err => {
    console.log(err);
})

//发送ajax请求
function sendAJAX(url) {//封装ajax请求
    return new Promise((resolve, reject) => {
        //1.创建对象
        const x = new XMLHttpRequest();
        //2.初始化
        x.open('GET', url);
        //3.发送
        x.send();
        //4.事件绑定
        x.onreadystatechange= function() {
            if (x.readyState === 4) {
                if (x.status >= 200 && x.status < 300) {
                    resolve(x.response);
                } else {
                    reject(x.status);
                }
            }
        }
    })
}
//promise then方法测试
sendAJAX("https://api.apiopen.top/getJoke").then(value => {
    console.log(value);
}, err => {});

//async和await
async function main() {
    let result= await sendAJAX("https://api.apiopen.top/getJoke");
    let tianqi = await sendAJAX("https://api.apiopen.top/getSingleJoke");
    console.log(result,tianqi);
}
main();
```

## ES8对象方法的扩展

```js
//声明对象
const school = {
    name: '清华大学',
    cities: ['北京', '上海', '深圳'],
    xueke: ['前端', 'Java', '大数据', '运维']
} 
//获取对象所有的键
console.log(Object.keys(school));//['name', 'cities', 'xueke']

//获取对象所有的值
console.log(Object.values(school));//['清华大学', Array(3), Array(4)]
```

## ES9中的扩展运算符和rest参数针对对象

```js
//ES9中的rest参数  针对对象
function connect({...args}) {//一定要有{}  如果没有{}则成ES6中的rest参数了 打印出来是[{host: '127.0.0.1', port: 3006, username: 'root', password: 'root', type: 'master'}]
    console.log(args);//{host: '127.0.0.1', port: 3006, username: 'root', password: 'root', type: 'master'}
}
connect({
    host: '127.0.0.1',
    port: 3006,
    username: 'root',
    password: 'root',
    type: 'master'
})

//ES9中的扩展对象
const one = {
    name: '清华大学'
}
const two = {
    cities: ['北京', '上海', '深圳']
}
console.log({...one, ...two});//{name: '清华大学', cities: Array(3)}
```

## ES10的字符串扩展方法trimStart和trimEnd

```js
const str = '  i   love   you   ';
console.log(str.trim());//i   love   you
console.log(str.trimStart());//i   love   you   
console.log(str.trimEnd());//  i   love   you
```

## ES10数组扩展flat

```js
//flat 可以将多维数组将阶
const arr = [1, 2, 3, [4, 5, 6]];
console.log(arr.flat());//[1, 2, 3, 4, 5, 6]
const arr1 = [1, 2, [3, 4, [5, 6]]];
console.log(arr1.flat(2));//[1, 2, 3, 4, 5, 6]
```

## ES11链可选运算符?.

```js
//?.
function main(config) {
    //正常判断
    const dbHost = config && config.db && config.db.host;
    console.log(dbHost);//192.168.1.100
    //?.
    const db_host = config?.db?.host;
    console.log(db_host);//192.168.1.100
}
main({
    db:{
        host: '192.168.1.100',
        username: 'root'
    },
    cache: {
        host: '192.168.1.200',
        username: 'admin'
    }
})
```

## globalThis

```js
console.log(globalThis);//Window {window: Window, self: Window, document: document, name: '', location: Location, …}
/**
         * 可以忽略环境  只用用globalThis就肯定是指向全局对象的
        */
```

