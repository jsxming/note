```shell
#用于新建一个标签，默认为HEAD，也可以指定一个commit id；
git tag <tagname>

# 指定标签信息；
git tag -a <tagname> -m "blablabla..."

# 可以查看所有标签。
git tag



# 推送一个本地标签；
git push origin <tagname>

# 推送全部未推送过的本地标签；
git push origin --tags

# 删除一个本地标签；
git tag -d <tagname>

# 删除一个远程标签。
git push origin :refs/tags/<tagname>
```

