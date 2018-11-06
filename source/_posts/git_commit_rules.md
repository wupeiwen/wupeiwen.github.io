---
title: Commit 提交规范
date: 2018/9/28 10:00:00
tags:
- git
- 思考
categories: 规范
---

```xml
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

1. type

提交 commit 的类型，包括以下几种

- feat: 新功能
- fix: 修复问题
- docs: 修改文档
- style: 修改代码格式，不影响代码逻辑
- refactor: 重构代码，理论上不影响现有功能
- perf: 提升性能
- test: 增加修改测试用例
- chore: 修改工具相关（包括但不限于文档、代码生成等）
- deps: 升级依赖<!-- more -->

2. scope

修改文件的范围

3. subject

用一句话清楚的描述这次提交做了什么

4. body

补充 subject，适当增加原因、目的等相关因素，也可不写。

5. footer

- **当有非兼容修改(Breaking Change)时必须在这里描述清楚**
- 关联相关 issue，如 `Closes #1, Closes #2, #3`

示例

```
fix($compile): [BREAKING_CHANGE] couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Document change on antvis/g2#12

Closes #392

BREAKING CHANGE:

  Breaks foo.bar api, foo.baz should be used instead
```
