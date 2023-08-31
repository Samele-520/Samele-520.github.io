---
title: TypeScript
tags: TypeScript
---

* 准则
  * 如果变量声明和赋值是同时进行，那么该变量便被标定为初次赋值的类型
  * 先声明，后赋值，可以指定变量类型（不指定则为`any`）。声明与赋值同时进行，不需要指定类型
  * 类型的主要使用场景是函数，由于函数的传入不定，所以返回值很容易不是想要的数据类型。还可以指定传入的数据类型，确保函数的正确执行。
  * 非必要情况下，不使用`any`来定义变量，`any`类型的变量可以赋值给任意其他类型定义的变量，即使`any`变量对应的值与其所匹配的变量对应的类型不一，也可以赋值

    ```ts
    let str = 'hello'
    let an: any
    an = 111
    str = an // 依旧可以进行赋值，不会报类型不行
    ```

  * `unknown`类型的变量可以也可以接受任意类型的值，但不可以赋值给其他类型定义的变量，需要校验类型后才可以进行赋值
  * 函数、对象都是`Object`类型的值，所以不实用

* 变量支持类型
  类型 | 例子 | 描述
  :-: | :-: | :-
  number | 1，-33,2.5 | 任意数字
  string | 'hi'，"hi"，\`hi\` | 任意字符串
  boolean | true、false | 布尔值
  字面量 | 其本身 | 类似const定义（常量），但是通过 `|` 可以定义多个字面量
  any | * | 任意类型（关闭ts类型检测，和js一样了）
  unknown | * | 类型安全的any（类型为unknown的变量不可以赋值给其他类型的变量，当遇到类型不确定时用unknown，不要用any）
  void | 空值（undefined、null） | 没有值（函数中return后只能是undefined、null，或者return后没有  任何值）
  never | 没有值 | 用于不会返回结果。<br/>比较少用，一般用来抛出错误(程序在报错处中断)：<br/> const fn: never = (() => {throw new Error('错误')})()
  object | {name: '小明'} | 任意js对象
  array | [1,2,3] | 任意js数组
  tuple | [4,5] | 元组，ts新增类型，元素的类型和个数定义时就限定了
  enum | enum{A,B} | 枚举，ts新增类型

* 个别类型解说
  * `any`:
    * 当一个变量类型设置为`any`时，相当于对其关闭了Ts类型检测
    * 可以将它的值赋值给其他任意类型的值
    * 若仅仅声明变量，不给其定义类型时，则为隐式声明变量

    ```ts
    let b; // 隐式any
    let b: any; // 显式any
    ```
  
  * `object`:
    * 常用的是使用`{}`来指定对象中有哪些属性

      ```ts
        // 语法
        { // 此时，声明的对象只能是指定的键值对，不能多，也不能少
          
          属性： 属性值
          属性?: 属性值 // 多个键值对，可以通过向属性名后边加问号来表示可选
        }
        // 例：
        let a:{name: string, age?: number}
        // 包含指定类型，可写其他类型
        // [propName: string]: any -- propName为任意值（可以用任意字符代替）
        let b: {name: string, [prorName: string]: any} // [prorName: string]对应整个对象，若其值为string，则所有属性值均为string（已确定属性名的也需要遵守这个类型规定），只有值为any时，才可以允许多种类型共存。
        // 例：
        b = {name: "张三", age: 12}
      ```

    * 函数结构声明

      ```js
      // 语法： (形参: 类型, 形参: 类型, ...) => 返回值
      let b: (a: number, b: number) => number
      ```

  * `array`:

    ```ts
    /**
     * 语法：
     * 类型[]
     * Array<类型>
    */
    let arr: string[] // 声明一个字符串数组
    let arr1: Array<number> // 声明一个数值数组
    ```

  * `tuple`:

    ```ts
    /**
     * 语法：
     * [类型,类型,类型...]
     */
    let h: [string, string] 
    ```

  * `enum`:

    ```ts
    enum Gender {
      male,
      female
    }
    let i: {name: string, gender: Gender}
    ```

* 类型断言
  
  ```ts
  /** 语法
    * 变量 as 类型;
    * <类型> 变量
  */
  let s: any;
  let a: string;
  let b: unknown;

  a = b as string;
  // 或
  a = <string> b
  ```

* 类型的别名：
  
  ```ts
  type myt = 1 | 12| 23
  let k: myt;
  ```

* 面向对象
  * 操作都是通过对象来完成
  * 一切事物，到了程序里，都是以对象的形式存在
  * 事物所拥有的数据，到了程序里叫属性，所拥有的功能称之为方法

* 类
  * 直接写在类里的叫实例属性
    * 仅实例可访问
  * 属性前关键字
    * `static`：类属性/静态属性，仅类可访问
    * `readonly`：只读属性，无法修改
	* `private`：私有属性，只能在类内部进行访问修改（继承类中也不可访问）
	* `protected`：受保护的属性，只能在当前类和当前类的子类中使用（实例上也不可访问）。
  * 以`abstract`做修饰的类，是抽象类，不可创建对象
    * 抽象类中可以定义抽象方法
	* 抽象方法定义在抽象类中，前边要加上`abstract`修饰词
  * 接口是以`interface`做修饰的类。用来规定一个类的结构。
    * 用来定义一个类中应该包含哪些属性和方法
	* 与`type`关键字相似，可以当成类型声明去使用。
	* 与`type`关键字不同的是，`type`关键字不可重复声明而接口可以重复声明，最终结果以多个同名接口合并为准。
	* 接口中的所有属性都不能有实际值，所有方法都是抽象方法。不需要加关键字做修饰。

* 泛型
  * 在定义函数或类时，遇到类型不明确时使用
  * 可以同时指定多个泛型
  
```js
// 定义及使用泛型
function abc<T>(a:T):T {
  return a;
}
function abc<T,K>(a:T, b:K):T { // 定义多个泛型
  return a;
}
// 调用泛型
abc(10); // 不指定泛型，TS可以自动对类型进行推断
abc<string>('hello'); // 指定泛型

interface Inter { // 接口
  length: number;
}

function fn<T extends Inter>(a:T):number { // T extends Inter 表示泛型T必须是Inter的实现类（子类）
  return a.length;
}
```
