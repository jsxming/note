##### 1、索引签名

在TypeScript中，如果实际赋值的对象的属性与方法比接口中定义的属性与方法多，那么将该对象的值赋给该接口类型会引起编译错误。例如，在以下代码中，接口中只定义了name和age属性，但实际赋值的对象还有height属性，这会引起编译错误

要解决这个问题，使用可选修饰符“?”将height作为可选参数加入接口定义中。

但这只适用于已知有哪些可选属性或方法的情况，如果遇到完全不确定有哪些可选属性或方法的情况应该怎么办呢？

这时就需要用到索引签名。通过索引签名，让接口支持任意数量的可选属性



TypeScript只支持两种类型的索引——字符串索引和数值索引

```typescript
interface 接口名称 {
    ...
    [索引名称:索引类型]:属性类型;
    ...
}
    
//字符串索引
interface Person {
    name: string,
    age: number,
    [index: string]: any
}

//数值索引
interface Person2 {
    name: string,
    age: number,
    [index: number]: any
}

//索引表示所有属性或方法不仅包含未定义的属性或方法，还包含已定义的所有属性和方法
interface Person {
    name: string,
    //编译错误：类型"number"的属性"age"不能赋给"string"索引类型"string"。ts(2411)
    age: number,
    [index: string]: string
}

//可以解决上面的问题
interface Person {
    name: string,
    age: number,
    [index: string]: string | number
}

//最佳 直接用any
interface Person {
    name: string,
    age: number,
    [index: string]: any
}
```





##### 接口继承合并

```typescript

interface Animal {
    name: string,
    age: number,
    eat: (food: string) => void
}

// 继承
interface Bird extends Animal {
    wings: string,
    fly: () => void
}


interface Colorful{
  color:string
}

//交叉类型
type Ca = Animal & Colorful

```

