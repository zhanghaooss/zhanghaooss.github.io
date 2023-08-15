forEach:用来遍历数组

需要一个函数作为参数

```javascript
var  arr=['戴尔','华硕','联想','宏基'];
arr.forEach((val,index)=>{
	console.log(val,index);
});
```

```javascript
var arr=[11,22,33,44,55];
arr.forEach(item=>{
 	console.log(item);
})
```

```javascript
var arr = [5,4,3,2,1]
//item为当前项，这个参数是必须要的 其他可以不写
arr.foreach((item,index,self)=>{
          console.log(item)    -----数组中对应的一项数据（当前数组中一个元素）
          console.log('第' + index + '个')    ------当前元素下标
          console.log(self)            ------数组本身
})
```

```javascript
var arr = [1, 3, 5, 13, 2];
var res = arr.forEach(function(item,index) {
    console.log(`数组第${index+1}个元素是${item}`);
})
console.log(res);//forEach的返回值为undefined,

```

```javascript
 var arr = [1, 3, 5, 13, 2];
 arr.forEach(function(item) {
     if (item == 3) {
        return console.log('找到了3，但未结束遍历');
      }
      console.log(item);
  })

```

```javascript
var arr2 = [1, 3, , 13, 2];
var newarr = [];
arr2.forEach(function(item, index) {  //forEach循环时会跳过空值,在函数中对数组添加新元素不会增加循环次数。也就是初始长度决定了遍历的次数。
    console.log(item);
    if (arr2[index] == 3) {
       arr2.push(14);
       arr2.push(5);
    }
    console.log("数组循环了" + index + "次");
})
console.log(arr2);
```

面试题:

1. forEach和for of的区别:

​				区别一：for in 和 for of 都可以循环数组，for in 输出的是数组的index下标，而for of 输出的是数组的每一项的值。
​				区别二：for in 可以遍历对象，for of 不能遍历对象。

​				for in适合遍历对象，for of适合遍历数组。for in遍历的是数组的索引，对象的属性，以及原型链上的属性。

2. for in循环可以遍历到原型对象上的属性吗? 答:可以遍历到

```javascript
var arr=[11,22,33];
for(var i in arr){
    console.log(i); // 0  1  2  i在此处代表的是下标
}
for(var i of arr){
    console.log(i);// 11  22  33  i代表的是数组中的元素
}


function Per(name,age){
	this.name=name;
    this.age=age;
}
var p1=new Per('zs',26);
Per.prototype.className='前端';
Per.prototype.say=function(){
   console.log(`我叫${this.name},我今年${this.age}`);
}
console.log(p1);
for(var i in p1){
    console.log(i); //name age  className say   可以看到for in循环是可以遍历到原型对象上的属性的
}
```

# JS 数组转树状结构

```javascript
  // 需转化数组示例
  data = [

  { id: '01', lable: '项目经理', pid: '' },

  { id: '02', lable: '产品leader', pid: '01' },

  { id: '03', lable: 'UIleader', pid: '01' },

  { id: '04', lable: '技术leader', pid: '01' },

  { id: '05', lable: '测试leader', pid: '01' },

  { id: '06', lable: '运维leader', pid: '01' },

  { id: '07', lable: '产品经理', pid: '02' },

  { id: '08', lable: '产品经理', pid: '02' },

  { id: '09', lable: 'UI设计师', pid: '03' },

  { id: '10', lable: '前端工程师', pid: '04' },

  { id: '11', lable: '后端工程师', pid: '04' },

  { id: '12', lable: '后端工程师', pid: '04' },

  { id: '13', lable: '测试工程师', pid: '05' },

  { id: '14', lable: '测试工程师', pid: '05' },

  { id: '15', lable: '运维工程师', pid: '06' }

 ]

```

**注**: 如某个对象的 `pid` 与某个对象的 `id` 相同, 则此 `pid 对象` 为此 `id 对象` 的 `子级`, 按此需求进行转化 ;

转换后的效果如下:

```javascript
// 转化结果示例
treeData = [
    {
      id: '01',
      lable: '项目经理',
      pid: '',
      children: [
        {
          id: '02',
          lable: '产品leader',
          pid: '01',
          children: [
            {
              id: '07',
              lable: '产品经理1',
              pid: '02',
              children: []
            },
            {
              id: '08',
              lable: '产品经理2',
              pid: '02',
              children: []
            }
          ]
        },
        {
          id: '03',
          lable: 'UIleader',
          pid: '01',
          children: [
            {
              id: '09',
              lable: 'UI设计师',
              pid: '03',
              children: []
            }
          ]
        },
        {
          id: '04',
          lable: '技术leader',
          pid: '01',
          children: [
            {
              id: '10',
              lable: '前端工程师',
              pid: '04',
              children: []
            },
            {
              id: '11',
              lable: '后端工程师1',
              pid: '04',
              children: []
            },
            {
              id: '12',
              lable: '后端工程师2',
              pid: '04',
              children: []
            }
          ]
        },
        {
          id: '05',
          lable: '测试leader',
          pid: '01',
          children: [
            {
              id: '13',
              lable: '测试工程师1',
              pid: '05',
              children: []
            },
            {
              id: '14',
              lable: '测试工程师2',
              pid: '05',
              children: []
            }
          ]
        },
        {
          id: '06',
          lable: '运维leader',
          pid: '01',
          children: [
            {
              id: '15',
              lable: '运维工程师',
              pid: '06',
              children: []
            }
          ]
        }
      ]
    }
  ]

```

代码实现:

```javascript
  function toTree(data) {
    // 1.定义最外层的数组
    const tree = []
    // 2.定义一个空对象
    const otherObj = {}
    // 3.遍历数组内所有对象
    data.forEach(item => {
      // 3.1.给每个当前对象添加一个 children 属性, 以便存放子级对象
      item.children = []
      // 3.2 将当前对象的 id 作为键, 与当前对象自身形成键值对
      otherObj[item.id] = item
    })

    // 4.再次遍历数组内所有对象
    data.forEach(item => {
      // 4.1.判断每个当前对象的 pid, 如当前对象 pid 不为空, 则说明不是最上级的根对象
      if (item.pid) {
          // 4.3.利用当前对象的 otherObj[pid] 找到 otherObj[id] 中对应当前对象的父级对象, 将当前对象添加到其对应的父级对象的 children 属性中
          otherObj[item.pid].children.push(item)
      } else {
        // 4.3.当前对象 pid 如果为空, 则为树状结构的根对象
        tree.push(item)
      }
    })
    // 5.返回树状结构
    return tree
  }

toTree(data)
```

