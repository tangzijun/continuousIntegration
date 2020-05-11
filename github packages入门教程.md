# Github packages 入门

作者：子君

日期：2020年5月11日



[Github packages](https://github.com/features/packages)（包管理器） 是github持续集成生态中的一个环节，打通了团队协作中代码共享的重要一环。github[收购](https://github.blog/2020-04-15-npm-has-joined-github/)npm后，npm依然会承担开源库托管的重任，Github packages则会承担起组织和团队协作中私有库共享的责任。



## 一、包管理器的基本概念

包管理器由一个组织开发及维护，作用是让全球的开发人员可以轻松的共享自己制作好的通用代码库，并让其他开发人员可以轻松的安装到自己的项目中使用。

要达到这个目的，就需要有一个服务器存放这些代码库以及代码库的信息，这个服务被称为`registry（注册表）` ， 然后需要一个工具让开发人员可以发布和安装代码库，这个工具被制作成`CLI（命令行工具）` ，然后这个组织还会制作一个`网站`，用于可视化搜索代码库，查看库的信息。这就是一个包管理器所需要的3个部分：

##### 包管理器的3个组成部分：

- **registry** （注册表）：就是一个数据库服务，用于记录所有包的信息。
- **CLI** （命令行工具）：用于安装包、发布包，和其他管理功能。
- **网站**：用于可视化搜索包，查看包的信息。

##### 最常见的包管理器：

- **npm**：[CLI](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)    [网站](https://www.npmjs.com)    [registry](http://registry.npmjs.org)
- **yarn**:  [CLI](https://classic.yarnpkg.com/en/docs/install#mac-stable)     [网站](https://classic.yarnpkg.com)    [registry](https://registry.yarnpkg.com)



## 二、包管理器的使用场景

### 场景一

我们一般接触的都是`public package`（公共包） , 只需要通过简单命令，就可以安装到自己的项目中开始使用，如下两种：

```
npm install react
npm install @testing-library/react
```

不带`@`符号的称为 `unscoped public packages` 无范围的公共包。

带有`@`符号的被称为`scoped public packages`  有范围的公共包。

带@范围的包一般是由一个组织维护的一系列的包，比如这3个组织 `@types` `@rollup` `@babel`，分别维护了[typescript的类型库](https://www.npmjs.com/org/types)、[rollup打包器相关的插件](https://www.npmjs.com/org/rollup) 、[babel编译器相关的插件](https://www.npmjs.com/org/babel)。带@范围的目的是为了明确标识这是由某个组织发布的包。



### 场景二

一些企业和组织需要有自己的私有库，不完全公开，只针对企业和组织内部使用。这类库被称为`private package`（私有包）。npm上支持收费的个人账户和组织账号[发布私有包](https://docs.npmjs.com/creating-and-publishing-private-packages)。



### 结论

以上两种场景说明至少可以发布3种类型的包：

1. 发布`unscoped public packages`（无范围的公共包）[npm文档](https://docs.npmjs.com/creating-and-publishing-unscoped-public-packages) 

   注册免费的npm个人账号即可发布。

2. 发布`scoped public packages`（有范围的公共包） [npm文档](https://docs.npmjs.com/creating-and-publishing-scoped-public-packages)

   需要注册付费的 `npm Org`（npm组织）账号。

3. 发布`private package`（私有包） [npm文档](https://docs.npmjs.com/creating-and-publishing-private-packages)

   需要注册付费`npm user` (个人账号)  或 付费的`npm Org`（npm组织）账号。



## 二、发布包到个人的github

### 特性：

- `github package ` 拥有自己的 `registry` 地址：https://npm.pkg.github.com ，发布前需要登录。
- 在github上发布包，必须要有一个github仓库，因为包发布时需要绑定在一个仓库上。
- 仓库是可见状态就决定了package是 `public package` 还是 `private package`。
- 这个package的 `scoped`(范围) 就是这个仓库的 owner（所有者）。
- 所以在github上可以发包的类型是有： `scoped public packages` (有范围的公共包) , `private package` （私有包），不能发布`unscoped public packages`（无范围的公共包）。



### 步骤：

#### 1、登录 github packages registry（github包注册表）

使用以下命令行，通过npm登录到github的registry，登录时不能直接使用github的密码登录，而是需要使用github的Personal access tokens（个人访问令牌），[查看创建个人访问令牌的方法](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) ，创建成功后你将获得一串40位的秘钥字符串，就是以下登录需要的`TOKEN`。

```shell
npm login --registry=https://npm.pkg.github.com
> Username: USERNAME   //github的账号名
> Password: TOKEN			 //个人访问令牌	
> Email: PUBLIC-EMAIL-ADDRESS   //公开邮箱
```

#### 2、创建项目并提交到github仓库

创建一个项目，并将代码提交到github仓库，这个步骤是必须的，因为在github上发布package必须绑定一个仓库。

```
mkdir test-project      # 创建一个文件夹
cd test-project					# 进入该文件夹
npm init || yarn init 	# 初始化项目，npm 或 yarn都可以
```

#### 3、添加npm配置文件 `.npmrc`

在项目的根目录创建一个文件名为`.npmrc`，内容如下，并将其中的`USERNAME` 替换为第一步中登录时的账号名。

```
@USERNAME:registry=https://npm.pkg.github.com
```

#### 4、修改项目的 `packages.json`文件

1. 修改`name`字段，如下，并将`USERNAME`替换为github的账号名（同上）,替换`PACKAGENAME`为你要发布包的名称。

   说明：`@USERNAME`是指发布包的scope（范围），配合上一步骤中 `.npmrc`的配置，意思是该项目的包将发布到github的注册表。

   ```
   "name": "@USERNAME/PACKAGENAME",
   ```

2. 添加`repository`字段，如下，并将`REPOURL`替换为github仓库的地址。

   说明：该设置用于说明项目在github上的仓库地址，原因是每个包都需要绑定到一个仓库，发布时会检测该仓库是否真实存在，并且你是否有权限发布包到这个仓库。

   ```
   "repository": {
      "type": "git",
      "url": REPOURL
   },
   ```

#### 5、发布

发布前，再次提交一次代码到仓库。

```
npm publish
```

通过以上步骤操作，即可将一个项目生成一个package，并发布到github上去。并绑定到你的github仓库。

如果出现错误，参见后面的【常见错误】一栏。



## 三、在其他项目中安装包

#### 1、登录 github packages registry

方式同发布一致。

#### 2、添加npm配置文件 `.npmrc`

方式同发布一致。

#### 3、安装

```
yarn add "@USERNAME/PACKAGENAME" 
```





## 五、参考链接

- [github packages官网](https://github.com/features/packages)
- [github packages 说明文档](https://help.github.com/en/packages/publishing-and-managing-packages)
- [npm文档](https://docs.npmjs.com/)
- [使用npm发布和安装github packages](https://help.github.com/en/packages/using-github-packages-with-your-projects-ecosystem/configuring-npm-for-use-with-github-packages)





## 常见问题

### github用户名在哪里?

![image-20200508165301996](/Users/tangzijun/Library/Application Support/typora-user-images/image-20200508165301996.png)





## 常见错误

### 404 Not Found

The expected resource was not found，详细错误如下：

```
npm ERR! code E404
npm ERR! 404 Not Found - PUT https://npm.pkg.github.com/@tangzijun%2fiffe-setting - The expected resource was not found.
npm ERR! 404 
npm ERR! 404  '@tangzijun/iffe-setting@0.2.0' is not in the npm registry.
npm ERR! 404 You should bug the author to publish it (or use the name yourself!)
npm ERR! 404 ...
```



**原因1**：

- 每一个github packages都需要关联一个github repository（git仓库）

- `npm publish`时，会检测当前项目`packages.json`文件中是否包含 repository 字段。

  ```
  "repository": "https://github.com/tangzijun/xxxxxx.git",
  ```

- `repository` 字段的值必须是你github账户下已存在的一个仓库地址。

- 如果以上验证正确，则会将这个包发布这个仓库中，并关联。（如下图）。

  ![image-20200509152652539](/Users/tangzijun/Library/Application Support/typora-user-images/image-20200509152652539.png)



**原因2：**

- 项目`packages.json` 文件中的 name 验证错误。

- name字段必须带有作用域范围，也就是该包必须属于github的某个用户或组织。

  ```
  // 错误
  "name": "iffe-setting",  
  
  //正确，@tangzijun 代表github账户名
  "name": "@tangzijun/iffe-setting", 
  ```

  



### 400 Bad Request

failed to stream package from json: unhandled input: Cannot publish over existing version.

```error log
npm ERR! code E400
npm ERR! 400 Bad Request - PUT https://npm.pkg.github.com/@tangzijun%2fiffe-setting3 - failed to stream package from json: unhandled input: Cannot publish over existing version.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/tangzijun/.npm/_logs/2020-05-09T07_27_38_297Z-debug.log
```

**原因：**

- `npm publish`时，会检测项目`packages.json`文件中的`version`字段是否与已发布的版本一致，如果一致则会报错。

- 需要将`version`字段进行修改，例如：

  ```
  - "version": "0.2.1",
  + "version": "0.2.2",
  ```









