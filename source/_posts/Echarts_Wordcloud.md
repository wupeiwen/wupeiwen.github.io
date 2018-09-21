---
title: echarts-wordcloud 兼容性测试
date: 2018/5/24 14:53:00
tags:
- 数据可视化 
- echarts
categories: 数据可视化
---

Echarts是业内一流的可视化组件库, 提供方便丰富的可视化图表. echarts图表中的词云属于扩展插件. 使用前需要引入echarts和echarts-wordcloud两个依赖. 实际使用中echarts-wordcloud对echarts有版本要求, 官方对这一点没有明确说明. 本文就两者的兼容性进行测试并得出相应结论.<!-- more -->

### npm中的依赖要求
------
echarts-wordcloud依赖项的版本要求（数据来自于npm package.json dependencies）
| 依赖项 | echarts-wordcloud1.0.0 | echarts-wordcloud1.1.0 | echarts-wordcloud1.1.1 | echarts-wordcloud1.1.2 | echarts-wordcloud1.1.3 |
| ------ | ------ | ------ | ------ |
| echarts | ^3.2.2 | ^3.6.2 | ^3.7.0 | ^3.7.0 | 无 |
| zrender | ^3.1.2 | 无 | 无 | 无 | 无 |

### 测试过程
------
| 兼容性测试 | echarts-wordcloud1.0.0 | echarts-wordcloud1.1.0 | echarts-wordcloud1.1.1 | echarts-wordcloud1.1.2 | echarts-wordcloud1.1.3 |
| ------ | ------ | ------ | ------ 
| echarts 3.2.2 | shape ok maskImage ok | shape ok maskImage no | shape no maskImage no | shape no maskImage no | shape no maskImage no |
| echarts 3.7.2 | shape ok(color error) maskImage ok(color error) | shape ok(color error) maskImage no | shape ok maskImage no | shape ok maskImage no | shape ok maskImage no |
| echarts 4.1.0 | shape ok(color error) maskImage ok(color error) | shape ok(color error) maskImage no | shape ok maskImage no | shape ok maskImage no | shape ok maskImage no |

### 结论
------
1. 官网demo时基于echarts-3.2.2和echarts-wordcloud1.0.0来实现的。

2. 现有工程试用的是echarts-4.1.0，如果降级到echarts-3.2.2搭配echarts-wordcloud1.0.0来实现词云，不排除较低版本的echarts对其他可视化图表产生影响。(已经发现使用echarts-3.2.2会对水球图产生影响，echarts-liquidfill-2.0.0依赖echarts^4.0.2和zrender^4.0.1)

3. 下 一步准备尝试使用wordcloud2.js来实现词云(注: echarts-wordcloud基于wordcloud2.js)

### 后续
------
已经在[ecomfe/echarts-wordcloud](https://github.com/ecomfe/echarts-wordcloud)提交[Issues](https://github.com/ecomfe/echarts-wordcloud/issues/62) . 本文记录了踩坑的过程, 供其他小朋友参考.