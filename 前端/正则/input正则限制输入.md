### 常用正则

```typescript
// 只能填写中英文
export const wordReg = /^[a-z\u4e00-\u9fa5]+$/i;

// 电话号码
export const phoneNumberReg = /^1[3-9]\d{9}$/;
// 座机号
export const telNumberReg = /^0\d{2,3}-\d{7,8}$/;

// 链接正则
export const isUrlReg = /^(https?:\/\/)?([\da-z\\.-]+)\.([a-z\\.]{2,6})([\\/\w \\.-]*)*\/?$/;

// 数字和中文
export const numberAndCN = /^[0-9\u4e00-\u9fa5]+$/; // 中文+数字，不要求同时出现

// 中文+英文
export const wordAndEnReg = /^[\u4e00-\u9fa5a-zA-Z]+$/;

//英文+数字
export const numAndEnReg = /^[a-zA-Z0-9]+$/;

//输入 4位整数+2位小数 
export const num =/^(?:\d{1,4})(\.\d{0,2})?$/;

```





### 正则限制输入 4位整数+2位小数

限制用户输入会有很多问题，不建议做这种需求，非要做只能用自己写的虚拟键盘！ 

```javascript
  const value = e.target.value as string;
  //最多4位整数+ 2位小数
  const regex = /^(?:\d{1,4})(\.\d{0,2})?$/;
  if (!regex.test(value)) {
    // 如果输入值不符合正则表达式，则将其设置为上一个有效值
    return value.slice(0, value.length - 1);
  }
  return value;
```