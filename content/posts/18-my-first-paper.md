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

> 摘要：地面激光扫描仪（TLS）为森林调查提供了一个毫米级别的测量工具。
> 但是样本扫描茎杆直径检索的精度没有达到一个令人信服的级别。
> 有必要对茎粗检索算法每一步所造成的误差进行评估。
> 在本次研究中，我们关注于数值计算步骤，并且比较了几种数值计算方法的异同，例如 cylinder fitting (CYF), circle fitting (CF), convex hull line fitting (CLF), closure B-Spline curve fitting (SP), closure Bézier curve fitting with global convexity (SPC) and 我们目前的 caliper simulation method (CSM). 
> 这些方法使用了经过点云处理的 2297 份茎片数据进行比对，这些数据通过 3D 茎轴曲线进行采集，并根据完整性进行过滤。
> 与 SPC 方法相比，CYF 和 CF 方法的 ME，MAE，RMSE 分别为 -1.672 cm, 1.627 cm 和 1.730 cm; SP 方法则分别为 -0.130 cm, 0.131 cm 和 0.180 cm；CLF，CSM 方法则分别为 -0.070 cm, 0.070 cm 和 0.090 cm；该结果说明了 CYF 和 CF 具有相同的表现，这两种方法的检索结果均小于 SPC 方法。
> CLF 和 CSM 具有相同的表现，这两种方法的检索结果和 SPC 方法非常接近。
> 该研究表明，对于茎秆检索而言茎点预处理是一个非常重要的步骤，关注此处需要有明确的目标和关注的重点。
> 可以通过加强茎点预处理步骤来提高样本扫描的茎粗检索精度，例如评估茎秆点云数据的完整性，并根据茎片点云数据的特性来选择合适的数值计算方法。
  
  关键字：TLS; 茎粗检索; 茎秆直径带; 测径尺; 圆柱体拟合;

# 1. 介绍

三维激光扫描（TLS）已经形成了一种无损获取三维森林结构属性的精确手段。
它有能力来获得毫米级别的 3D 几何结构的树木，并且该方法超越的传统的测量工具。
经过二十年的研究，从 TLS 数据中自动检索到的树木属性不仅包括广泛使用的森林资源，还包括那些无法用常规工具测量的属性。
例如，树木的位置，茎粗，茎曲线，茎高，茎冠宽度估算，茎部体积估算和其他的林业参数。
然而，在森林资源中 TLS 依旧没有被看作成一个实用的工具，主要问题在于构造自动茎参检索方法所提供的结果是难以信服的。

在森林资源中茎粗指标是非常重要的参数并且一些重要的茎秆指标的计算也是在建立在该指标的基础上。
例如构造茎秆锥度和茎秆数量销售。
因此利用 TLS 数据研究森林资源的茎粗精度检索成为了很重要的研究。

茎粗检索算法存在很多步骤，例如茎秆横截面检测，茎点的选择和数值计算。
根据这篇报告（. Kore ˇn et al.），采用拟圆法检索茎秆直径（DBH）的精度是受茎秆横截面的厚度所影响的。
这个影响在 1-10 cm 之间没有显著差别，但是在 10-100 cm 之间有相当大的差别。
在实践中，对于所有的茎粗检索算法而言评估每一步所有步骤的引入误差是不现实的。
然而，茎粗检索算法的所有步骤可以被分为两大主要步骤，一是茎点预处理，该步骤侧重于数据收集和预处理，例如 co-registration ，基点移除，茎部位置，茎秆截面检测和茎点挑选。
二是对于茎粗检索的数值计算，该步骤侧重于数值计算方法，例如拟圆法和圆柱体拟合。
这两个步骤的目的在于分别提供高质量茎点和茎粗检索，分别具有较高的精度。
因此茎秆检索算法被分为两个明确的步骤，当两个目标全部被实现时将会得到令人满意的茎秆直径。

