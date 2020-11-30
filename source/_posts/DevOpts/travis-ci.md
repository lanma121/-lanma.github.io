---
title: travis ci
date: 2020-11-30 16:04:26
tags:
---

# What? 

  > Travis CI 提供的是持续集成服务（Continuous Integration，简称 CI）。它绑定 Github 上面的项目，只要有新的代码，就会自动抓取。然后，提供一个运行环境，执行测试，完成构建，还能部署到服务器。

# Why?

  > 持续集成的好处在于，只要代码有变更，就自动运行构建和测试，反馈运行结果。从而不断累积小的变更，而不是在开发周期结束时，一下子合并一大块代码。

# Notes: 

  > Travis CI 只支持 Github，不支持其他代码托管服务。

# .travis.yml

    该文件必须保存在 Github 仓库项目的根目录里面，一旦代码仓库有新的 Commit，Travis 就会去找这个文件，执行里面的命令。

  ```yml
    sudo: required # 需要sudo权限
    language: node_js # 指定了默认运行环境
    node_js:
      - 10 # use nodejs v10 LTS
    before_install: sudo npm install requirejs # 在安装依赖之前需要安装foo模块
    cache: npm
    branches:
      only:
        - master # build master branch only
    script: # 指定要运行的脚本，script: true表示不执行任何脚本，状态直接设为成功
      - hexo generate # generate static files
    deploy:
      provider: pages
      skip-cleanup: true
      github-token: 983cda7ff384d0514e7f44bcf18c8c6b2e808ab1
      keep-history: true
      on:
        branch: master
      local-dir: public
  ```

