# Git

## 从 `svn` 迁移至 `git`

1. Malformed XML: no element found

```
git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata -trunk=trunk
```

报错信息：

```
Error from SVN, (175009): Malformed network data: The XML response contains invalid XML: Malformed XML: no element found
```

2. Dumping stack trace to perl.exe.stackdump

```
git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata
```

报错信息：

```
[main] perl 11008 cygwin_exception::open_stackdumpfile: Dumping stack trace to perl.exe.stackdump
```

**参考**

https://www.jianshu.com/p/7c5caf82fc72

3. --log-window-size=5000000

```
git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata --log-window-size=5000000
```

## git clone 用法

```
git clone -b branch_4x https://github.com/apache/lucene-solr.git ./lucene-solr-4.x
```

或者

```
git clone -b i18n https://github.com/PanJiaChen/vue-element-admin.git ./hs-cloud-app
```

## git archive 用法

1. 获取 fromId 到 endId 版本间修改过的文件，不包含 fromId

```
git archive -o hot-fix-20151001.zip HEAD \$(git diff 05104e3...a0eb9bc --name-only)
```

2. 包含 fromId

```
git archive -o hot-fix-201707181818.zip HEAD \$(git diff ^c2b0b19...bfbd8fe --name-only)
```

3. 导出指定的 commit

```
git archive -o hot-fix.zip 08fc322
```

## git cherry-pick 用法

> 把已经提交的 commit, 从一个分支迁移到另一个分支

```
git cherry-pick c2b0b19
```

或

```
git cherry-pick 1a19304^..9874cd6
```

## git p4merge 安装文档

[https://www.jianshu.com/p/bc059d2846c1](https://www.jianshu.com/p/bc059d2846c1)

[https://www.perforce.com/products/helix-core-apps/merge-diff-tool-p4merge](https://www.perforce.com/products/helix-core-apps/merge-diff-tool-p4merge)

sourcetree 中的配置方法

## git rebase 用法

> git rebase -i HEAD~N

N 是指要压缩的最近的 commit

- 筛选需要合并的 commit

命令输入后会切换到编辑框，vi 命令操作

第一行还是 pick 开头

后面需要的 commit message 前缀改成 s，不需要的前缀改成 d，如下图所示

![rebase-1](./assets/rebase-1.png)

修改完毕后 wq 保存

- 修改 commit message

此时会切换至 commit message 合并页面，依然是 vi 命令，将不需要的 commit message 用 # 注释即可，wq 保存

![rebase-2](./assets/rebase-2.png)

- 强制推送

`git push –f`

由于服务器上已经存在该分支，所以需要强制推送，注意，无法强制推送至受保护的分支

![rebase-3](./assets/rebase-3.png)

_注意：在开发过程中，建议 checkout 新分支进行开发，开发完毕后，merge rquest 给分支负责人进行合并，merge request 的时候勾选 squash 即可由 gitlab 帮我们进行分支合并_

## git commit 规范

- feat: 新功能（feature）
- fix: 修补 bug
- docs: 文档（documentation）
- style: 格式（不影响代码运行的变动）
- refactor: 重构（即不是新增功能，也不是修改 bug 的代码变动）
- test: 增加测试
- chore: 构建过程或辅助工具的变动

**详见**
[https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#type](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#type)

## 解决 git 每次输入密码的问题
每次进行将本地代码提交到远程的时候总会要求输入账号和密码： `git push origin master`，输入账号和密码，明明是对的，却提示登陆失败： `Logon failed, use ctrl+c to cancel basic credential prompt.`

执行
```
git config --system --unset credential.helper
或
git config --global --unset credential.helper
```
参考：[解决git每次输入密码的问题](https://www.dtmao.cc/news_show_664548.shtml)

实际上，github 已经禁止了 auth commit，后续可以统一采用 token 的方式，token 在 github settings -> Developer Settings -> Personal access tokens 中生成 token，然后采用
```
https://your_access_token@github.com/ekoz/skill-summary.git 
```
推送至远端，如果是 gitlab，地址格式为：
```
https://oauth2:your_access_token@gitlab.com/ekoz/skill-summary.git
```