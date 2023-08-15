reduce:

reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。

要执行的函数，要执行的函数中也可以传入参数，如下

prev：上次调用函数的返回值

cur：当前元素

index：当前元素索引

arr：被遍历的数组

函数迭代的初始值

array.reduce(function(prev, currentValue, currentIndex, arr), initValue)

```javascript
const arr = [11, 22, 33, 44]
const sum = arr.reduce((prev, cur, index) => {
   console.log(prev, cur, index);
   return prev + cur
})
console.log('sum:', sum);

const arr = [11, 22, 33, 44]
const sum = arr.reduce((prev, cur, index) => {
   console.log(prev, cur, index);
   return prev + cur
},0)
console.log('sum:', sum);

```

```javascript
const arr = [11, 22, 33, 44]
const sum = arr.reduce((prev, cur, index) => {
   console.log(prev, cur, index);
   return prev + cur
}, 10)
console.log('sum:', sum);

```

```javascript
var arr = [1, 2, 3, 4];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
})
console.log(arr, sum);

var  arr = [1, 2, 3, 4];
var sum = arr.reduce(function(prev, cur, index, arr) {
    console.log(prev, cur, index);
    return prev + cur;
},0);
console.log(arr, sum);
```

```javascript
var  arr = [1, 2, 3, 4];
var sum = arr.reduce((x,y)=>x+y)
var mul = arr.reduce((x,y)=>x*y)
console.log( sum ); //求和，10
console.log( mul ); //求乘积，24
```

```javascript
let arr = [[0, 1], [2, 3], [4, 5]]
let newArr = arr.reduce((pre,cur)=>{
    return pre.concat(cur)
},[])
console.log(newArr); // [0, 1, 2, 3, 4, 5]

```

```javascript
var result = [
    {
        subject: 'math',
        score: 10
    },
    {
        subject: 'chinese',
        score: 20
    },
    {
        subject: 'english',
        score: 30
    }
];

var sum = result.reduce(function(prev, cur) {
    return cur.score + prev;
}, 0);
console.log(sum) //60

```

#### 计算数组中每个元素出现的次数

```javascript
const arr = ['张三', '李四', '张三', '王五', '赵六', '王五', '张三']
const obj = arr.reduce((prev, cur) => {
  console.log(prev, cur);
  if (cur in prev) {
    prev[cur] += 1
  } else {
    prev[cur] = 1
  }
  return prev
}, {})
console.log(obj);

```

#### 数组去重

```javascript
const arr = ['张三', '李四', '张三', '王五', '赵六', '王五', '张三']
const newArr = arr.reduce((prev, cur) => {
   console.log(prev, cur);
   if (!prev.includes(cur)) {
     prev.push(cur)
   }
   return prev
 }, [])
 console.log(newArr);

```

#### 扁平化数组

```javascript
const arr = [
 {
    label: '水果',
    children: [
      {
        label: '香蕉'
      },
      {
        label: '橘子'
      }
    ]
  },
  {
    label: '蔬菜',
    children: [
      {
        label: '西红柿'
      },
      {
        label: '白菜'
      }
    ]
  },
  {
    label: '小猫'
  },
  {
    label: '小狗'
  }
]
const func = (res) => res.reduce((prev, cur) => {
  console.log(prev, cur)
  return prev.concat(Array.isArray(cur.children) ? func(cur.children) : cur)
}, [])
console.log(func(arr))

```