在过去的二十多年种，许多的茎秆检索的数值计算方法被引进和研究用于不同树种和环境因素进行研究。
综上所述，这些方法大致可以分为两种类型：（1）常规的经典方法。茎部被简化为常规的几何体，而茎秆的横截面被假定为一个圆形的几何体。
因此茎秆的直径被圆柱体拟合，拟圆法，Hough 转换和一些其他的变体方法检测出来。
这种方法是很容易执行的，并且当茎点密度不足时还可以输出令人满意的结果，尤其是对于单次扫描的数据。
然而，这都忽略了从 TLS 所得的茎秆横截面数据的不规则性，拟合结果可能不符合实际情况。

（2）手工模拟测量。茎秆横截面的不规则性和实际的工地地点。
茎带的是走向沿着茎秆横截面的突起部分，这些数据在茎粗检索中被考虑。
由此，茎秆直径是通过使用 CLF，SP 和 SPC 来模拟茎带突起部分实现检索的。
尽管以上这些方法在不同的研究中分别使用不同的研究材料已经达到了令人满意的成就。
由于不同演技的输入不同，所以难以直接比较这些方法的性能。
这就带来了一个问题，茎秆点云数据如何选择什么样的茎秆检索计算机方法。

除了茎秆直径带之外，在茎粗的工作领域中卡尺也是一个常用的工具。
卡尺被用于测量茎秆直径的几个不同方向，而所有方向直径的平均值都是采用卡尺测量的茎秆直径。
Van Laar 和 Akca 列出了详细的卡尺操作规范。
在实践中，使用茎尺测量的茎粗要略高于卡尺测量的结果。
唐氏理论证明采用卡尺测量所测量茎秆直径所有方向的平均值等同于使用茎径尺测量的结果，无论茎秆横截面凹凸。
目前，TLS 可以捕获茎秆横截面的几何特征，这使得使用 80 条 TLS 数据来模拟测量茎尺测量和卡尺测量等价性判定成为可能。

在本次研究中，我们关注的是不同茎粗数值计算方法下的相似与不同，而不考虑茎点预处理的步骤。
具体目标是：
（1）评估不同不同茎秆检索数值计算方法理论和实际上的表现。
（2）评估茎尺和卡尺测量的等价性。
（3）讨论提高茎秆参数检索精度的可行性。

# 2. 材料和方法

## 2.1 茎点集合和预处理 

扫描的样本是在 2018 年七月在中国辽宁省，大沽家林场 (N42◦210, E124◦520 ) 
研究材料的扫描数据来源于三个山椒树，每一幅图的区域为 30 m × 30 m.
树的扫描数据分别来源于九个 TLs 位置。（中心位置，四个角和每个边框的中心位置）
扫描装置为 FARO Focus3D 92 X 330 . 
茎点数据被手动提取于 Geomagic Studio 12.
实验数据采用数据处理程序来获得。
对于茎秆的点云数据，将 1 cm 半径内少于 1 的临近点作为异常值剔除。
除此之外，构造 3D 茎轴曲线。
因此，任何方向下的茎秆生长曲线都可以构造的 3D 茎轴曲线所计算。
除此之外，茎秆横截面平面开始于 10 cm ，结束于顶部间隔 10 cm 之处。
对于每个计算高度，茎秆由两个并行的横截面组成，这两个横截面分别叫做上茎片横截面和下茎片横截面。kkk
下方为茎轴曲线构建的茎横截面平面，上方为下方，两者之间的距离为10 cm。对于一个茎片，通过几何变换，将计算出的茎片生长方向的矢量旋转到矢量（0，0，1），得到三维茎片点云。
通过将三维茎片点云投影到水平面上，得到二维临时点云。
二维临时点云的茎干截面轮廓点采用角度简化法计算，即按角度间隔5度将二维临时点云划分为72个角度区域，并以二维临时点云为几何中心点。
一个茎干截面轮廓点是一个角度区域内所有点的平均值。
然后，对干片的完整性进行检查，即如果空角区域的数量大于6，则丢弃该三维干片。
由于当茎干横截面厚度在 1 - 10 cm之间时，DBH检索精度影响不大，因此将茎干片厚度为5 cm的中间部分作为茎干参数检索的三维茎干片点云。

