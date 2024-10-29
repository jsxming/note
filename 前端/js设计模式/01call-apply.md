call是包装在apply上面的一颗语法糖，本质没有区别

```typescript
// apply 传递数组
var func = function( a, b, c ){
  alert ( [ a, b, c ] );    // 输出 [ 1, 2, 3 ]
};

func.apply( null, [ 1, 2, 3 ] );

// call 传递普通参数
var func = function( a, b, c ){
  alert ( [ a, b, c ] );    // 输出 [ 1, 2, 3 ]
};

func.call( null, 1, 2, 3 );
```







```typescript
function say() {
  console.log(arguments);
  //取出arguments对象第一个参数
  let firstArg = [].shift.call(arguments)
  
  //因为slice内部实现是使用的this为代表调用对象，
  //那么当[ ].slice.call() 传入 arguments 对象的时候，通过 call 函数改变原来 slice 方法的 this 指向, 使其指向 arguments，并对 arguments 进行复制操作，然后返回一个新数组。
  //所以就达到了把 arguments 类数组转为数组的目的！
  let args = [].slice.call(arguments)
  console.log(firstArg);
  console.log(args);
}
say(1, 2, 3)
```

