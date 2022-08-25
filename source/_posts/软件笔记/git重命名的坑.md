如果使用git命令进行仅涉及大小写的重命名,git 默认是把你的动作忽略的，所以当你删掉本地代码，重新pull代码时，你会发现文件还是重命名之前的,神奇吧，记下这个坑，等着你们踩着坑来这看吧，坏笑/

解决方法如下：

 - 设置git库为大小写敏感（不建议）
```
git config core.ignorecase false
```
用这种方法进行重命名，用git status就可以识别出修改了，但是不推荐用这种方式，因为在更新这种修改的时候会有麻烦。

 - 使用git mv命令（仅当core.ignorecase为true时可用）
```
$ git mv ABC.java Abc.java

$ git status
......
renamed:
 ABC.java -> Abc.java

```