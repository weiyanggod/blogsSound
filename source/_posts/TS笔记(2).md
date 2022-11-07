---
title: typescript笔记(2)-enum,type和interface
tags: typescript
cover: ../img/014ae65e78353ba80120a89596f262.jpg
categories: typescript笔记
abbrlink: 61325
---

# enum 类型

<font color='red' size='5'>用法:用于对数据做映射</font>

### 第一种用法

```ts
// 当后端传回的数据是1,2,3,4,分别对应不同的状态时,可以用enum做映射
enum person {
  '完成' = 1, //1表示真实值,'完成'表示映射值
  '进行中' = 2,
  '未完成' = 3
}

let a: person = 1 || person['完成']
// a可以表示成1或者person['完成'] ,当表示为person['完成'] 时,会自动转换成1
```

### 第二种用法

```ts
// 用于做权限控制时
enum authority {
  none = 0, // 无权限 二进制表示为:0000
  read = 1 << 0, // 二进制表示为:0001
  write = 1 << 1, // 左移一位,二进制表示为:0010
  remove = 1 << 2, //左移两位,二进制表示为:0100
  admin = read | write | remove //管理员,二进制表示为: 0111
}

type User = {
  authority: authority
}

const user: User = {
  authority: 0b0011 // 0b表示后面的值为二进制
}
// 权限判断
if ((user.authority & authority.read) === authority.read) {
  // 当用户的authority字段并上我们之前声明的authority类型里的某个权限还等于这个权限时
  //表示这个用户有这个权限,说明此用户的二进制包含了authority类型里的这个权限
  // 例如:当用户的权限二进制为0011时,包含了read的二进制0001
  console.log(1)
}
if ((user.authority & authority.write) === authority.write) {
  // 写权限
  console.log(2)
}
// 因为用户权限的的二进制为0011,同时包含了read,write,所以这里会同时打印1和2
```

# type 和 interface

## type

<font color='blue'>
type用于给一个类型去一个别名,例如:
</font>

```ts
type A = number
// 把A做了一个初始化,他的值就等于所以属于number这个属性的值
*注意* type无法重复声明
type A = string // 这里会报错,因为A已经声明成number了
//  可以使用交集
type B = A & string // 这样B的值就等于number+string两个集合
```

## interface

<font color='blue'>
interface用于声明接口(描述对象的属性),例如:
</font>

```ts
interface A {
  a: number
  b: string
  // 索引签名的方式描述
  // [k: string]: number
}

// 描述数组

interface B extends Array<string> {
  // 额外的属性
  name: string
}
// type写法:
/* type B = Array<string> {
  // 额外的属性
  name: string
} */

// 描述函数

interface Fn {
  // 括号里是参数,冒号后面是返回值的类型
  (a: string): void
  // 额外的属性
  b: number
}
// 使用
const a: Fn = a => {}
a.b = 1

// 描述日期

interface C extends Date {}
// 使用
const c: C = new Date()
```

<font color='red'>
type和interface的区别
</font>
</br>
<font color='pink'>
区别1:
interface只描述对象,type可以描述所有类型
</br>
区别2:type只是别名,interface是类型声明(面试可能问,我们实际使用基本无法感觉到差别)
</br>
区别3:type无法重新赋值(缺点是无法扩展),interface可以
</font>

```ts
interface会自动合并, type不会
例: interface A {
  a: number
  b: string
}
interface A {
  c: () => void
}
// A最终的类型是a,b,c的集合
```
