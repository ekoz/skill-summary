========================
工作中的技能经验总结
========================

Git
========================

从 `svn` 迁移至 `git` 
---------------------
1. `git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata -trunk=trunk`

Error from SVN, (175009): Malformed network data: The XML response contains invalid XML: Malformed XML: no element found


2. `git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata` 

0 [main] perl 11008 cygwin_exception::open_stackdumpfile: Dumping stack trace to perl.exe.stackdump

**参考**
https://www.jianshu.com/p/7c5caf82fc72


3. git svn clone http://172.16.1.244/svn/xmas/src/kbase/kbase-workflow/trunk1.1 -r 20354:HEAD --no-metadata --log-window-size=5000000

git clone 用法
---------------------
`git clone -b branch_4x https://github.com/apache/lucene-solr.git ./lucene-solr-4.x`

`git clone -b i18n https://github.com/PanJiaChen/vue-element-admin.git ./hs-cloud-app`

git archive 用法
---------------------
1. `git archive -o hot-fix-20151001.zip HEAD $(git diff 05104e3...a0eb9bc --name-only)`

2. 获取 fromId 到 endId 版本间修改过的文件，不包含fromId 

  git archive -o hot-fix-201707181818.zip HEAD $(git diff c2b0b19...bfbd8fe --name-only)


3. 导出指定的 commit 

  git archive -o hot-fix.zip 08fc322

git cherry-pick 用法
---------------------
  把已经提交的commit, 从一个分支迁移到另一个分支

`git cherry-pick c2b0b19`

`git cherry-pick 1a19304^..9874cd6`

git p4merge 安装文档
---------------------
https://www.jianshu.com/p/bc059d2846c1

https://www.perforce.com/products/helix-core-apps/merge-diff-tool-p4merge

git rebase 用法
----------------
`git rebase -i HEAD~N`

1. i键入

  pick XXX xxx

  需要改成s XXX xxx
  
  不需要改成d XXX xxx
  
  wq

2. i键入

  修改Commit
  
  wq

3. 推送

git commit 规范
---------------------
- feat: 新功能（feature）
- fix: 修补bug
- docs: 文档（documentation）
- style:  格式（不影响代码运行的变动）
- refactor: 重构（即不是新增功能，也不是修改bug的代码变动）
- test: 增加测试
- chore: 构建过程或辅助工具的变动

**详见**
https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#type

License
-------

The project is licensed under the MIT license.
