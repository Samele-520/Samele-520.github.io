---
title: node的npm
tags: node npm
---

## npm基本指令了解

1. `npm init` OR  `npm init -y`
    - 所处目录必须是英文，不能使用中文，不能出现空格
    - 初始化包
2. `npm config ls`
    - 查看`npm`配置信息
    - `npm config -l ls` 更详细信息
3. `npm list`
    - 查看已经安装的包 -> 树形结构展开
4. `npm list -g --depth 0`
    - 查看全局安装了哪些包
5. `npm view 包名 versions`
    - 查看对应包的所有版本号
      - 无前缀的表示就是指定版本
      - 前缀是`~`表示锁定主版本号和次版本号
      - 前缀是`^`表示锁定主版本号
      - `*`表示最新版本
6. `npm outdated`
    - 显示已安装包的版本信息
    - Package - 包名
    - Current - 当前版本
    - Wanted - 我想要的 -- 配置里的配置最新包
    - Latest - 最新版本
    - Location - 位置
    - package.json 文件下包名前缀
        - ^ : 锁定主版本
        - ~ ：锁定住版本号和副版本号
        - （什么都没有）：锁定当前版本
        - \* : 最新包
7. `npm install --production`
    - 只下载生产环境的包
8. `npm install`
    - 运行`npm install`命令后，包管理工具会先读取`package.json`中的`dependencies`节点，读取到记录的所有依赖包名称和版本号，然后将其进行下载
9. `npm install 包名`
    - 核心依赖包
    - 将所要安装的包记录到`package.json`的`dependencies`节点下
    - 当安装时，提供`-g`参数，则会把包安装为全局包
      - 全局包会被默认安装到`c:\users\用户目录\appdata\roaming\npm\node_modules`目录下
      - 只有**工具性质的包**才有全局安装的必要性 -- 它们可以提供了好用的终端命令
      - 可以参考官方文档提供的使用说明来判断是否需要全局安装包
10. `npm install 包名@指定版本`
    - 下载`@`符后面跟随的包版本
11. `npm install -D 包名` OR `npm install --save-dev 包名`
    - 开发依赖包
    - 将所要安装的包记录到`package.json`的`devDependencies`节点下
12. `npm update 包名`
    - 更新包
13. `npm uninstall 包名`
    - 卸载包
    - 卸载成功后，会把包从`package.json`的`dependencies`中移除掉，并且把对应的包从`node_modules`中删除
    - 添加`-g`则为卸载全局包
14. `npm config get registry`
    - 查看电脑已经配置的源
15. `npm config set registry 源地址`
    - 设置源 - 新增源
    - 取消npm代理设置

      ```node
      // 查看目前自己的代理
      npm config get proxy
      npm config get npm config get https-proxy
      // 重置代理 - 置空
      npm config set proxy false
      npm cache clean --force
      OR
      npm config set proxy null
      npm config set https-proxy null
      ```

    - 添加淘宝镜像 `npm --registry https://registry.npm.taobao.org info underscore`
    - 设置官方源 `npm config set registry https://registry.npmjs.org/`

16. `cls`
    - 清空终端
17. `npm cache clean --force`
    - 清除缓存 -> 可解决拉包拉错了，之后使用一直报错（另一种解决方法是删除node_modules依赖包重新安装） -- 相较省事
18. `npx webpack`
    - 直接进行`webpack`打包 - 配置后才可用
    - 无配置，直接可以使用
    - 可以自动去项目中的`node-modules/bin`中查找可执行文件，若没有，会临时下载一个  

## node 本地服务器

1. 全局安装 `npm install serve -g`
2. 进入对应源码同级文件夹，执行`serve dist -p 5000`
    - **dist**: 源码所在文件夹名
    - **5000**: 开启的端口号
3. 或者进入源码文件下直接执行`serve -p 开启的端口号` or `serve` - 后者直接打开默认端口

## nvm版本管理

