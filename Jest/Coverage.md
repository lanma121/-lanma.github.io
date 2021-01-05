
---
title: Jest Coverage
date: 2021-01-05
tags: test
description: "jest coverage for branch, statement, lines and functions"
---

代码覆盖率是有用的，但不要将其作为衡量单元测试的唯一指标。因为一些边缘情况和不同的场景，代码覆盖率并不能做到。
默认情况下，Jest将为每个有测试的文件（以及它们正在导入的任何文件）计算覆盖率。

## Branch coverage
分支覆盖率是指否执行了每个控制结构（如if和case语句）的每个分支，而不是期望的结果是什么，即路径测试。只要每条路径都被执行，它就会被覆盖。例如，给定一个if语句，是否同时执行了true和false分支？换句话说，程序中的每一条边都被执行了吗？”    
### 什么语句会引起branch test？
1. 条件语句（if/switch）
2. 循环语句（for/while...）
3. `break` 或者 `continue` 语句
4.  `&`或者`||`运算符
5. 默认参数`function A(a = []) { ... }`

## Stmts coverage
每条语句是否被执行

## Lines coverage
每行是否被执行
## Funcs coverage
每个函数是否被调用
