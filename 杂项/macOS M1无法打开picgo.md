macOS M1 用户如果遇到安装 PicGo 无法打开的情况（比如提示文件已损坏或者不信任的开发者），参考以下步骤：

信任开发者，会要求输入密码:

```
sudo spctl --master-disable
```



然后放行 PicGo :

```
xattr -cr /Applications/PicGo.app
```

然后再启动即可。