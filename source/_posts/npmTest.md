---
title: npmTest
date: 2021-12-28 15:08:14
tags: node
---

# 前言

###### 本地多个包相互依赖，如何去本地npm安装并测试呢？

# 项目结构

```
npmTest
├─ .DS_Store
├─ a
│  ├─ .DS_Store
│  ├─ index.mjs
│  ├─ package-lock.json
│  └─ package.json
├─ b
│  ├─ .DS_Store
│  ├─ index.mjs
│  └─ package.json
└─ c
   ├─ index.mjs
   └─ package.json

```

### 假设有三个package
a项目依赖b项目，b项目依赖c项目
安装的时候只在a项目下安装依赖，即npm install --save ./b

a -> package.json 如下：
```json
  {
  "name": "b",
  "version": "2.0.0",
  "description": "",
  "main": "index.mjs",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "c": "file:../c"
  }
}
```

a -> index.mjs 如下：
```js
  import c from 'c';

  export default c;
```

a -> node_modules 目录结构 如下：
```
node_modules
├─ b
│  ├─ node_modules 
│     ├─ c
|        ├─ index.mjs
|        ├─ package.json
```

a项目node_modules里面b依赖包会与 ./b 建立软连接，方便开发在本地调试。

### 查看npm解析安装依赖包详细情况
> npm install --verbose  (打印详细信息)
> npm install --loglevel silly (答应更加具体的解析安装详情)
> npm install --loglevel silly 2>&1 | tee log.txt (输出安装依赖信息到log.txt文件中 2>&1 输出成单个流; tee 读取标准输入到数据，并将内容输出成文件)

```zsh
npm info it worked if it ends with ok
npm verb cli [
npm verb cli   '/usr/local/bin/node',
npm verb cli   '/usr/local/bin/npm',
npm verb cli   'install',
npm verb cli   '--loglevel',
npm verb cli   'silly'
npm verb cli ]
npm info using npm@6.14.15
npm info using node@v14.16.0
npm verb npm-session 85e124dc20badd09
npm sill install runPreinstallTopLevelLifecycles
npm sill preinstall a@1.0.0
npm info lifecycle a@1.0.0~preinstall: a@1.0.0
npm sill install loadCurrentTree
npm sill install readLocalPackageData
npm timing stage:loadCurrentTree Completed in 9ms
npm sill install loadIdealTree
npm sill install cloneCurrentTreeToIdealTree
npm timing stage:loadIdealTree:cloneCurrentTree Completed in 1ms
npm sill install loadShrinkwrap
npm timing stage:loadIdealTree:loadShrinkwrap Completed in 6ms
npm sill install loadAllDepsIntoIdealTree
npm timing stage:loadIdealTree:loadAllDepsIntoIdealTree Completed in 3ms
npm timing stage:loadIdealTree Completed in 11ms
npm sill currentTree a@1.0.0
npm sill currentTree └─┬ b@2.0.0
npm sill currentTree   └── c@1.0.0
npm sill idealTree a@1.0.0
npm sill idealTree └─┬ b@2.0.0
npm sill idealTree   └── c@1.0.0
npm sill install generateActionsToTake
npm timing stage:generateActionsToTake Completed in 2ms
npm sill diffTrees action count 0
npm sill decomposeActions action count 0
npm sill install executeActions
npm sill doSerial global-install 0
npm verb correctMkdir /Users/xf/.npm/_locks correctMkdir not in flight; initializing
npm verb lock using /Users/xf/.npm/_locks/staging-cc44cd3a0fd01acc.lock for /Users/xf/Documents/Test/test/js/npmTest/a/node_modules/.staging
npm sill doParallel extract 0
npm sill doReverseSerial unbuild 0
npm sill doSerial remove 0
npm sill doSerial move 0
npm sill doSerial finalize 0
npm sill doParallel refresh-package-json 0
npm sill doParallel preinstall 0
npm sill doSerial build 0
npm sill doSerial global-link 0
npm sill doParallel update-linked 0
npm sill doSerial install 0
npm sill doSerial postinstall 0
npm verb unlock done using /Users/xf/.npm/_locks/staging-cc44cd3a0fd01acc.lock for /Users/xf/Documents/Test/test/js/npmTest/a/node_modules/.staging
npm timing stage:executeActions Completed in 29ms
npm timing stage:rollbackFailedOptional Completed in 0ms
npm sill install runPostinstallTopLevelLifecycles
npm sill build a@1.0.0
npm info linkStuff a@1.0.0
npm sill linkStuff a@1.0.0 has /Users/xf/Documents/Test/test/js/npmTest as its parent node_modules
npm sill install a@1.0.0
npm info lifecycle a@1.0.0~install: a@1.0.0
npm sill postinstall a@1.0.0
npm info lifecycle a@1.0.0~postinstall: a@1.0.0
npm sill prepublish a@1.0.0
npm info lifecycle a@1.0.0~prepublish: a@1.0.0
npm info lifecycle a@1.0.0~prepare: a@1.0.0
npm timing stage:runTopLevelLifecycles Completed in 81ms
npm sill saveTree a@1.0.0       // 依赖的结构
npm sill saveTree └─┬ b@2.0.0
npm sill saveTree   └── c@1.0.0
npm sill install saveToDependencies
npm verb saving []
npm verb shrinkwrap skipping write for package.json because there were no changes.
npm info lifecycle undefined~preshrinkwrap: undefined
npm info lifecycle a@1.0.0~shrinkwrap: a@1.0.0
npm verb shrinkwrap skipping write for package-lock.json because there were no changes.
npm info lifecycle a@1.0.0~postshrinkwrap: a@1.0.0
npm WARN a@1.0.0 No description
npm WARN a@1.0.0 No repository field.

npm sill install printInstalled
npm timing audit submit Completed in 511ms
npm http fetch POST 200 http://r.tnpm.oa.com/-/npm/v1/security/audits/quick 512ms
npm timing audit body Completed in 2ms
audited 2 packages in 0.565s
found 0 vulnerabilities

npm verb exit [ 0, true ]
npm timing npm Completed in 906ms
npm info ok 

```