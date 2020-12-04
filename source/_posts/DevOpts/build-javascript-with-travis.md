---
layout: deploy
title: biuld javascript with travis
date: 2020-11-30 16:27:47
tags:
---


## 使用.nvmrc 指定nodejs 版本

    若没有在 .travis.yml 文件中指定版本号，可以在仓库根目录下包含 .nvmrc 文件来指定node.js的版本号。[参考](https://github.com/nvm-sh/nvm#nvmrc)

```yml
    language: node_js
    node_js:
      - 7
    cache:
      npm: false
```

# Ember Apps #

  > You can build your Ember applications on Travis CI. The default test framework is Qunit. The following example shows how to build and test against different Ember versions.

```yml
dist: trusty
addons:
  apt:
    sources:
      - google-chrome
    packages:
      - google-chrome-stable
language: node_js
node_js:
  - "7"
env:
    - EMBER_VERSION=default
    - EMBER_VERSION=release
    - EMBER_VERSION=beta
    - EMBER_VERSION=canary
jobs:
  fast_finish: true
  allow_failures:
    - env: EMBER_VERSION=release
    - env: EMBER_VERSION=beta
    - env: EMBER_VERSION=canary

before_install:
    # setting the path for phantom.js 2.0.0
    - export PATH=/usr/local/phantomjs-2.0.0/bin:$PATH
    # starting a GUI to run tests, per https://docs.travis-ci.com/user/gui-and-headless-browsers/#using-xvfb-to-run-tests-that-require-a-gui
    - export DISPLAY=:99.0
    - sh -e /etc/init.d/xvfb start
    - "npm config set spin false"
    - "npm install -g npm@^2"
install:
    - mkdir travis-phantomjs
    - wget https://s3.amazonaws.com/travis-phantomjs/phantomjs-2.0.0-ubuntu-12.04.tar.bz2 -O $PWD/travis-phantomjs/phantomjs-2.0.0-ubuntu-12.04.tar.bz2
    - tar -xvf $PWD/travis-phantomjs/phantomjs-2.0.0-ubuntu-12.04.tar.bz2 -C $PWD/travis-phantomjs
    - export PATH=$PWD/travis-phantomjs:$PATH
    - npm install -g bower
    - npm install
    - bower install
script:
    - ember test --server
```