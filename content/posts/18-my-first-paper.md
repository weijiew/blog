---
title: '比较不同数值计算方法下激光扫描的茎秆直径数据'
date: 2021-01-17
published: false
slug: 18-my-first-paper
tags: ['总结','翻译']
cover_image: "./images/p18.jpg"
canonical_url: false
description: '论文翻译'
---

:::note
为了更好的理解，我打算翻译一遍这一篇论文。
:::

# 比较不同数值计算方法下使用激光扫描的茎粗检索数据

<a href="License: CC BY-SA 4.0"><img src="https://img.shields.io/github/license/weijiew/codestep?color=265ca2&labelColor=212c42)](http://creativecommons.org/licenses/by-sa/4.0/" alt="copyright"/></a>

> 摘要：地面激光扫描仪（TLS）为森林调查提供了一个毫米级别的测量工具。但是样本扫描茎杆直径检索的精度没有达到一个令人信服的级别。有必要对茎粗检索算法每一步所造成的误差进行评估。在本次研究中，我们关注于数值计算步骤，并且比较了几种数值计算方法的异同，例如 cylinder fitting (CYF), circle fitting (CF), convex hull line fitting (CLF), closure B-Spline curve fitting (SP), closure Bézier curve fitting with global convexity (SPC) and 我们目前的 caliper simulation method (CSM). 这些方法使用了经过点云处理的 2297 份茎片数据进行比对，这些数据通过 3D 茎轴曲线进行采集，并根据完整性进行过滤。与 SPC 方法相比，CYF 和 CF 方法的 ME，MAE，RMSE 分别为 -1.672 cm, 1.627 cm 和 1.730 cm; SP 方法则分别为 -0.130 cm, 0.131 cm 和 0.180 cm；CLF，CSM 方法则分别为 -0.070 cm, 0.070 cm 和 0.090 cm；该结果说明了 CYF 和 CF 具有相同的表现，这两种方法的检索结果均小于 SPC 方法。CLF 和 CSM 具有相同的表现，这两种方法的检索结果和 SPC 方法非常接近。该研究表明，对于茎秆检索而言茎点预处理是一个非常重要的步骤，关注此处需要有明确的目标和关注的重点。可以通过加强茎点预处理步骤来提高样本扫描的茎粗检索精度，例如评估茎秆点云数据的完整性，并根据茎片点云数据的特性来选择合适的数值计算方法。


  关键字：TLS; 茎粗检索; 茎秆直径带; 测径尺; 圆柱体拟合;

# 1. 介绍


Terrestrial laser scanning (TLS) has emerged as an accurate means for non-destructively deriving three-dimensional (3D) forest structural attributes. It has a capacity to obtain the 3D geometrical structure of a tree in a millimeter-level, that is beyond the ability of traditional measurement tools. 

After two decades of research, the tree attributes that can be automatically retrieved from TLS data include not only widely used forest inventories, but also those that are not measurable using
conventional tools. 

For example, tree location, stem diameter, stem curve, stem height, stem crown width estimation, stem volume calculations and other forestry parameters. 

However, TLS has not yet been considered as an operational tool in forest inventories, and the main reason is that the difficulties in constructing automatic stem parameter retrieval methods that provides convincing measurement results.
