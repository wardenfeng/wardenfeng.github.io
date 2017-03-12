# feng3d

## 风的3d引擎介绍

2014年3月14日 开始写feng3d的第一天

制作这个引擎的最主要的目的是为了掌握3d引擎技术，精通away3d。制作过程参考away3d引擎，难免会有与away3d相似的地方，或者会成为away3d的feng版本，也或者会成为与away3d有极大差别的引擎。

目前这个阶段就是抄袭away3d，先想办法站到away3d这个巨人的肩膀上去。

现在只是个开始，需要一两年甚至几年的努力吧。

## feng3d相对away3d的优化 （当然feng3d很多地方不如away3d的）

1. 自定义事件机制，实现3d显示对象上的冒泡功能。

2. Context3d缓存处理系统，高效避免没必要的计算。

3. 使用Fagal降低AGAL代码书写难度，提高可读性。

## 历史记录（有待整理）

2015年7月22日（new）: 整理动画模块，优化Context3D缓存系统，优化引擎框架，实现阴影渲染。版本代码：[http://git.oschina.net/wardenfeng/feng3d/tree/shadowmap/](http://git.oschina.net/wardenfeng/feng3d/tree/shadowmap/)

2015年2月6日：实现粒子特效、优化Context3D缓存系统、整理动画模块（未完成）。

2014年11月10日：feng3d已实现3d场景中容器与对象，mesh、线段、天空盒、顶点动画、骨骼动画、光照渲染等功能。并且从feng3d中衍生出类Cg语言的[Fagal](fagal.md)库，使用轻松的方式来取代AGAL渲染语言的编写过程。

feng3d示例
http://www.feng3d.me/?page_id=30

最新代码（20150722）
[http://git.oschina.net/wardenfeng/feng3d](http://git.oschina.net/wardenfeng/feng3d)

API REFERENCE
[http://www.feng3d.me/feng3dApi/index.html](http://www.feng3d.me/feng3dApi/index.html)

以前版本（有待整理）
[http://pan.baidu.com/s/1mg5fSRA](http://pan.baidu.com/s/1mg5fSRA)