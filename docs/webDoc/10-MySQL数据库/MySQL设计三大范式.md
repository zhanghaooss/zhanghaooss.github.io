 : 01-Bootstrap入门
date:12/13
---

## 前言

范式即规范。MySQL 范式的作用是：

- 让我们建的表更佳简洁和高效。

- 让功能独立化，避免耦合。

## MySQL 设计三大范式

**数据库设计三范式**

设计数据库表的时候所依据的规范，共三个规范：

```undefined
第一范式：要求有主键，并且要求每一个字段原子性不可再分

第二范式：要求所有非主键字段完全依赖主键，不能产生部分依赖

第三范式：所有非主键字段和主键字段之间不能产生传递依赖
```

### 第一范式

数据库表中不能出现重复记录，每个字段是原子性的不能再分

不符合第一范式的实例：

![img](https:////upload-images.jianshu.io/upload_images/4807654-eab3b56930c4f552.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/587/format/webp)

`第一范式1.PNG`

存在问题:

最后一条记录和第一条重复（不唯一，没有主键）
联系方式字段可以再分，不是原子性的

![img](https:////upload-images.jianshu.io/upload_images/4807654-03cb8c62dabb930c.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/585/format/webp)

`第一范式2.PNG`

关于第一范式，每一行必须唯一，也就是每个表必须有主键，这是数据库设计的最基本要求，主要采用数值型或定长字符串表示，关于列不可再分，应该根据具体的情况来决定。如联系方式，为了开发上的便利可能就采用一个字段。

### 第二范式

第二范式是建立在第一范式基础上的，另外要求所有非主键字段完全依赖主键，不能产生部分依赖

实例：

![img](https:////upload-images.jianshu.io/upload_images/4807654-b922f9c1d1814859.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/586/format/webp)

`二1.PNG`

确定主键：

![img](https:////upload-images.jianshu.io/upload_images/4807654-b94e4f8adcc2f9cd.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/586/format/webp)

`二2.PNG`

以上虽然确定了主键，但此表会出现大量的冗余，主要涉及到的冗余字段为“学生姓名”和“教师姓名”，出现冗余的原因在于，学生姓名部分依赖了主键的一个字段学生编号，而没有依赖教师编号，而教师姓名部分依赖了主键的一个字段教师编号，这就是第二范式部分依赖。

解决：

![img](https:////upload-images.jianshu.io/upload_images/4807654-ef1c439e3cd7e13b.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/580/format/webp)

`解决.PNG`

如果一个表是单一主键，那么它就是复合第二范式，部分依赖和主键有关系

以上是典型的“多对多”设计

### 第三范式

建立在第二范式基础上的，非主键字段不能传递依赖于主键字段（不要产生传递依赖）

![img](https:////upload-images.jianshu.io/upload_images/4807654-c906d1f0cf0c4227.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/586/format/webp)

`三1.PNG`

上表中，班级名称字段存在冗余，因为班级名称字段没有直接依赖于主键，班级名称字段依赖于班级编号，班级编号依赖于学生编号，这就是传递依赖，解决的办法就是将冗余字段单独拿出来建立表：

![img](https:////upload-images.jianshu.io/upload_images/4807654-a6b47b46a606682f.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/593/format/webp)

`解决三.PNG`

以上设计是典型的一对多的设计，一存储在一张表中，多存储在一张表中，在多的那张表中添加外键指向一的一方

### 几个经典的设计：

一对一：

```undefined
第一种方案：分两张表存储，共享主键
第二种方案：分两张表存储，外键唯一
```

一对多：

```undefined
分两张表存储，在多的一方添加外键，
这个外键字段引用一的一方中的主键字段
```

多对多：

```undefined
分三张表存储，在学生表中存储学生信息，在课程表中存储课程信息，
在学生选课表中存储学生和课程的关系信息
```

![img](https:////upload-images.jianshu.io/upload_images/4807654-85473b90ab1feee7.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/528/format/webp)

`共享主键.PNG`

![img](https:////upload-images.jianshu.io/upload_images/4807654-13ec5e1d12a119b0.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/323/format/webp)

`外键唯一.PNG`

实际开发中，数据库设计尽量遵循三范式，但是还是根据实际情况进行取舍，有时可能会拿冗余换速度，最终目的是要满足客户需求

作者：我可能是个假开发

链接：https://www.jianshu.com/p/3e97c2a1687b
