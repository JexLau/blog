---
title: 基于antv/g设计工业地图渲染器
description: 背景和目标：原demo（基于红牛地图）在拖动放大过程中，FPS在40-60之间，但目前线上的红牛地图只有10-30FPS，因此这次的测试目标是，定位webgl在大地图渲染卡顿问题，能让它达到原demo的渲染效果。改造过程：去掉react挂载因目标是做成一个库，所以需要去掉react挂载，改为v...
pubDate: 2022-08-21T07:54:23.000Z
---

背景和目标：<br />原demo（基于红牛地图）在拖动放大过程中，FPS在40-60之间，但目前线上的红牛地图只有10-30FPS，因此这次的测试目标是，定位webgl在大地图渲染卡顿问题，能让它达到原demo的渲染效果。

改造过程：

1. 去掉react挂载

因目标是做成一个库，所以需要去掉react挂载，改为vite启动和打包（vite简单好用）。<br />测试结果：没有问题，FPS依然是40-60，帧率没有明显下降

2. 工程化

原demo将所有的业务逻辑都写在了源代码中，期望将业务逻辑代码抽离处理，渲染器只负责渲染。

   1. 将数据请求抽离，改为传入到实例中，测试结果40-60FPS
   2. 地图元素渲染器(node, area, edge......)，测试结果非常卡顿，init时间>22秒，拖动等其他操作也非常卡顿。经过debugger后，发现是Arrow元素原因。(预期做一个性能实验室，每次增加元素后的输出性能数据)
   3. 项目目录结构(typings，utils，constants....)
   4. 配置化(width, height, data, color, line-width, font-size......)
