---
title: JavaScript常用方法
tags: JavaScript function
---

* 数组去重

    ```js
    // 双层for循环去重
    function deWeight(Arr) {
      for (let i = 0; i < Arr.length; i++) {
          for (let j = i+1; j < Arr.length; j++ ) {
              if (Arr[i] === Arr[j]) {
                  Arr.splice(j, 1)
                  j--;
              }
          }
      }
      return Arr;
    }
    //indexOf去重
    function deWeight(Arr) {
      if (!Array.isArray(Arr)) {
          console.log('输入的不是数组');
          return;
      }
      let array = [];
      for(let i = 0; i < Arr.length; i++){
          if (array.indexOf(Arr[i]) === -1){
              array.push(Arr[i]);
          }
      }
      return array;
    }
    // 解构赋值去重
    function deWeight(Arr) {
      return [...new Set(Arr)]
    }
    // set去重
    function deWeight(Arr) {
      return Array.from(new Set(Arr))
    }
    ```

* 数组排序

  ```js
  // 快排 - 传入数值数组，返回数组
  function quickSort(arr) {
    if (arr.length <= 1) { return arr; }
    const pivotIndex = Math.floor(arr.length / 2);
    const pivot = arr.splice(pivotIndex, 1)[0];
    let left = [], right = [];
    for (let i = 0; i < arr.length; i++){
      if (arr[i] < pivot) {
        left.push(arr[i]);
      } else {
        right.push(arr[i]);
      }
    }
    return quickSort(left).concat([pivot], quickSort(right));
  }
  ```

* 深拷贝

   ```js
    function deepClone(source) {
      const targetObj = source.constructor === Array ? [] : {}
      for(let keys in source){
        if(source.hasOwnProperty(keys)){
          if(source[keys] && typeof soure[keys] === 'Object'){ // 引用类型
            targetObj[keys] = source[keys].constructor === Array ? [] : {}// 判断对象还是数组
            targetObj[keys] = deepClone(source[keys])
          }else{
            tyargetObj[keys] = source[keys] // 基础类型
          }
        }
      }
      return targetObj
    }
   ```

* 类型输出判断

  ```js
  function type(target) {
    let ret = typeof(target)
    let template = {
      "[object Array]" : "array",
      "[object Object]" : "object",
      "[object Number]" : "object——number",
      "[object Boolean]" : "object——boolean",
      "[object String]" : "object——string",
    }
    switch(ret) {
      case null : return 'null'
      case 'object' : {
        const str = Object.prototype.toString.call(target)
        return template[str]
      }
      default : return  ret
    }
  }
  ```

* 解析url路径
  
  ```js
  //2、有这样一个字符串"http://www.baidu.com?a=1&b=2&c=&a=5&d=xxx",要求转化成 {"a":[1,5],"b":2,"c":,"d":xxx}
  
  function digit(str) {
    const obj = {}
    str.split['?'](1)
      .split('&')
      .map((val)=> val.split('='))
      .map(val=>{
        obj[val[0]] ?
          Array.isArray(obj[val[0]]) ?
            obj[val[0]].push(val[1]) 
            : obj[val[0]] = [obj[val[0]], val[1]]// 已经有了
          : obj[val[0]] = val[1] // 没有
      })
    return obj
  }
  // 通过正则处理 - 不能处理数组等特殊数据
  function digit(str) {
    return JSON.parse('{"' + decodeURI(str.split('?')[1]).replace(/&/g,'","').replace(/=/g, '":"') + '"}')
  }
  ```

* 合并对象`Object.assign(obj1, obj2)`

* 随机颜色

  ```js
  function color() {
    return `#${Math.random().toString(16).substring(2,8)}`
  }
  ```

* 获取字符串总字节数

  ```js
  function retBytees(str) {
    let num = str.length;
    for(var i = 0; i < str.length; i ++){
      if(str.charCodeAt(i) > 255){ // 汉字的字符编码大于255  ；charCodeAt（i）：返回下标对应的字符编码数值
        num++
      }
    }
    return num
  }
  ```
  