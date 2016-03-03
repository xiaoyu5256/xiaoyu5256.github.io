title: svn之服务器镜像同步
date: 2016-03-02 18:46:40
categories:
- 开发工具
- svn
tags:
- svn
- 开发工具
- 版本管理
---

如果同步，需要在服务器上，如果是备份，则在安装svn客服端的机器即可。
1.建立新项目
```
svnadmin create svn_repos
```

1.执行(如果windows，需要将`pre-revprop-change`改成`pre-revprop-change.bat`)
```
cp /var/svndata/svn_repos/hooks/pre-revprop-change.tmpl /var/svndata/svn_repos/hooks/pre-revprop-change
chmod a+x /var/svndata/svn_repos/hooks/pre-revprop-change
svnsync init file:///var/svndata/svn_repos https://showmecode.cn/code/svn_repos  --sync-username user_svnsync --sync-password svnsync
svnsync sync --non-interactive file:///var/svndata/svn_repos

```
>参考
[window下给svn域做镜像](https://iacoware.wordpress.com/2009/10/02/howto-use-svnsync-to-mirror-a-repository-on-windows/)
[使用svnsync实现已有版本库的镜像](http://zengrong.net/post/1598.htm)
