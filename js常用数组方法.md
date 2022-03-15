# 数组常用方法

## 一、操作方法

数组基本操作可以归纳为增、删、改、查，需要留意哪些方法会对原数组产生影响  

### 增

push()、unshift()、splice()会对原数组产生影响，concat()不会影响

+ push() 方法接收任意数量的参数，并将它们添加到数组末尾，<font color=#c2185b >返回数组的最新长度</font>

```javascript
  let colors = [];
  let count = colors.push('red','green');
  console.log(count) // 2
```  

+ unshift() 方法接收任意数量的参数，并将它们添加到数组开头，<font color=#c2185b >返回数组的最新长度</font>

```javascript
  let colors = new Array();
  let count = colors.unshift('red','green');
  alert(count) // 2
```  

+ splice() 传入三个参数：开始位置、要删除的元素数量（0）、插入的元素，返回空数组

```javascript
    let colors = ["red","green","blue"];
    let removed = colors.splice(1, 0, "yellow", "orange");
    console.log(colors) // ["red", "yellow", "orange", "green", "blue"]
    console.log(removed) // []
```

+ concat() 首先会创建一个当前数组的副本，然后再把它的参数添加到副本末尾，最后返回这个新构建的数组，不会影响原始数组

```javascript
    let colors = ["red","green","blue"];
    let colors2 = colors.concat("yellow",["black", "brown"]);
    console.log(colors) // ["red","green","blue"]
    console.log(colors2) // ["red","green","blue","yellow","black","brown"]
```

### 删

pop()、shift()、splice()会影响原数组，slice()不会影响  

+ pop() 方法用于删除数组的最后一项，同时减少数组的 length 值，<font color=#c2185b >返回被删除的项</font>

```javascript
    let colors = ["red", "green"];
    let item = colors.pop();
    console.log(item) // green
    console.log(colors.length) // 1
```

+ shift() 方法用于删除数组的最后一项，同时减少数组的 length 值，<font color=#c2185b >返回被删除的项</font>

```javascript
    let colors = ["red", "green"];
    let item = colors.shift();
    console.log(item) // red
    console.log(colors.length) // 1
```

+ splice() 传入两个参数：开始位置、要删除的元素数量，返回包含删除元素的数组

```javascript
    let colors = ["red","green","blue"];
    let removed = colors.splice(0, 1);
    console.log(colors) // ["green", "blue"]
    console.log(removed) // ["red"]
```

+ slice() 接收两个参数：开始位置、结束位置（不包括该元素，如果省略，将选择从开始位置到数组末尾的所有元素），返回包含删除元素的数组，使用负数则从数组的末尾进行选择

```javascript
    let colors = ["red","green","blue","yellow","purple"];
    let colors2 = colors.slice(1);
    let colors3 = colors.slice(-4,-1)
    console.log(colors) // ["red","green","blue","yellow","purple"]
    console.log(colors2) // ["green","blue","yellow","purple"]
    console.log(colors3) // ["green","blue","yellow"]
```

### 改

即修改原来数组的内容，常用splice

+ splice() 传入三个参数：开始位置、要删除的元素数量、要插入的任意多个元素，返回包含删除元素的数组，对原数组产生影响

```javascript
    let colors = ["red","green","blue"];
    let removed = colors.splice(1, 1, "yellow", "black");
    console.log(colors) // ["red","yellow","black","blue"]
    console.log(removed) // ["green"]
```

### 查

即查找元素，返回元素坐标或者原始值  

+ indexOf() 返回要查找的元素第一次在数组中的位置，如果没找到则返回-1

```javascript
    let numbers = [1,2,3,4,5,4,3,2,1]
    numbers.indexOf(4) // 3
```

+ includes() 返回要查找的元素在数组中的位置，找到返回true，否则false

```javascript
    let numbers = [1,2,3,4,5,4,3,2,1]
    numbers.includes(4) // true
```

+ find() 返回第一个匹配的元素

```javascript
    const people = [
        {
            name: "Matt",
            age: 27
        },
        {
            name: "Nicholas",
            age: 29
        }
    ];
    people.find((element, index, array) => element.age < 30) // {name: "Matt", age: 27}
```

## 二、排序方法

数组有两个方法可以用来对元素重新排序：

+ reverse() 将数组反转

```javascript
    let values = [1,2,3,4,5];
    values.reverse();
    console.log(values) // [5,4,3,2,1]
```

+ sort() 接收一个比较函数，用于判断哪个值排在前面

```javascript
    let values = [0,1,5,10,15];
    values.sort((a,b) => a-b);
    console.log(values) // [0,1,5,10,15]
    values.sort((a,b) => b-a);
    console.log(values) // [15,10,5,1,0]
```

## 三、转换方法  

将数组转换为字符串

+ join() 方法接收一个参数，即字符串分隔符，返回包含所有项的字符串

```javascript
    let colors = ["red","green","blue"]
    console.log(colors.join(",")) // red,green,blue
    console.log(colors.join("||")) // red||green||blue
```

## 四、迭代方法

常用来迭代数组的方法（都不改变原数组）

+ some() 对数组每一项都运行传入的函数，如果有一项函数返回 true ，则这个方法返回 true

```javascript
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let someResult = numbers.some((item, index, array) => item > 2);
    console.log(someResult) // true
```

+ every() 对数组每一项都运行传入的函数，如果对每一项函数都返回 true ，则这个方法返回 true

```javascript
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let everyResult = numbers.every((item, index, array) => item > 2);
    console.log(everyResult) // false
```

+ forEach() 对数组每一项都运行传入的函数，没有返回值

```javascript
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    numbers.forEach((item, index, array) => {
        // 执行某些操作
    });
```

+ filter() 对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回

```javascript
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let filterResult = numbers.filter((item, index, array) => item > 2);
    console.log(filterResult) // [3,4,5,4,3]
```

+ map() 对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组

```javascript
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let mapResult = numbers.map((item, index, array) => item * 2);
    console.log(mapResult) // [2,4,6,8,10,8,6,4,2]
```

## 五、归并方法

+ reduce() 会对数组每一项进行遍历，同时将前面数组项遍历产生的结果与当前遍历项进行运算

```javascript
    // 语法
    array.reduce((prev, cur, index, arr)=>{}, init)
    /* 
        prev: 必需。初始值，或计算结束后的返回值
        cur：必需。当前元素
        index：可选。当前元素的索引
        arr：可选。当前元素所属的数组对象
        init：可选。传递给函数的初始值，相当于 prev 的初始值
    */

   // 数组求和
    const arr = [12, 34, 23];
    const sum = arr.reduce((prev, cur) => prev + cur, 0);
```

+ reduceRight() 该方法用法与reduce()其实是相同的，只是遍历的顺序相反，它是从数组的最后一项开始，向前遍历到第一项
