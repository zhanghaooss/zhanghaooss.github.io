map:

### 1.数组map方法的作用 : 映射数组

按照某种映射关系,把数组的每一个元素给修改了

### 2.语法: array.map( function ( item, index, arr) {} )

第一个参数:item,必须,当前元素的值

第二个参数:index,可选,当前元素在数组中的索引值

第三个参数:arr,当前元素属于的数组对象

### 3.map方法特点

(1)函数执行次数 === 数组长度

(2)函数内部的return

return 新的元素

如果没有return,则map的返回值都是undefined

(3)map方法的返回值

返回映射之后的新数组

### 4.注意点:

(1)map()方法不会对空数组进行检测

(2)map()方法不会改变原始数组



练习:

```javascript
const arr = [88,90,100,20,50]
const res = arr.map(item => item * 0.8)
```

```javascript
var arr=[1,2,3,4,5];
var arr1=arr.map(function (item,index,arr) { /
    return item*2;
});
console.log(arr1);
```

```javascript
let list = [
        { name: '张三', age: 18 },
        { name: '李四', age: 19 },
        { name: '王五', age: 20 },
        { name: '老六', age: 66 },
];
let newArr = list.map((obj) => obj.name);
console.log(newArr); 

```

```javascript
var data = [1, 2, 3, 4];

var arrayOfSquares = data.map(function (item) {

　　return item * item;

});

alert(arrayOfSquares); // [1, 4, 9, 16]
```

```javascript
 var arr = ["Tom","Jack","Lucy","Lily","May"];
    var a = arr.map(function(value,index,self){
        console.log(value + "--" + index + "--" + (arr === self))
    })


    var arr = ["Tom","Jack","Lucy","Lily","May"];
    var a = arr.map(function(value,index,self){
        return "hi:"+value;
    })
    console.log(a); 
```

```javascript
var persons = [
  {firstname : "Malcom", lastname: "Reynolds"},
  {firstname : "Kaylee", lastname: "Frye"},
  {firstname : "Jayne", lastname: "Cobb"}
];


function getFullName(item) {
  var fullname = [item.firstname,item.lastname].join(" ");
  return fullname;
}

function myFunction() {
  document.getElementById("demo").innerHTML = persons.map(getFullName);
}
```

```javascript
const array1 = ['周一','周二','周三','周四','周五','周六','周日']
const array2 = ['1','2','3','4','5','6','7']
const newArr = array1.map((item,index) => {
  return {
    name:item,
    id:array2[index]
  }
})
console.log(newArr)

```

```javascript
const users = [
  {firstName : "White", lastName: "Green"},
  {firstName : "Red", lastName: "Pink"},
  {firstName : "Yellow", lastName: "Black"}
];
 
const userFullnames = users.map(function(element){
    return `${element.firstName} ${element.lastName}`;
})
 
console.log(userFullnames);

```

```javascript
const numbers = [1, 2, 3, 4, 5];
const filteredNumbers = numbers.map(function (num, index) {
  if (index < 3) {
    return num;
  }
});

console.log(filteredNumbers);

```

```javascript
const arr = [1, 5, 10, 15, 66, 200];

const newArr = arr.map(function (item) {
  if (item > 15) {
    item += 1
  }
  return item
});

console.log(arr);
console.log(newArr);

```

```javascript
const arr = [
  { name: 'wfly', age: 18 },
  { name: 'fly', age: 8 },
  { name: 'fnn', age: 18 },
];

const newArr = arr.map(function (item) {
  if (item.name.includes('fly')) {
    item.age += 1
  }
  return item
});

console.log(arr);
console.log(newArr);

```

```javascript
const arr = [3, 5, 10, 15, 66, 200];

const newArr = arr.map(function (item, index) {
  arr[index] = arr[index] + 10
  arr.push(999)
  console.log('当前 arr 的值：', arr);
  arr.unshift(111)
  return item
});

console.log(arr);
console.log(newArr);

```

