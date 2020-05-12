

# Github actions 入门教程

作者：子君

日期：2020年5月11日



## github持续集成记录

> 持续集成 (CI) 是一种需要频繁提交代码到共享仓库的软件实践。 频繁提交代码能较早检测到错误，减少在查找错误来源时开发者需要调试的代码量。 频繁的代码更新也更便于从软件开发团队的不同成员合并更改。 这对开发者非常有益，他们可以将更多时间用于编写代码，而减少在调试错误或解决合并冲突上所花的时间。



- [Github actions商店](https://github.com/marketplace?type=actions)



## 持续集成概念

持续集成发生在代码提交到仓库时：

持续集成步骤：

1. 代码测试，包括
   1. 语法检查：eslint
   2. 语法样式检查: eslint + prettier
   3. 安全性检测
   4. 代码覆盖率
   5. 功能测试
   6. 其他自定义
   7. 邮件通知
   8. 定时执行



## Github actions

- 持续集成Continuous integration (CI) 
- 持续部署Continuous deployment (CD) 
- [使用github actions发布软件包](https://help.github.com/cn/actions/publishing-packages-with-github-actions)



## Github packages

- [文档](https://help.github.com/en/packages/publishing-and-managing-packages/about-github-packages)



## 一些写好的actions

- [发布到github pages](https://github.com/marketplace/actions/deploy-to-github-pages)
- 





## 创建工作流

- 在项目根目录创建目录 `.github/workflows`

- 在目录中创建 `.yml` 或者 `.yaml` ， 例如 `ci.yml`

- 工作流文件内容示例：

  ```
  # 以下工作流程在仓库的master分支提交代码时运行
  # 运行环境是 ubuntu-latest
  # 第一步：获得仓库代码
  # 第二步：运行项目，并安装依赖。
  # 第三步：运行项目中定义好的script进行代码检查
  # 第四步：编译项目
  
  name: ci
  on:
    push:
      branches:
        - master
  jobs:
    build-and-deploy:
      runs-on: ubuntu-latest
      steps:
        # 获得仓库代码
        - name: checkout
          uses: actions/checkout@v2
        # 安装依赖
        - name: install dependencies
          run: yarn
        # 代码检查
        - name: code check
          run: yarn lint
        # 编译
        - name: buld
          run: yarn build
  ```

  - name : 工作流的名称
  - on： 工作流触发运行的条件，3类：
    - [git事件触发](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#onpushpull_requestbranchestags)，例如 `git push` 提交了代码。
    - [webhook事件触发](https://help.github.com/en/actions/reference/events-that-trigger-workflows#webhook-events)
    - [定时触发](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#onschedule) ，例如每天的晚上12点。
  - runs-on ：运行工作流的虚拟环境，2类：
    - [使用github提供的托管环境](https://help.github.com/en/actions/reference/virtual-environments-for-github-hosted-runners#supported-runners-and-hardware-resources)
    - [使用自己搭建的环境](https://help.github.com/en/actions/hosting-your-own-runners/using-self-hosted-runners-in-a-workflow)
  - uses ：引用动作，可以引用3种
    - [使用公共存储库的action](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#referencing-an-action-from-a-public-repository)（常用）
    - [使用自己写好的独立action](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#referencing-an-action-in-the-same-repository-where-a-workflow-file-uses-the-action) （常用）
    - [使用docker hub中的docker定义好的action](https://help.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#referencing-a-container-on-docker-hub)
  - strategy : 如果需要跨平台运行工作流，可以设置[构建矩阵](https://help.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idstrategy) ， 例如你的代码要再多种node环境下测试[setup-node](https://github.com/actions/setup-node)
  - run ： 自定义运行的脚本，例如:  `yarn && yarn build`
  - with : 设置输入变量，必须与uses一起使用
  - env : 设置环境变量，[默认环境变量](https://help.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables#default-environment-variables)



## 在工作流中使用变量和秘钥

创建secrets：

- 个人的仓库，必须是仓库的拥有者才能创建secrets
- 组织的仓库 ，需要有 `admin` 权限才能创建secrets
- 使用REST API创建secrets，需要有该仓库的读写权限，[详细](https://developer.github.com/v3/actions/secrets/#create-or-update-a-secret-for-a-repository)



环境变量

- [默认环境变量](https://help.github.com/en/actions/configuring-and-managing-workflows/using-environment-variables#default-environment-variables)





## 缓存工作流数据

- [artifacts](https://help.github.com/en/actions/configuring-and-managing-workflows/persisting-workflow-data-using-artifacts) ： 工件可让您在工作流程运行做成功的jobs之间共享数据，并在工作流程完成后存储数据。
  - `actions/upload-artifact` [仓库](https://github.com/actions/upload-artifact)
  - `actions/download-artifact` [仓库](https://github.com/actions/download-artifact)



## 缓存依赖项加快工作流程

- `actions/cache` [仓库](https://github.com/actions/cache)



## nodejs 和 javascript

[文档](https://help.github.com/en/actions/language-and-framework-guides/using-nodejs-with-github-actions)



## 参考文章

- [Martin Fowler-Continuous Integration](https://www.martinfowler.com/articles/continuousIntegration.html)
- [知乎-如何理解持续集成](https://www.zhihu.com/question/23444990)
- [阮一峰-持续集成是什么](http://www.ruanyifeng.com/blog/2015/09/continuous-integration.html)
- [阮一峰-github actions入门](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

- [github-持续集成方案](https://help.github.com/cn/actions/building-and-testing-code-with-continuous-integration)
- [使用github token进行身份验证](https://help.github.com/en/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)
- [Github action 核心概念](https://help.github.com/en/actions/getting-started-with-github-actions/core-concepts-for-github-actions)

- [Github actions marketplace（市场）](https://github.com/marketplace/actions/)