1. windows下获取nvm
    - 进入[nvm-windows](https://github.com/coreybutler/nvm-windows/releases)官网
    - 选择**nvm-setup.zip**下载并安装
    - 输入`nvm -v`命令验证是否安装成功
2. `nvm ls`
    - 列出所有已经安装的node版本
3. `nvm install version`
    - 安装指定版本号的node
    - `version`为版本号
    - 示例：`nvm install v16.14.2` - 安装对应版本
4. `nvm use version`
    - 全局切换node的版本
    - `version`为版本号
    - 若执行结果是`exit status 1:后面跟一堆乱码`表示没有权限，可以使用管理员身份再次运行试试
    - 示例：`nvm use v16.14.2 | 14.19.3` - 切换到对应版本
5. `nvm uninstall version`
    - 卸载版本对应版本（卸载前，请切到其他版本，否则卸不掉）
    - `version`为版本号
    - 示例：`nvm uninstall v16.14.2` - 切换到对应版本
6. `nvm current`
    - 获取当前node版本

  > 注意： 使用nvm安装的node是不具备npm的，所以执行npm指令会报错，若想用npm需要自己去官网下载一下[node](https://nodejs.org/zh-cn/download/releases)，然后进行安装，这样就可以使用node的npm了

  > 解决`nvm`安装`node`没有`npm`的问题
  >
  > 1. 命令行运行：`nvm root` 显示出`nvm`的安装目录
  > 2. 打开`nvm`文件夹下的`settings.txt`文件，在最后追加加以下代码：
  > 3. 然后再安装对应`node`,可以在对应的`node_modules`中看到`npm`包

  ```node
    node_mirror: https://npm.taobao.org/mirrors/node/
    npm_mirror: https://npm.taobao.org/mirrors/npm/
  ```

## 项目依赖检测工具

- 当项目因为依赖而报错时可以使用此工具获取哪些依赖缺失，也可用于检测哪些依赖没有使用到
- 安装：
  1. 全局安装

      ```sh
      npm i -g depcheck
      ```

  2. 检测依赖使用情况

      ```sh
      depcheck
      ```

  3. 检测结果：
     - Missing dependencies：缺失依赖及对应项目使用位置
     - Unused dependencies：安装但未使用的依赖
     - Unused devDependencies：伪已安装未使用（其实已经使用了）
       - 针对解决：
        1. 在根目录下新建`.depcheckrc`文件

            ```js
            // .depcheckrc文件下
            ignores: ["less", "less-loader"]
            ```

## nrm源管理

1. 安装
  
    ```node
      npm install -g nrm
    ```

2. 添加源
    - 格式：`nrm add xx (xx代表名字)`
    - 案例：`nrm add mr https://registry.nodejitsu.com/`
3. 查询已有源
    - `nrm ls`
4. 切换源
    - `nrm use xx`
5. 删除源
    - `nrm del xx`
6. 测试源的相应响应时间（速度）
    - `nrm test`

## 包结构

- 规范包的必须结构
  1. 包必须以**单独的目录**而存在
  2. 包的顶级目录下，必须包含`package.json`包管理配置文件
  3. `package.json`中必须包含name(包名)、version(版本号)、main(包的入口)属性
- 包的基本结构 - 文件名即包名
  - `package.json` - 包管理配置文件
  - `index.js` - 包入口文件
  - `README.md` - 包说明文档

## Package.json文件了解

- name ： 包名(文件夹的名字) -- 包名不可与其他包(线上包)名重复
- version ： 版本号(1.0.0)
  - 第一位数字是大版本
  - 第二位数字是功能版本
  - 第三位数字是bug修复版本
- main ： 对外暴露的文件 -> 当别人引用这个包时，对他暴露的文件 - 包的入口文件
- descriptin ： 对包的简单描述
- keywords ： 此包的关键字(别人搜索什么可以搜索到你) - 数组类型
- scripts ： npm脚本集 -- 用一个键代替很长一段npm指令
  - echo -- 打印 -- 输出echo后面跟随的字符串
  - npm run 键 -- 运行scripts里边写的键对应的值
    - start、test -- 可以省略run直接运行
    - 示例 -- "start" : "./a/b.js" -- npm start
    - 简洁运行指令 "runjs" : "./a/b.js" -- npm run runjs
    - 多条并行执行 "runjs": "./a/b.js & ./c/d.js"
    - 多条串行执行 "runjs": "./a/b.js && ./c/d.js"
- author ： 作者
- entry point：（index.js）：包的主文件/入口地址
- test command：包的测试指令
- git repository：git的厂库地址
- license：（ISC）这个包遵循什么样的开源许可协议
- dependencies：开发和上线后都会使用的包
- devDependencies：开发阶段使用的依赖包 - 项目上线后便不会使用了

## 发布自己的包

1. 注册一个[npm账号](https://www.npmjs.com/)
2. `npm adduser`
   - 登录 `npm login`
      - 用户名
      - 密码
      - 邮箱
      - 源 --> 不能是淘宝源或其他源，得是npm的源 `https://registry.npmjs.org`
3. `npm publish`
    - 发布
4. 删除云端的上传包：npm unpublish（会出现删除提醒）
5. 强制删除云端的上传包：npm --force unpublish（此时便真删除了包）
    1. 删除本地包对应的版本号的那一版本
    2. 最低版删不了
    3. 每次只能删除一个版本
    4. 每次删除都要改本地的对应版本号

> 同一个包，每次上传需要改文件下的版本号，以做区分;