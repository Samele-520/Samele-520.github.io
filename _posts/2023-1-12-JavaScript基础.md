---
title: JavaScript基础
tags: JavaScript
---

* 所有通过`var`定义的全局变量和函数都会成为**window**对象的属性和方法，使用`let`和`const`的顶级声明不会定义在全局上下文中，但是作用域链继续上的效果是一样的。

* 双等
  * 可以不考虑数据类型只看内容是否相等
  * 比较过程：不进行数据类型的比较
  * 两个值类型相同，再进行三个等号(===)的比较
  * 如果两个值类型不同，则进行数据转换：需根据以下规则进行类型转换在比较：
    * 如果一个是null，一个是undefined，那么相等
    * 如果一个是字符串，一个是数值，把字符串转换成数值之后再进行比较
* 三等
  * 对数据类型都有严格要求，若不是同一数据类型则返回false
  * 先进行类型判断，如果类型不同，就一定不相等
  * 类型相等的情况下再进行以下判断：
    * 如果两个都是数值，并且是同一个值，那么相等；如果其中至少一个是NaN，那么一定不相等。（带双引号的NaN相等，表字符串）
    * 如果两个都是字符串，每个位置的字符都一样，那么相等，否则不相等。
    * 如果两个值都是true，或是false，那么相等
    * 如果两个值都引用同一个对象或是函数，那么相等，否则不相等
    * 如果两个值“都是”null，或是undefined，那么相等

* `?.`：可链式操作，先判断前边的属性有没有，若通过，则继续向后判断
* `let`: 定义变量
  1. 变量不能重复定义
  2. 块级作用域
  3. 不存在变量提升
  4. 不影响作用域链

    > 示例：对for使用let

* `const`:定义常量
  1. 一定要赋初始值
  2. 潜(最好)：一般常量使用大写
  3. 常量的值不能修改
  4. 块级作用域
  5. 对于数组和对象的元素修改，不算对常量的修改 -- 不报错 -- 首地址不变

* 由于逻辑语句if和循环语句for无法创建作用域，因此变量可以相互覆盖

* `变量结构赋值`：对应变量，对应赋值
  1. 解构出的数据，受其变量声明符影响（可不可修改）
  2. 通过`let`声明的解构值，可对其进行值修改，但是，不会影响解构前的对象或数组 => 拷贝值

      ```js
      const obj = {
        aaa: '111'
      }
      // 块一  ==> let解构
      let { aaa } = obj
      aaa = '222' // => obj => { aaa: '111' } , aaa => '222'
      // 块二  ==> const解构
      const { aaa } = obj
      aaa = '222' // 报错 -  Assignment to constant variable. => 不可修改
      ```

  3. 数组的结果赋值

      ```js
      const F4=["以","而","伞","斯"]
      let [xiao, liu, zao, song] = F4  // 一般对对象数组使用  ---  对位赋值
      // 不完全解构赋值
      let [xiao, ...quan] = F4
      ```

  4. 对象的结构赋值

      ```js
      const zao = {
        name: "赵",
        age: "20",
        xiao: function () {
          输出内容
        }
      }
      let { name1, age1, xiao1 } = zao
      // 如此，函数可以直接用xiao1()来调用
      ```

* ``：模板字符串
  1. 声明字符串
  2. 内容中可以直接出现换行符
  3. 变量拼接：

  ```js
  let abn="二货"
  let out = `${abn}是傻叉` // 二货是傻叉
  ```

* `简写对象`：属性和值一样，可以简写为一个属性

  ```js
  const name = '张某人'
  // 原
  const obj = {
    name: name
  }
  // 简写
  const obj = {
    name
  }
  ```

* `=>`：箭头函数
  1. this是静态的，this始终指向函数声明时所在的作用域下的this的值
  2. 适用于与this无关的回调、定时器、数组的方法回调
  3. 不适合与this有关的回调。事件回调，对象的方法。
  4. 不能作为构造函数实例化对象
  5. 不能使用arguments变量
  6. 箭头函数简写
     * 当只有一个参数时，可以省略小括号
     * 当代码体只有一条语句时，可以省略花括号，当花括号省略后return也**必须**省略，执行结果就是返回值
  
  ```js
  // 形式
  const fun = () => {}
  // 简化
  const fun = val => val
  ```

* `函数参数`：形参初始值、参数收集
  1. 具有默认值的参数，一般建议位置靠后，不放后边也可以 - 放在前边，传参不齐时，最后一共没参数
  2. rest参数(参数收集)用于获取函数的实参，用来代替arguments
      * arguments返回的是一个对象，而rest返回的是一个数组
      * rest参数必须放在参数最后---它会吸收剩余全部实参，当放中间时，放它后边的形参就没有值了
  
  ```js
  // 参数初始化
  const fun1 = (obj, val = 10, text = '111') => {}
  // 参数搜集
  const fun2 = (val, ...temp) => {}
  ```

* `...`：扩展运算符
  1. 能将数组转换为逗号分隔的参数序列
  2. 与rest很相似，但是rest放在形参位置，而它放在实参位置
  3. 数组合并
  4. 数组克隆(若有有引用类型的话，便是浅拷贝)
  5. 将伪数组(以数值字符串为属性的对象)转为真正的数组
  
  ```js
  const arr = ['1', '2', '3']
  ...arr // '1', '2', '3'
  ```

