# 代码提交与版本发布规范化

作者：子君

日期：2020年5月13日



## 概念

#### 提交规范

- [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) ：一种提交代码的规范。

- [ Angular commit guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines) 是Angular团队提出的符合 [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) 规范的更具体的规范。

####提交工具

- [commitizen](https://github.com/commitizen/cz-cli) ：一个工具，用于执行一种提交适配器。

- [cz-conventional-changelog](https://github.com/commitizen/cz-conventional-changelog) 一个符合 Angular Conventional Commits 规范的适配器。

- [cz-customizable](https://github.com/leonardoanalista/cz-customizable) : 一个可以自定义的适配器，用于配置属于自己项目的 [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) 规范。

####提交规范校验工具

- [commitlint](https://github.com/marionebl/commitlint) ：一个校验工具，用于校验一个提交插件。在`git commit`前执行，如果校验失败，则停止提交。
- [commitlint/config-conventional](https://www.npmjs.com/package/@commitlint/config-conventional)  是实现 [ Angular commit guidelines](https://github.com/angular/angular.js/blob/master/DEVELOPERS.md#-git-commit-guidelines)  规范的 `commitlint` 插件。





## 根据提交规范进行代码发布

- [semantic-release](https://github.com/semantic-release/semantic-release) ：用于语义化自动发布版本，与上面的提交规范配合使用。





## Conventional Commits 规范

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

- type（必须）： 提交类型
- scope（可选）：该提交的影响范围
- description（必须）：简短的变更描述
- body（可选）：详细的变更描述
- footer（可选）：



##  Angular commit guidelines 规范

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

### type

- feat 新增一个功能
- fix 修复一个Bug
- docs 文档变更 
- style 代码格式（不影响功能，例如空格、分号等格式修正） 
- refactor 代码重构  
- perf 改善性能  
- test 测试  
- build 变更项目构建或外部依赖（例如scopes: webpack、gulp、npm等）  
- ci 更改持续集成软件的配置文件和package中的scripts命令，例如scopes: Travis, Circle等  
- chore 变更构建流程或辅助工具  
- revert 代码回退

### scope（可选）

自定义本次提交影响的范围

### subject

简要描述改动的主题：

- 使用现在时  "change" ，不要使用 "changed" 、"changes"
- 首字母不要大写
- 默认不要添加符号 (.)

###body

详细描述改动的信息：

- 增加本次改动的动机。
- 并且增加本次改动与之前的对比。

### footer

- BREAKING CHANGE 
  - 出现不兼容上一版本的修改，应该添加此描述，[详细](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.gbbngquhe0qa)
- ISSUES CLOSED
  - 如果本次提交需要关闭github上的ISSUES，则应该添加本项。[详细](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#heading=h.uh9jqqu4fjy3)

```
fix(admin): dsfj代付款

打开了是非得失浪费

BREAKING CHANGE:
的康师傅就是

ISSUES CLOSED: #31,#23
```





## 参考文章

- [Cz工具集使用介绍 - 规范Git提交说明](https://juejin.im/post/5cc4694a6fb9a03238106eb9)
- [使用文章](https://segmentfault.com/a/1190000021132745)

