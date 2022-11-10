##  1、ios12 不能动态注入userAgent字段

需求：需要ios动态注入pathname字段用于表示webview进入时的第一个页面的pathname，在header组件中的返回功能使用，判断当前location.pathname如果是

等于注入的pathname则调用原生app的viewback方法返回到原生页面中，如果不等于则返回调用history.go(-1)



问题：目前大部分机型、版本没有问题，只出现在ios12系统版本中，由于ios每次打开新的webview都要动态修改userAgent的pathname字段，但是系统却不支持就导致了h5无法正确获取参数



####  解决办法

- 调用app桥动态获取该变量，不存储在userAgent中

- 让ios在window.localStorage对象上挂载该变量