* **包装类**
  1. `Number`、`String`、`Boolean`三种类型有包装类
  2. 通过调用`.valueOf()`来销毁实例
  3. 包装过程及原始值调用方法过程

      ```js
      // 过程原理
      const temp = new String(xxx) // 1、创建包装对象
      temp.xxxx  // 2、代替调用指定方法
      temp.valueOf() // 3、还原原始值
      // 示例原理
      const str = temp
      str.length = 2 // new String(str).length  ==> 调用后，即销毁对象 或 匿名对象
      console.log(str.length) // 重新创建一个对象来调用.length方法，与赋值的那个不是一个对象
      ```

* `Symbol`：数据类型
  1. 原始数据类型，表示独一无二
  2. JavaScript第七种数据类型
  3. 一种类似于字符串的数据类型
  4. 可以显式转为字符串 String(Symbol值) --> Symbol值.toString
  5. 可以转为布尔值 Boolean(Symbol值)：默认为true --> 但是不能转为数值
  6. 值唯一，用来解决命名冲突问题
  7. 不能与其他数据进行运算
  8. 定义的对象属性不能用for…in来循环遍历，但是可以使用Reflect.ownKeys来获取对象的键名
  9. 通过`Symbol('内容')`来创建此类型的数据不能通过`new Symbol('内容')`来创建，可以接受字符串参数，括号里边的值为注释,用来表示对当前Symbol值的描述，即使内容相同，值也不同
  10. 通过`Symobl.for('内容')`来创建全局字符，括号里边的内容相同，则值相同
  11. 常用在给对象添加属性和方法

* `Promise`：
  1. 是个对象，需要实例化
  2. 函数内是一个异步操作
  3. 实例化时，接受一个含有俩参数的函数`resolve(成功)`，`reject(失败)`
  4. 当执行`resolve`时，返回成功状态，当执行`reject`时，返回失败状态
  5. 通过调用实例化后的`.then()`方法，获取两个参数分别时value和reason的函数，当成功时执行第一个，失败执行第二个,两个状态，只会执行一个。当执行一个后，另一个就不会执行了，且在最后执行
  6. `.then()`方法返回的值是一个Promise对象，对象状态由回调函数的执行结果决定
  7. 如果回调函数返回的结果时非Promise类型的属性，状态为成功，返回值为对象的成功值
  8. 若返回的是一个Promise对象，则内部的Promise的状态决定then返回的状态，返回的成功值，就是外边的成功值
  9. 抛出错误，状态为失败，值为抛出值
  10. 可链式调用，避免回调地狱

  ```js
  // 接收一个Promise数组，用于批量Promise处理，返回一个Promise对象，状态始终为成功，即使里边有错误的，也输出其错误状态，并输出其错误值。
  Promise.allSttled()
  // 接受一个Promise数组
  // 当有一个在期约待定时，则整体都在待定状态
  // 当有一个出现失败时，便会输出失败状态，并输出错误值
  Promise.all()
  // 返回成功的期约
  Promise.resolve()
  // 返回失败的期约
  Promise.reject()
  ```

* `async AND await`
  * `async`：返回值为Promise对象
    1. 返回的结果不是一个Promise对象，则返回的都是成功的Promise对象，值为return的值
    2. 抛出错误，返回的结果是一个失败的Promise对象

  * `await`
      1. await必须写在async函数中，而async中可以没有await
      2. await后边跟的表达式一般为Promise对象
      3. await返回的使用Promise成功的值
      4. await的Promise失败了，就会抛出异常，需要通过`try...catch`捕获处理，捕获的结果就是错误的值

* `迭代器（Iterator）`
    1. 是一种接口，为各种不同的数据结构提供统一的访问机制
    2. 新的遍历命令for…of循环，Iterator接口主要使用for…of循环
    3. for…of打印键名，for…in打印键名
    4. 原生具备iterator接口的数据有( Array、Arguments、Ser、Map、String、TypedArray、NodeList )--可以使用for…of
    5. 工作原理：
        1. 创建一个指针对象，指向当前数据结果的起始位置
        2. 第一次调用对象的next方法，指针自动指向数据结果的第一个成员
        3. 接下来不断调用next方法，指针一直往后移动，直到最后一个成员
        4. 每调用next方法，返回一个包含value和done属性的对象

    ```js
    [Symbol.iterator]() {
      // 索引变量
      let index = 0;
      return {
        next: () => {
          if (index < this.stus.length) {
              const result = {
                value: this.stus[index],
                done: false
              }
              index++
              return result
          } else return {
              value: undefined,
              done: true
            }
        }
      }
    }
    ```

* `生成器`：
  1. 特殊的函数
  2. 声明函数时，要在function和变量名间加一个"*",位置不定
  3. 纯回调函数
  4. 使用"yield"做函数代码的分隔符
  5. 直接调用，不执行，必须要调用函数的next( )属性才能执行
  6. 此next( )传入的实参，将做上一个yield的返回结果
  7. 初始调用（第一次调用）在实例化后，之后每次调用传入数据返回自己

* `Set`：

* `Map`：类似于对象，是键值对的集合
  1. 键值不限于字符串，可以是各种类型的值
  2. 升级版的对象

* `JSON`
  * `JSON.stringify()`： 方法用于将 JavaScript 值转换为字符串
  * `JSON.parse()`： 将字符还原

* `报错处理`

  ```js
    try {
    } catch() {
      throw new Error(`Something failed`);
    } finally {
    }
  ```
  