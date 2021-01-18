---
title: '比较不同数值计算方法下激光扫描的茎粗检索数据'
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

# 比较不同数值计算方法下激光扫描的茎粗检索数据

<a href="License: CC BY-SA 4.0"><img src="https://img.shields.io/github/license/weijiew/codestep?color=265ca2&labelColor=212c42)](http://creativecommons.org/licenses/by-sa/4.0/" alt="copyright"/></a>

> 摘要：地面激光扫描仪（TLS）为森林调查提供了一个毫米级别的测量工具。但是样本扫描茎杆直径检索的精度没有达到一个令人信服的级别。有必要对茎粗检索算法每一步所造成的误差进行评估。在本次研究中，我们关注于数值计算步骤，并且比较了几种数值计算方法的异同，例如 cylinder fitting (CYF), circle fitting (CF), convex hull line fitting (CLF), closure B-Spline curve fitting (SP), closure Bézier curve fitting with global convexity (SPC) and 我们目前的 caliper simulation method (CSM). 这些方法使用了经过点云处理的 2297 份茎片数据进行比对，这些数据通过 3D 茎轴曲线进行采集，并根据完整性进行过滤。与 SPC 方法相比，CYF 和 CF 方法的 ME，MAE，RMSE 分别为 -1.672 cm, 1.627 cm 和 1.730 cm; SP 方法则分别为 -0.130 cm, 0.131 cm 和 0.180 cm；CLF，CSM 方法则分别为 -0.070 cm, 0.070 cm 和 0.090 cm；该结果说明了 CYF 和 CF 具有相同的表现，这两种方法的检索结果均小于 SPC 方法。CLF 和 CSM 具有相同的表现，这两种方法的检索结果和 SPC 方法非常接近。该研究表明，对于茎秆检索而言茎点预处理是一个非常重要的步骤，关注此处需要有明确的目标和关注的重点。可以通过加强茎点预处理步骤来提高样本扫描的茎粗检索精度，例如评估茎秆点云数据的完整性，并根据茎片点云数据的特性来选择合适的数值计算方法。


  关键字：TLS; 茎粗检索; 茎秆直径带; 测径尺; 圆柱体拟合;

# 1. 介绍

三维激光扫描（TLS）已经形成了一种无损获取三维森林结构属性的精确手段。它有能力来获得毫米级别的 3D 几何结构的树木，并且该方法超越的传统的测量工具。经过二十年的研究，从 TLS 数据中自动检索到的树木属性不仅包括广泛使用的森林资源，还包括那些无法用常规工具测量的属性。例如，树木的位置，茎粗，茎曲线，茎高，茎冠宽度估算，茎部体积估算和其他的林业参数。然而，在森林资源中 TLS 依旧没有被看作成一个实用的工具，主要问题在于构造自动茎参检索方法所提供的结果是难以信服的。

在森林资源中茎粗指标是非常重要的参数并且一些重要的茎秆指标的计算也是在建立在该指标的基础上。例如构造茎秆锥度和茎秆数量销售。因此利用 TLS 数据研究森林资源的茎粗精度检索成为了很重要的研究。

茎粗检索算法存在很多步骤，例如茎秆横截面检测，茎点的选择和数值计算。根据这篇报告（. Kore ˇn et al.），采用拟圆法检索茎秆直径（DBH）的精度是受茎秆横截面的厚度所影响的。这个影响在 1-10 cm 之间没有显著差别，但是在 10-100 cm 之间有相当大的差别。在实践中，对于所有的茎粗检索算法而言评估每一步所有步骤的引入误差是不现实的。然而，茎粗检索算法的所有步骤可以被分为两大主要步骤，一是茎点预处理，该步骤侧重于数据收集和预处理，例如 co-registration ，基点移除，茎部位置，茎秆截面检测和茎点挑选。二是对于茎粗检索的数值计算，该步骤侧重于数值计算方法，例如拟圆法和圆柱体拟合。这两个步骤的目的在于分别提供高质量茎点和茎粗检索，分别具有较高的精度。因此茎秆检索算法被分为两个明确的步骤，当两个目标全部被实现时将会得到令人满意的茎秆直径。

在过去的二十多年种，许多的茎秆检索的数值计算方法被引进和研究用于不同树种和环境因素进行研究。综上所述，这些方法大致可以分为两种类型：（1）常规的经典方法。茎部被简化为常规的几何体，而茎秆的横截面被假定为一个圆形的几何体。因此茎秆的直径被圆柱体拟合，拟圆法，Hough 转换和一些其他的变体方法检测出来。这种方法是很容易执行的，并且当茎点密度不足时还可以输出令人满意的结果，尤其是对于单次扫描的数据。然而，

However, these overlook the irregularity of the stem cross-section that can be derived from TLS data, and may not always correspond to the reality [7]. 

(2) simulated manual measurement approach. The irregularity of the stem cross-section and the working scenario of the field work, i.e., the path of the diameter tape is passed through the convex part of the stem cross-section, are considered in stem diameter retrieval.

From this, the stem diameter is retrieved by simulating the path of the diameter tape using convex
hull line fitting (CLF) [21], closure B-Spline curve fitting (SP) [8] and closure Bézier curve fitting with global convexity (SPC) [22]. Such methods can achieve better retrieval accuracy, however, they are time-consuming and require high point density. Although these above methods in different studies have achieved satisfactory result using respective study material. 

It is difficult to directly compare the performances of these methods between different studies as the inputs are different [7,11]. 

This brings up the question of how to choose a stem diameter retrieval numerical method for a given stem point cloud.

71