在这项研究中，我们获得了 36 个茎的 2297 个 3D 茎片点云。 对每个三维茎片点云分别进行茎径检索方法。

# 2.2. 茎径检索方法

在本研究中，CYF、CF、SP、SPC、CLF和CSM方法的初始输入都是三维干片点云，不同的方法在最终的计算中使用了由三维干片点云导出的不同形式。

CYF方法的输入是三维茎片点云。由于三维茎片点云的茎生长方向通过几何变换旋转到矢量(0，0，1)，所以茎生长方向以及拟合圆柱体的轴线方向都等于矢量(0，0，1)。然后，采用最小二乘法进行圆柱体拟合，以轴线方向为输入参数，检索出茎的直径。
CF方法最终输入的是二维茎片点云，即三维茎片点云在水平面上的投影点集。然后用最小二乘法通过圆周拟合检索出茎部直径。

CLF方法最后输入的是凸壳点集，即二维茎片点云的凸壳。然后，将相邻两个凸壳点连接起来，得到一个凸壳多边形。茎秆直径等于凸壳多边形的周长除以PI。

SP方法的最后输入是凸体点集。利用Global插值法在凸壳点集上插值出一条闭合的立方体B-spline曲线。阀杆直径等于闭合B-Spline曲线的长度除以PI。

图1. 使用椭圆度为6.69%的茎片进行茎径检索方法的示意图。 黑点代表茎点（或其在二维空间的投影），蓝点代表凸点壳点，(b)中的红色表面代表拟合圆柱体的表面，红色点(曲线) 代表拟合点（曲线）。(a)三维茎片点云；(b)三维空间的圆柱体拟合；(c)通过圆柱体拟合在二维空间中对三维茎片点云和拟合点的投影；（d．圈拟合；（e）三维茎片点云及其凸壳点的投影；（f）立方体 B-直线拟合采用凸壳点；(g)凸壳线拟合采用凸壳点；(h)立方体．利用凸壳点进行闭合贝齐尔曲线拟合。三个子图中的矩形代表了三种方法的区别部分。

SPC方法的最终输入是凸壳点集。一个闭合的凸立方体Bézier? 通过构建具有二阶几何性质的片状立方体贝齐尔曲线，得到曲线连续性和全局凸性。茎径等于闭合凸曲线的长度除以PI。

图1为上述方法与茎片的示意图。


## 2.2.2. 卡尺的方法模拟

据我们所知，还没有报道的研究使用卡尺模拟检索茎直径使用TLS数据。在这项研究中，我们提出了一个卡尺模拟方法（CSM）来检索茎直径使用TLS数据。

CSM方法的最终输入是凸壳点集。

In the filed work, the two arms of the caliper is paralleled, and located on the convex part of the stem cross-section in each measurement direction. 
Thus, the two arms can be seemed as two parallel lines that just pass through the convex part of the stem cross-section, while the entire region of the stem cross-section is located between the two lines. 

To ensure the accuracy of the CSM method, several angular regions (72 regions in this study) with angle interval α (it was set to 5 in this study) degrees were constructed based on the geometrical central point of the convex hull points set. 

The convex hull points fall into different regions. 

The i-th region and (i + 360
2α
)-th region (0 ≤ i ≤ 360
150 2α − 1) were formed
i-th caliper measurement direction. Then, the caliper measurement can be simulated at 360
2α
151 directions.
152 For i-th direction, two lines were constructed to simulate the two arms of the caliper (Figure 2). The
direction vector of the two lines was (sin(i × α + α
2
), cos(i × α + α
2
153 )).After that, the two convex hull
154 points was searched form the convex hull points set to meet that the two lines passed through the
155 searched two convex hull points and the other convex hull points were located between the two lines.
156 Then, the distance of the two lines was the stem diameter of the i-th region by caliper simulation. At
last, the final stem diameter, i.e., the average value of stem diameters of 360
2α
157 directions, was calculated
after caliper simulation for each direction.


Based on the CSM method, the minimum and maximum stem diameters were retrieved among
360
2α
directions measurement. Thus, the ovality in percent [30] for a 3D stem slice was calculated
according by the following formula.