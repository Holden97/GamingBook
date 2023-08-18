---
description: 三人成众
---

# Git

使用e.coding.net账户推送不上去时，有可能是账户凭据不对。

在电脑中打开设置，搜索凭据。

![](<.gitbook/assets/image (9).png>)

## 上传大文件

超过120MB（一说 100MB)无法上传至Git，使用LFS提供对大文件的支持。

## .gitignore失效

在项目开发过程中个，一般都会添加 .gitignore 文件，规则很简单，但有时会发现，规则不生效。\
原因是 .gitignore 只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。\
那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交。

```
git rm -r --cached .

git add .

git commit -m 'update .gitignore'
```

