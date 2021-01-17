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

# 比较不同数值计算方法下激光扫描的茎秆直径数据

> 摘要：地面激光扫描仪（TLS）为森林调查提供了一个毫米级别的测量工具。但是样本扫描茎杆直径检索的精度没有达到一个令人信服的级别。

However, the accuracy of stem diameter retrieval in the sample plot scanning has not reached a convincing level. It is necessary to evaluate the errors caused by each step of stem diameter retrieval algorithms. In this study, we focused on the numerical calculation step, and compared
 similarities and differences of several numerical calculation methods , i.e., cylinder fitting (CYF),
  circle fitting (CF), convex hull line fitting (CLF), closure B-Spline curve fitting (SP), closure Bézier
  curve fitting with global convexity (SPC) and our presented caliper simulation method (CSM). These
  methods were compared using processed point clouds containing 2297 stem slices, which were
 collected by the 3D stem axis curve and filtered according to the completeness. Compared with the
  SPC method, the mean error (ME), mean absolute error (MAE) and root mean square error (RMSE) of
  the CYF and CF methods were -1.672 cm, 1.627 cm and 1.730 cm, respectively; that of the SP method
  were -0.130 cm, 0.131 cm and 0.180 cm, respectively; and that of the CLF, CSM methods were -0.070
  cm, 0.070 cm and 0.090 cm, respectively. The results illustrated that the CYF and CF methods had
  the same performance, the retrieval results by the two methods were smaller than that by the SPC
  method; the CLF and CSM had the same performance, the retrieval results by the two methods
  were very close to that by the SPC method. This study demonstrated that stem points preprocessing
  step is an important step for stem diameter retrieval, which should have clear aims and need to be
  focused on. The feasible ways to improve the accuracy of stem diameter retrieval in sample plot
  scanning include enhancing the content of the stem points preprocessing step, such as evaluating the
  completeness of the stem slice point cloud, and selecting appropriate numerical calculation method
  according to the properties of the stem slice point cloud.
  Keywords: TLS; stem diameter retrieval; stem diameter tape; caliper; cylinder fitting;
  1. Introduction
  Terrestrial laser scanning (TLS) has emerged as an accurate means for non-destructively deriving
  three-dimensional (3D) forest structural attributes [1]. It has a capacity to obtain the 3D geometrical
  structure of a tree in a millimeter-level, that is beyond the ability of traditional measurement tools
  [2]. After two decades of research, the tree attributes that can be automatically retrieved from TLS
Submitted to Remote Sens., pages 1 – 14 www.mdpi.com/journal/remotesensing
Version December 25, 2020 submitted to Remote Sens. 2 of 14
  data include not only widely used forest inventories, but also those that are not measurable using
  conventional tools. For example, tree location [3–5], stem diameter [6–10], stem curve [11,12], stem
  height [9,13], stem crown width estimation [9], stem volume calculations [14,15] and other forestry
  parameters. However, TLS has not yet been considered as an operational tool in forest inventories, and
  the main reason is that the difficulties in constructing automatic stem parameter retrieval methods that
  provides convincing measurement results [16].
  Stem diameter is one of the most important parameters in forest inventory as some important stem
  metrics are calculated based on it, such as stem taper construction and merchantable stem volume[17].
  Therefore, accurate retrieval of stem diameter has become one of the important studies for forest
  inventories using TLS data.
  There are many steps in a stem diameter retrieval algorithm, such as stem cross-section
  determination, stem points selection and numerical computation. Kore ˇn et al. reported that the
  accuracy of stem diameter at breast height (DBH) retrieval by circle fitting method is effected by the
  stem cross-section thickness, the impact is not significantly different among 1-10 cm and is considerably
  different among 10-100 cm [18]. In practice, it is unrealistic to evaluate introduced errors of all steps
  for all stem diameter retrieval algorithms. However, the steps of all stem diameter retrieval algorithms
  can be divided into two major steps, one is stem points preprocessing, which focus on data collection
  and preprocessing, such as co-registration, ground points removal, stem location, stem cross-section
  determination and stem points selection. The next step is numerical calculation for stem diameter
  retrieval, which focus on numerical computational method, such as circle fitting and cylinder fitting.
  The aims of the two steps are providing stem points with high quality and retrieving stem diameter
  with high precision, respectively. Thus, the steps of stem diameter retrieval algorithms are divided
  into two steps with clear aims, and the satisfactory stem diameter will be retrieved when the two aims
  are all achieved.
  Many stem diameter retrieval numerical methods have been introduced and performed with
  different tree species and environmental factors during the past two decades. In summary, these
  methods can be roughly divided into two types: (1) classic regular approach. The stem is simplified as
  a regular geometry, of which the stem cross-section is assumed as a circular geometry. Therefore, stem
56 diameter is retrieved using cylinder fitting (CYF) [9], circle fitting (CF) [3], Hough transform [19] and
57 some variants methods [12,20]. Such methods are easy to perform, and also can output satisfactory
58 result when stem point density is insufficient, especially for single-scan data. However, these overlook
59 the irregularity of the stem cross-section that can be derived from TLS data, and may not always
60 correspond to the reality [7]. (2) simulated manual measurement approach. The irregularity of the
61 stem cross-section and the working scenario of the field work, i.e., the path of the diameter tape is
62 passed through the convex part of the stem cross-section, are considered in stem diameter retrieval.
63 From this, the stem diameter is retrieved by simulating the path of the diameter tape using convex
64 hull line fitting (CLF) [21], closure B-Spline curve fitting (SP) [8] and closure Bézier curve fitting with
65 global convexity (SPC) [22]. Such methods can achieve better retrieval accuracy, however, they are
66 time-consuming and require high point density. Although these above methods in different studies
67 have achieved satisfactory result using respective study material. It is difficult to directly compare the
68 performances of these methods between different studies as the inputs are different [7,11]. This brings
69 up the question of how to choose a stem diameter retrieval numerical method for a given stem point
70 cloud.
71 In addition to the stem diameter tape, the caliper is also a common measuring tool for stem
72 diameter in the field work. The caliper is used to measure stem diameters in several directions, and
73 the average of all the directional diameters is the stem diameter measured by the caliper. Van Laar and
74 Akca listed the detailed quality specifications for caliper operation [23]. The stem diameter measured
75 with the diameter tape were slightly larger than that with the caliper in practice [24]. Tang theoretically
76 proved that the average of the stem diameter measured in all directions with the caliper is equal to
77 that measured using the stem diameter tape whether the shape of the stem cross-section is convex or
Version December 25, 2020 submitted to Remote Sens. 3 of 14
78 concave [25]. At present, TLS can capture the geometrical characteristic of the stem cross-section, it
79 becomes possible to evaluate the equivalency between the stem diameter tape and the caliper using
80 TLS data by simulation measurement.
81 In this study, we consider the similarities and differences between several stem diameter numerical
82 calculation methods, without regard to the stem points preprocessing step. The specific objectives were:
83 (1) to evaluate the performances of different stem diameter retrieval numerical methods practically
84 and theoretically; (2) to evaluate the equivalency between the diameter tape and the caliper; (3) to
85 discuss feasible ways to improve the accuracy of stem parameter retrieval.
86 2. Materials and Methods
87 2.1. stem points collection and preprocessing
The scanning sample was performed in July 2018 in Dagujia Forest Farm (N42◦210
, E124◦520
88 ) in
89 Qingyuan County, Liaoning Province, China. The study materials sourced from three Larix kaempferi
90 plots, the area of each plot is 30 m × 30 m. The trees were scanned from nine TLS positions (the centre
91 position of the stand, four corners and centre positions of each border). The scanner device was a
FARO Focus3D 92 X 330 [26]. The stem points were manually extracted in Geomagic Studio 12.
93 A set of data processing procedures was used to obtain the experimental data. For a stem point
94 cloud, the outliers were removed when the number of neighbor point in a radius of 1 cm was less
95 than 1. After that, the 3D stem axis curve [22] was constructed. Then, the stem growth direction at
96 any height along the stem can be calculated by the constructed stem axis curve. Therefore, the stem
97 cross-sectional planes, that started at the height of 10 cm and ended at the tip of the stem with an
98 interval of 10 cm were constructed. For each calculated height, a stem slice was formed by two parallel
99 stem cross-sections, which are called the lower stem cross-section and the upper stem cross-section.
100 The lower one is the stem cross-sectional plane constructed by the stem axis curve, the upper one is
101 above the lower one, and the distance between them is 10 cm. For a stem slice, a 3D stem slice point
102 cloud was obtained through geometric transformation by rotating the vector from the calculated stem
103 growth direction to the vector (0, 0, 1). A 2D temporary point cloud was obtained by projecting the
104 3D stem slice point cloud onto the horizontal plane. The stem cross-sectional profile points of the 2D
105 temporary point cloud were calculated by the angle simplification [22], that is, the 2D temporary point
106 cloud were divided into 72 angular regions by angle interval 5 degrees and the geometrical central
107 point of the 2D temporary point cloud. A stem cross-sectional profile point was the average of all the
108 points in an angular region. Then, the completeness of the stem slice was checked, i.e., if the number of
109 the empty angular regions is bigger than 6, the 3D stem slice was discarded. As the impact of accuracy
110 of DBH retrieval is not significant when the thickness of stem cross-section among 1-10 cm [18], the
111 middle part with 5 cm thickness of a stem slice were used as a 3D stem slice point cloud for stem
112 parameter retrieval.
113 In this study, we obtained 2297 3D stem slice point clouds from 36 stems. Stem diameter retrieval
114 methods were performed on each 3D stem slice point cloud separately.
115 2.2. stem diameter retrieval methods
116 2.2.1. methods simulation for the diameter tape
117 In this study, the initial input of the CYF, CF, SP, SPC, CLF and CSM methods were 3D stem slice
118 point clouds, and different methods use different forms derived from 3D stem point clouds in the final
119 calculation.
120 The input of the CYF method was the 3D stem slice point cloud. As the stem growth direction of
121 the 3D stem slice point cloud was rotated to the vector (0, 0, 1) through geometric transformation, the
122 stem growth direction, as well as the axis direction of the fitted cylinder, were equal to the vector (0,
Version December 25, 2020 submitted to Remote Sens. 4 of 14
123 0, 1). Then, the stem diameter was retrieved by the cylinder fitting using least squares method [27],
124 which using the axis direction as an input parameter.
125 The final input of the CF method was the 2D stem slice point cloud, that was the projection point
126 set of the 3D stem slice point cloud on the horizontal plane. Then, the stem diameter was retrieved by
127 circle fitting using least squares method.
128 The final input of the CLF method was the convex hull points set, which is the convex hull of the
129 2D stem slice point cloud. Then, a convex hull polygon was obtained by connecting the two adjacent
130 convex hull points. The stem diameter was equal to the perimeter of the convex hull polygon divided
131 by the PI.
132 The final input of the SP method was the convex hull points set. A closed cubic B-spline curve
133 was interpolated on the convex hull points set using Global interpolation [28]. The stem diameter was
equal to the length of the closure B-Spline curve divided by the PI.
Figure 1. Diagrams of stem diameter retrieval methods using a stem slice with an ovality of 6.69%.
The black points represent stem points (or its projection in 2D space), the blue points represent convex
hull points, the red surface in (b) represents the surface of the fitted cylinder, the red points (curve)
represent the fitted points (curve). (a) A 3D stem slice point cloud; (b) cylinder fitting in 3D space; (c)
the projection of the 3D stem slice point cloud and fitting points by cylinder fitting in 2D space; (d)
circle fitting; (e) the projection of the 3D stem slice point cloud and its convex hull points; (f) cubic
B-Spline fitting using convex hull points; (g) convex hull line fitting using convex hull points; (h) cubic
closure Bézier curve fitting using convex hull points. The rectangles in the three sub figure represent
the distinction part of the three methods.
134
135 The final input of the SPC method was the convex hull points set. A closed convex cubic Bézier
136 curve was obtained by constructing piecewise cubic Bézier curves with second-order geometric
137 continuity and global convexity [29]. The stem diameter was equal to the length of the closure convex
138 curve divided by the PI.
139 Diagrams of above methods with a stem slice are shown in Figure 1.
140 2.2.2. method simulation for the caliper
141 To the best of our knowledge, there is no reported study using caliper simulation to retrieve stem
142 diameter using TLS data. In this study, we presented a caliper simulation method (CSM) to retrieve
Version December 25, 2020 submitted to Remote Sens. 5 of 14
143 stem diameter using TLS data. The final input of the CSM method was the convex hull points set. In
144 the filed work, the two arms of the caliper is paralleled, and located on the convex part of the stem
145 cross-section in each measurement direction. Thus, the two arms can be seemed as two parallel lines
146 that just pass through the convex part of the stem cross-section, while the entire region of the stem
147 cross-section is located between the two lines. To ensure the accuracy of the CSM method, several
148 angular regions (72 regions in this study) with angle interval α (it was set to 5 in this study) degrees
149 were constructed based on the geometrical central point of the convex hull points set. The convex hull
points fall into different regions. The i-th region and (i + 360
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
0 5 10 15 20 25 30 35 40
x (cm)
0
5
10
15
20
25
30
35
40
y (cm)
Figure 2. The diagram of caliper simulation method. The black points represent the 2D stem slice
points. The blue diamond points represent the convex hull points of the 2D stem slice point cloud.
The colored lines with squares represent the simulated caliper arms, the two lines in one direction are
described in the same color. The red diamond points represent the corresponding convex hull points
that passed through by the two lines.
158
Based on the CSM method, the minimum and maximum stem diameters were retrieved among
360
2α
directions measurement. Thus, the ovality in percent [30] for a 3D stem slice was calculated
according by the following formula.
O = (1 −
DMin
DMax
) × 100 (1)
Where DMin and DMax represent the minimum and maximum stem diameters measured from 360
2α
159
160 directions.
Version December 25, 2020 submitted to Remote Sens. 6 of 14
161 2.3. Reference data and assessment indices
162 The manual measurement with calipers or diameter tapes may lead to potential errors due to the
163 operator’s visual estimation [6]. Meanwhile, the path of the diameter tape can be efficiently simulated
164 by the constructed curve using the SPC method. Compared with field measured stem diameter by the
165 diameter tape, the root mean square error (RMSE) of the SPC method was 0.129 cm for single stem
166 scanning with high point density [22]. Therefore, without loss of generality, stem diameters retrieved
167 by the SPC method were used as the reference data to reduce the impact of human measurement
168 factors.
169 The accuracy of stem diameter retrieval is evaluated using the mean error (ME), mean absolute
170 error (MAE), mean absolute percentage error (MAPE), root mean square error (RMSE), relative root
171 mean square error (RRMSE).
172 3. Results
173 3.1. The basic information of 3D stem slice point clouds
174 The maximum, minimum and average values of stem diameter, the heights at which stem diameter
175 were measured, and the ovality of the studied 3D stem slices are listed in Table 1. The histogram of
176 stem diameters, the heights at which stem diameter were measured, and the ovality of the studied 3D
177 stem slices are shown in Figure 3.
Table 1. The stat info of the studied 3D stem slices
Stem diameter (cm) Measured height (m) Ovality (%)
Maximum 40.99 11.10 20.42
Minimum 7.36 0.10 2.96
Average 22.65 3.65 10.50
(a)
5 10 15 20 25 30 35 40
Stem diameters by the SPC method (cm)
0
50
100
150
200
250
300
350
Count
(b)
0 1 2 3 4 5 6 7 8 9 10 11 12
Height at which stem diameters measured (m)
0
50
100
150
200
Count
(c)
2 4 6 8 10 12 14 16 18 20 22
Ovality (%)
0
50
100
150
200
250
300
350
Count
Figure 3. The histogram of the stem diameter (a), the stem height of stem diameter measured (b)
and ovality (c). Stem diameters were retrieved by SPC method, and the stem diameters for ovality
calculation were retrieved by the CSM method.
178 The stem diameters retrieved by the SPC method for the studied 3D stem slices were mainly
179 distributed between 15 cm and 30 cm. The measured height were mainly distributed between 1 m
180 and 6 m, and the higher the height, the smaller the number of measured diameters. Based on stem
181 diameter retrieved by CSM method, the ovality were mainly distributed between 6% and 14%.
182 3.2. Results of stem diameter retrieval methods
183 The differences between the SPC method and the other stem diameter retrieval methods are
184 shown in Figure 4, and the regression equations and the assessment indices are shown in Figure 5. The
Version December 25, 2020 submitted to Remote Sens. 7 of 14
185 stem diameters retrieved by the CYF method and the CF method all were smaller than that by SPC
186 method, and the differences between them mainly distributed between 1 cm and 3 cm (Figure 4). The
187 ME and MAE values were -1.627 cm and 1.627 cm (Figure 5), respectively. It means that the differences
188 between the CYF method and the SPC method were equal to that between the CY method and the SPC
189 method, i.e., stem diameters retrieved by the CLY method and the CY method were identical in this
190 study. Most stem diameters retrieved by the SP method were larger than that by the SPC method, and
191 most differences between them were around zero (Figure 4). The differences between the CLF method
192 and the SPC method all were equal to that between the CSM method and the SPC method (Figure 5(d)
193 and Figure 5(e), and these differences were positive and negative, but very close to zero (Figure 4). It
194 means that stem diameters retrieved by the CLF method and the CSM method were identical in this
195 study. The assessment indices in Figure 5(e) demonstrated that the stem diameters retrieved by the
CSM method and the SPC method were very close. According to Figure 4 and Figure 5, compared
10 20 30 40
Stem diameters by the SPC method (cm)
-4.0
-3.0
-2.0
-1.0
 0.0
 1.0
 2.0
 3.0
Difference (cm)
DCYF
 - DSPC
DCF
 - DSPC
DCLF
 - DSPC
DCSM
 - DSPC
DSP
 - DSPC
Figure 4. Diameter differences between the SPC method and other stem diameter retrieval methods.
DCYF, DSPC, DCF, DCLF and DCSM represent stem diameters retrieved from CYF, SPC, CF, CLF and
CSM methods.
0 20 40
SPC method (cm)
0
5
10
15
20
25
30
35
40
Cylinder fitting (cm)
(a)
y = 0.980x - 1.173
ME = -1.627 cm
RMSE = 1.730 cm
RRMSE = 0.082
MAE = 1.627 cm
MAPE = 7.57%
0 20 40
SPC method (cm)
0
5
10
15
20
25
30
35
40
Circle fitting (cm)
(b)
y = 0.980x - 1.173
ME = -1.627 cm
RMSE = 1.730 cm
RRMSE = 0.082
MAE = 1.627 cm
MAPE = 7.57%
0 20 40
SPC method (cm)
0
5
10
15
20
25
30
35
40
B-Spline fitting (cm)
(c)
y = 1.003x + 0.070
ME = 0.130 cm
RMSE = 0.180 cm
RRMSE = 0.008
MAE = 0.131 cm
MAPE = 0.60%
0 20 40
SPC method (cm)
0
5
10
15
20
25
30
35
40
Convex line fitting (cm)
(d)
y = 0.999x - 0.045
ME = -0.070 cm
RMSE = 0.090 cm
RRMSE = 0.004
MAE = 0.070 cm
MAPE = 0.32%
0 20 40
SPC method (cm)
0
5
10
15
20
25
30
35
40
Caliper simulation (cm)
(e)
y = 0.999x - 0.045
ME = -0.070 cm
RMSE = 0.090 cm
RRMSE = 0.004
MAE = 0.070 cm
MAPE = 0.32%
Figure 5. Regression equation and assessment index values between the SPC method and other stem
diameter retrieval methods. (a) the CYF method, (b) the CF method, (c) the SP method, (d) the CLF
method, (e) the CSM method.
Version December 25, 2020 submitted to Remote Sens. 8 of 14
196 with the SPC method, the differences of the CLF method and the CSM method were smallest, that of
197 the CYF method and the CY method were largest, and the differences of the SPC method were bigger
198 than that of the CLF method and the CSM method, and were smaller than that of the CYF method
and the CY method. The change of stem diameter differences was not obvious with the increase of
0 2 4 6 8 10 12
Stem height (m)
-4.0
-3.0
-2.0
-1.0
 0.0
 1.0
 2.0
 3.0
Difference (cm)
DCYF
 - DSPC
DCF
 - DSPC
DCLF
 - DSPC
DCSM
 - DSPC
DSP
 - DSPC
Figure 6. The relationship between the diameter differences and the stem height.
5 10 15 20
Ovality (%)
-4.0
-3.0
-2.0
-1.0
 0.0
 1.0
 2.0
 3.0
Difference (cm)
DCYF
 - DSPC
DCF
 - DSPC
DCLF
 - DSPC
DCSM
 - DSPC
DSP
 - DSPC
Figure 7. The relationship between the diameter differences and the ovality.
199
200 the stem diameter. The large differences and small differences were distributed in different diameter
201 ranges (Figure 4). Meanwhile, The change of stem diameter differences also were not obvious with the
202 change of the stem height (Figure 6) and the ovality (Figure 7).
203 4. Discussion
204 4.1. The accuracy analysis of these stem diameter retrieval methods
205 In this study, the input of these methods were the 3D stem slice point clouds, that were collected
206 from the stem cross-section with the height of 5 cm by the 3D stem axis curve. The influencing
207 factors brought by different method using different input were eliminated, and the advantages and
208 disadvantages of these method can be better reflected when and only when using the same input.
Version December 25, 2020 submitted to Remote Sens. 9 of 14
-20 -15 -10 -5 0 5
x (cm)
-25
-20
-15
-10
-5
0
y (cm)
(a)
-20 -15 -10 -5 0 5
x (cm)
-30
-25
-20
-15
-10
-5
0
y (cm)
(b)
-5 0 5
x (cm)
0
2
4
6
8
10
12
14
y (cm)
(c)
Figure 8. The results of the CF and SPC methods using stem slices with different ovality. The ovality
values are (a) 3.01, (b) 10.26 and (c) 14.30, respectively. The black points represent the 2D stem slice
points, the red diamonds represent the convex hull points of the 2D stem slice point cloud, the red
points represent the circle points fitted by the CF method and the blue curves represent the closure
curve with global convexity by the SPC method.
209 The convex hull points set in 2D space derived from the 3D stem slice point cloud was used as
210 input for the CSM, CLF, SP and SPC method in this study. The constructed curve by the SPC method
211 and the constructed lines by the CLF method were closure with global convexity. The constructed
212 two parallel lines by the CSM method were around the 2D stem slice point cloud from different
213 directions, and the two parallel lines of each direction forms a convex region. The measurements from
214 all directions form a complete convex region. This means that the CSM, CLF and SPC methods have
215 very similar measurement mode, which constructed a compact convex region to surround the 2D
216 stem slice. This is why the results of the above three methods were very similar. Although the SP
217 method interpolated on the convex hull points, the constructed cubic B-Spline curve is not a global
218 convex curve [8] (as the rectangle shown in Figure 1 (f)). It caused that the difference between the SP
219 method and the SPC method was larger than that between the CSM, CLF and the SPC method. The
220 extremely similar measurement results between the CSM method and the SPC method demonstrated
221 the viewpoint that the average of the stem diameter measured in all directions with the caliper is equal
222 to that measured using the stem diameter tape, i.e., the equivalency between the stem diameter tape
223 and the caliper.
224 The final input of the CYF and CF methods were the 3D stem slice point cloud and the 2D stem
225 slice point cloud, respectively. The 2D stem slice point cloud was the projection point set of the 3D
226 stem slice point cloud. The normal vector of the projection plane was equal to the stem growth of the
227 3D stem slice, as well as the axis direction of the fitted cylinder. The least squares method were used as
228 fitting method for the two methods. That is why the CYF and CF methods behave the same way (as
229 shown in Figure 1 (c) and Figure 1 (d)) and had the same performance for stem diameter retrieval.
230 Stem diameters retrieved by the CYF and CF methods were smaller than that retrieved by the
231 SPC method. That can be explained from the following two aspects. The first aspect is that the final
232 input of the two methods is different. The SPC method uses convex hull points of the 2D stem slice
233 point cloud, and the CYF (CF) uses 3D (2D) stem slice point cloud. The 3D (2D) stem slice point cloud
234 must be located in the polygon formed by the convex hull points. The second aspect is that the curve
235 fitted by the least squares method must be located between convex hull points and internal boundary
236 points in theory, and the internal boundary point has the smallest distance from the center point of the
237 polygon within a certain angle range. The more regular the stem cross-section, the smaller the ovality
238 value (Equation 1), and the more obvious the located position of the fitted curve that between convex
239 hull points and internal boundary points (Figure 8). Combing the above two aspects, the closure curve
240 with global convexity by the SPC method interpolated on the convex hull points, rather than the inner
241 points, meanwhile, the circle curve by the CYF (CF) method needs to consider both the convex hull
Version December 25, 2020 submitted to Remote Sens. 10 of 14
242 points and the inner points. Thus, it is determined that the stem diameters retrieved by the CYF (CF)
243 method should be smaller than that by the SPC method in theory.
244 Considering the consistency of the SPC method and the field measurement using the diameter
245 tape and the high accuracy of the SPC method for individual stems [22], compared with the field
246 measurement, it can be inferred that the sequence of the accuracy of stem diameter retrieval is the SPC
247 (CLF, CSM) method, the SP method, and the CYF (CF) method; and stem diameters retrieved by the
248 CYF (CF) method should be smaller than the field measurement using stem diameter tape when using
249 relatively complete stem points scanned from sample plot.
250 4.2. The usage of ovality
251 In this study, the ovality of the stem cross-section was calculated by the CSM method. This
252 provides a way to judge the regularity of the stem cross-section and the validity of the stem slice point
cloud using TLS data.
-20 -15 -10 -5 0
x (cm)
-10
-5
0
5
10
y (cm)
(a)
-20 -10 0 10
x (cm)
-30
-25
-20
-15
-10
-5
0
y (cm)
(b)
-30 -20 -10 0
x (cm)
-15
-10
-5
0
5
10
15
20
25
y (cm)
(c)
0 5 10
x (cm)
-12
-10
-8
-6
-4
-2
0
2
4
y (cm)
(d)
Figure 9. Four stem slice point cloud with different ovality values, (a) 5.07, (b) 10.00, (c) 15.01, (d) 20.94.
-10 -5 0 5
x (cm)
-12
-10
-8
-6
-4
-2
0
2
y (cm)
(a)
-5 0 5 10
x (cm)
-6
-4
-2
0
2
4
6
8
10
y (cm)
(b)
-15 -10 -5 0 5 10
x (cm)
-20
-15
-10
-5
0
y (cm)
(c)
Figure 10. Three stem slice point clouds that need to be further processed for stem diameter retrieval,
(a) 23.97, (b) 26.29, (c) 30.17.
253
254 The ovality is change with the regularity of the stem cross-section. The higher the ovality, the
255 higher the degree of irregularity (Figure 9). When the ovality is greater than the threshold value, the
256 validity of the stem slice point cloud start to decrease (Figure 10). It is not difficult to find that stem
257 slice point clouds in Figure 10 are either contains outliers (Figure 10(a) and Figure 10(c)) or registered
258 incorrectly (Figure 10(b)). Actually, an unreasonable stem slice point cloud has been shown in Figure
259 9(d), but it is not more obvious than that in Figure 10. Therefore, the ovality can be used to judge
260 whether the stem cross-section is reasonable and suitable for stem parameter retrieval.
Version December 25, 2020 submitted to Remote Sens. 11 of 14
261 4.3. The implicit parameter of these stem diameter retrieval methods
262 The axis direction of cylinder was an input parameter in the CYF method in this study. It is
263 different from some exist cylinder fitting methods, for example, the axis direction is an output variable
264 and is equal to the eigenvector corresponding to the maximum eigenvalue by PCA [31], which is
265 suitable for the height of the stem slice point cloud is over the diameter. As the diameter of the fitted
266 cylinder is closely related to the axis direction, the accuracy of stem diameter can be ensured when
267 providing accurate axis direction. For the CYF method in this study, the input 3D stem point cloud was
268 geometric transformed by rotating the stem growth direction to the vector (0, 0, 1). Then, the vector (0,
269 0, 1) was seemed as the axis direction of the fitted cylinder and an input parameter for cylinder fitting.
270 For the CF and other methods work in the 2D space, the input was the projection of the transformed
271 3D stem point cloud. It is obviously that the stem growth direction is not only a parameter in data
272 preprocessing in this study, but also an important implicit parameter for stem diameter retrieval, that
273 should be paid attention to.
274 4.4. The necessity and importance of stem points preprocessing
275 Fourteen DBH retrieval algorithms (seven algorithms based on the CF method, five algorithms
276 based on the CYF method, one algorithm based on maximum distance, and the details of another
277 algorithm have not been published) were compared using identical TLS datasets and evaluated
278 the results through a common procedure respecting reliable references [2]. The study reported
279 that the performances of these algorithms were different with each other, and parts of algorithms
280 underestimated the DBH, while the others overestimated the DBH. It demonstrated that even if the
281 input data is the same, and the numerical computation methods are very similar (most are based on
282 CF and CYF methods), the differences between different algorithms are quite obvious. According to
283 the performance of CF and CYF methods in this study, although the numerical computation methods
284 of these fourteen algorithms have some subtle differences, the main difference between these fourteen
285 algorithms should be stem points preprocessing. Thus, the necessity and importance of stem points
286 preprocessing has been highlighted.
287 Although the CYF and CF method is not sensitive to outliers, it does not means that the influence
288 of outliers can be overlooked in the CYF and CF methods. The outliers will bring significant errors
289 in the CYF [32] and CF [2] methods. The SP, SPC, CLF and CSM methods are constructed based on
290 convex hull points, these methods are more sensitive to outliers than CYF and CF methods. The
291 outliers in stem slice point cloud in Figure 10(a) and Figure 10(c) will inevitably affect the accuracy of
292 stem parameter retrieval. Therefore, no matter what method is used, removing outliers is a necessary
293 condition for accurate retrieving stem parameter.
294 In this study, the study materials from sample plots were filtered by the number of non-empty
295 angular regions. That can provide enough points and ensure the completeness of the stem cross-section.
296 Obviously, the more the completeness of the stem cross-section, the more accurate the retrieved
297 stem parameters. The perfect stem slice point cloud should be able to effectively reflect the profile
298 characteristics of the stem cross-section, and should be no (little) noise points. However, it is hard to
299 obtain the idea stem slice point cloud in sample plot scanning. Therefore, checking and repairing the
300 completeness of the stem slice point cloud (or checking and providing enough valuable data to help
301 the subsequent numerical calculation step) is a feasible step for accurate stem parameter retrieval.
302 Actually, most stem diameter retrieval algorithms include stem points processing part, either
303 indirectly or directly. For example, Wang et al. used a Fourier series curve to eliminate the outliers
304 and branch points, and approximate the stem cross-section shapes [33]. Wang et al. used a Outer Hull
305 Model method to iteratively fits convex hulls and handles noisy data [34]. Pitkänen et al. evaluated
306 the stem cross-section with multiple-scan data using cylinder fitting and if it is found to be deficient,
307 diameter was detected using single-scan data [7]. These above data processing part is either mixed
308 with or cooperated with numerical calculation method. Getting rid of the influence of subsequent
309 calculation methods and accurately defining the aim of stem points processing separately, it will help
Version December 25, 2020 submitted to Remote Sens. 12 of 14
310 to obtain stem slice point cloud with high-quality, thereby improving the accuracy of stem parameter
311 retrieval.
312 4.5. The feasible path for improving the accuracy of stem diameter retrieval
313 The stem growth direction and the thickness of the stem slice point cloud are parameters that all
314 methods need to use, meanwhile, the two parameters influence the accuracy of stem diameter retrieval.
315 Therefore, the stem growth direction should be accurate calculated, and the thickness of the stem slice
316 point cloud should be determined after point cloud quality assessment or experimental verification.
317 The accuracy of stem diameter retrieval on single tree is higher than that on the plot-level [11]. It
318 means that there is a possibility of improving the accuracy of stem diameter retrieval on the plot-level.
319 According to the above stem diameter retrieval methods, a suitable method can be selected based on
320 the data attributes of a given stem point cloud. For a stem point cloud extracted from single-scan TLS
321 data, the CYF or CF methods are recommended. For a stem point cloud extracted from multi-scan TLS
322 data, the completeness of the stem slice point can be evaluated by the number of non-empty angular
323 regions, if the number is over the count threshold, the CSM, SPC or CLF methods are recommended,
324 otherwise, the CYF or CF methods are recommended. When using the CSM, SPC or CLF methods, the
325 validity of the stem slice point cloud can be evaluated by the ovality using CSM method, if the ovality
326 is over the given threshold (that may be determined by tree species), the stem slice point cloud needs
327 further processing (removing outliers or checking whether the registration is successful) before stem
328 diameter retrieval. Considering the accuracy and computational efficiency of these methods, the order
329 of priority of CSM, SPC and CLF methods is CLF, CSM and SPC method; and the order of priorities of
330 the CYF and CF is similar if the two methods are all implemented by the least square method.
331 5. Conclusions
332 In this study, we focused on the similarities and differences between stem diameter retrieval
333 numerical methods using a set of 3D stem slice point clouds. In order to reduce the impact of manual
334 measurement, the SPC method were used as the referenced method as this method can simulate the
335 measurement mode of stem diameter tape. We also presented a caliper simulation method to verify the
336 equivalency between stem diameter tape and caliper. The results showed that the CYF and CF methods
337 had the same performance, the CLF and the CSM methods had the same performance; compared
338 with the SPC method, stem diameter retrieved by the CYF and CF methods were smaller, and the ME,
339 MAE and RMSE were -1.672 cm, 1.627 cm and 1.730 cm, respectively; most stem diameter retrieved
340 by the SP methods were larger, and the ME, MAE and RMSE were -0.130 cm, 0.131 cm and 0.180 cm,
341 respectively; stem diameter retrieved by the CLF and CSM methods were lower and closer to that
342 retrieved by the SPC method and the ME, MAE and RMSE were -0.070 cm, 0.070 cm and 0.090 cm,
343 respectively.
344 Based on this study, we can come to the following conclusion: (1) the measurement results by
345 the CLF and CSM methods, as well as the SPC method, are the closest to the filed measurement by
346 the stem diameter tape, followed by the SP method, and the CYF and CF methods again; (2) the
347 measurement results between the SPC and CSM methods are very close, it means that the equivalency
348 between the diameter tape and the caliper is satisfied; (3) the stem points preprocessing step is a very
349 important step for stem diameter retrieval, enriching the content of this step (such as stem cross-section
350 completeness checking, stem cross-section validity verification and outliers removal) is helpful to
351 improve the accuracy of stem diameter retrieval, especially for sample plot scanning.
352 Author Contributions: Conceptualization, software and writing–original draft preparation, Lei You.; data
353 processing, software, validation and investigation, Xincheng Ai and Jie Wei.; investigation, writing–review and
354 editing, Yong Pang.; writing–review and editing, Xinyu Song. All authors have read and agreed to the published
355 version of the manuscript.
356 Funding: This research was funded in part by the National Natural Science Foundation of China (No.31872704
357 and 61761003), the National Key Research and Development Program (No.2017YFD0600404), the Foundation for
Version December 25, 2020 submitted to Remote Sens. 13 of 14
358 Distinguished Young Talents in Higher Education of Henan (No.2020GGJS157), and the Nanhu Scholars Program
359 for Young Scholars of XYNU.
360 Conflicts of Interest: The authors declare no conflict of interest.
361 References
362 1. Pueschel, P. The influence of scanner parameters on the extraction of tree metrics from FARO Photon 120
363 terrestrial laser scans. ISPRS Journal of Photogrammetry and Remote Sensing 2013, 78, 58–68.
364 2. Liang, X.; Hyyppä, J.; Kaartinen, H.; Lehtomäki, M.; Pyörälä, J.; Pfeifer, N.; Holopainen, M.; Brolly, G.;
365 Francesco, P.; Hackenberg, J.; Huang, H.; Jo, H.W.; Katoh, M.; Liu, L.; Mokros, M.; Morel, J.; Olofsson, K.; ˘
366 Poveda-Lopez, J.; Trochta, J.; Wang, D.; Wang, J.; Xi, Z.; Yang, B.; Zheng, G.; Kankare, V.; Luoma, V.; Yu, X.;
367 Chen, L.; Vastaranta, M.; Saarinen, N.; Wang, Y. International benchmarking of terrestrial laser scanning
368 approaches for forest inventories. ISPRS Journal of Photogrammetry and Remote Sensing 2018, 144, 137 – 179.
369 3. Cabo, C.; Ordóñez, C.; López-Sánchez, C.A.; Armesto, J. Automatic dendrometry: Tree detection, tree height
370 and diameter estimation using terrestrial laser scanning. International Journal of Applied Earth Observation and
371 Geoinformation 2018, 69, 164 – 174.
372 4. Ritter, T.; Schwarz, M.; Tockner, A.; Leisch, F.; Nothdurft, A. Automatic Mapping of Forest Stands Based on
373 Three-Dimensional Point Clouds Derived from Terrestrial Laser-Scanning. Forests 2017, 8.
374 5. Xia, S.; Wang, C.; Pan, F.; Xi, X.; Zeng, H.; Liu, H. Detecting Stems in Dense and Homogeneous Forest Using
375 Single-Scan TLS. Forests 2015, 6, 3923–3945.
376 6. Ravaglia, J.; Fournier, R.A.; Bac, A.; Véga, C.; Côté, J.F.; Piboule, A.; Rémillard, U. Comparison of
377 Three Algorithms to Estimate Tree Stem Diameter from Terrestrial Laser Scanner Data. Forests 2019,
378 10. doi:10.3390/f10070599.
379 7. Pitkänen, T.P.; Raumonen, P.; Kangas, A. Measuring stem diameters with TLS in boreal forests by
380 complementary fitting procedure. ISPRS Journal of Photogrammetry and Remote Sensing 2019, 147, 294 –
381 306.
382 8. You, L.; Tang, S.; Song, X.; Lei, Y.; Zang, H.; Lou, M.; Zhuang, C. Precise Measurement of Stem Diameter
383 by Simulating the Path of Diameter Tape from Terrestrial Laser Scanning Data. Remote Sensing 2016, 8, 717.
384 doi:10.3390/rs8090717.
385 9. Srinivasan, S.; Popescu, S.C.; Eriksson, M.; Sheridan, R.D.; Ku, N.W. Terrestrial Laser Scanning as an Effective
386 Tool to Retrieve Tree Level Height, Crown Width, and Stem Diameter. Remote Sensing 2015, 7, 1877–1896.
387 10. Kankare, V.; Liang, X.; Vastaranta, M.; Yu, X.; Holopainen, M.; Hyyppä, J. Diameter distribution estimation
388 with laser scanning based multisource single tree inventory. ISPRS Journal of Photogrammetry and Remote
389 Sensing 2015, 108, 161–171.
390 11. Hunˇcaga, M.; Chudá, J.; Tomaštík, J.; Slámová, M.; Kore ˇn, M.; Chudý, F. The Comparison of Stem Curve
391 Accuracy Determined from Point Clouds Acquired by Different Terrestrial Remote Sensing Methods. Remote
392 Sensing 2020, 12. doi:10.3390/rs12172739.
393 12. Liang, X.; Kankare, V.; Yu, X.; Hyyppä, J.; Holopainen, M. Automated Stem Curve Measurement Using
394 Terrestrial Laser Scanning. IEEE Transactions on Geoscience and Remote Sensing 2014, 52, 1739–1748.
395 13. Luoma, V.; Saarinen, N.; Wulder, M.A.; White, J.C.; Vastaranta, M.; Holopainen, M.; Hyyppä, J. Assessing
396 Precision in Conventional Field Measurements of Individual Tree Attributes. Forests 2017, 8.
397 14. Ninni Saarinen, V.K.; Vastaranta, M.; Luoma, V.; Pyörälä, J.; Tanhuanpää, T.; Liang, X.; Kaartinen, H.; Kukko,
398 A.; Jaakkola, A.; Yu, X.; Holopainen, M.; Hyyppä, J. Feasibility of Terrestrial laser scanning for collecting stem
399 volume information from single trees. ISPRS Journal of Photogrammetry and Remote Sensing 2017, 123, 140 –
400 158.
401 15. Nölke, N.; Fehrmann, L.; I Nengah, S.; Tiryana, T.; Seidel, D.; Kleinn, C. On the geometry and allometry of
402 big-buttressed trees-a challenge for forest monitoring: new insights from 3D-modeling with terrestrial laser
403 scanning. iForest-Biogeosciences and Forestry 2015, 8, 574.
404 16. Liang, X.; Kankare, V.; Hyyppä, J.; Wang, Y.; Kukko, A.; Haggrën, H.; Yu, X.; Kaartinen, H.; Jaakkola, A.;
405 Guan, F.; Holopainen, M.; Vastaranta, M. Terrestrial laser scanning in forest inventories. ISPRS Journal of
406 Photogrammetry and Remote Sensing 2016, 115, 63–77.
407 17. Kachamba, D.J.; Eid, T. Total tree, merchantable stem and branch volume models for miombo woodlands of
408 Malawi. Southern Forests: a Journal of Forest Science 2016, 78, 41–51. doi:10.2989/20702620.2015.1108615.
Version December 25, 2020 submitted to Remote Sens. 14 of 14
409 18. Kore ˇn, M.; Hunˇcaga, M.; Chudá, J.; Mokroš, M.; Surový, P. The Influence of Cross-Section Thickness on
410 Diameter at Breast Height Estimation from Point Cloud. ISPRS International Journal of Geo-Information 2020,
411 9. doi:10.3390/ijgi9090495.
412 19. Simonse, M.; Aschoff, T.; Spiecker, H.; Thies, M. Automatic determination of forest inventory parameters
413 using terrestrial laserscanning. Proceedings of the scandlaser scientific workshop on airborne laser scanning
414 of forests, 2003, Vol. 2003, pp. 252–258.
415 20. Olofsson, K.; Holmgren, J.; Olsson, H. Tree Stem and Height Measurements using Terrestrial Laser Scanning
416 and the RANSAC Algorithm. Remote Sensing 2014, 6, 4323–4344.
417 21. Gao, S. Branches modeling and Morphological Parameters Extraction Technology Based on 3D Laser
418 Scanning Technology. Thesis, Chinese Academy of Forestry, 2013.
419 22. You, L.; Guo, J.; Pang, Y.; Tang, S.; Song, X.; Zhang, X. 3D stem model construction with geometry
420 consistency using terrestrial laser scanning data. International Journal of Remote Sensing 2021, 42, 714–737.
421 doi:10.1080/01431161.2020.1811919.
422 23. Van Laar, A.; Akca, A. Forest mensuration; Vol. 13, Springer Science & Business Media, 2007.
423 24. Binot, J.M.; Pothier, D.; Lebel, J. Comparison of relative accuracy and time requirement between the
424 caliper, the diameter tape and an electronic tree measuring fork. THE FORESTRY CHRONICLE 1995, 71.
425 doi:10.5558/tfc71197-2.
426 25. Tang, S. The theoretical comparison of measuring diameter using caliper and stem diameter tape. Forestry
427 resource management 1977, pp. 23–26.
428 26. FARO Technologies, I. FARO Focus3D X330. https://www.faro.com/en-gb/news/the-new-faro-laser-sca
429 nner-focus3d-x-330-the-perfect-instrument-for-3d-documentation-and-land-surveying-2/, 2013.
430 27. Eberly, D. Geometric Tools Engine 5.1. https://www.geometrictools.com/GTE/Mathematics/ApprCylin
431 der3.h, 2020.
432 28. Piegl, L.; Tiller, W. The NURBS book; Springer-Verlag: Berlin, 1997.
433 29. You, L.; Feng, Y.; Guo, J.; Ye, J.; Tang, S.; Song, X. Construction of Closed, Global Convexity
and G
2
434 Continutiy Curve. Journal of Computer-Aided Design & Computer Graphics 2017, 30, 309–315.
435 doi:10.3724/SP.J.1089.2017.16604.
436 30. Pfeifer, N.; Winterhalder, D. Modelling of tree cross sections from terrestrial laser scanning data with
437 free-form curves. International Archives of Photogrammetry, remote sensing and spatial information sciences 2004,
438 36, W2.
439 31. Nurunnabi, A.; Sadahiro, Y.; Lindenbergh, R.; Belton, D. Robust cylinder fitting in laser scanning point cloud
440 data. Measurement 2019, 138, 632–651.
441 32. Nurunnabi, A.; Sadahiro, Y.; Lindenbergh, R.C. Robust cylinder fitting in three-dimensional point cloud
442 data. Isprs Hannover Workshop, 2017.
443 33. Wang, D.; Kankare, V.; Puttonen, E.; Hollaus, M.; Pfeifer, N. Reconstructing stem cross section shapes from
444 terrestrial laser scanning. IEEE Geoscience and Remote Sensing Letters 2017, 14, 272–276.
445 34. Stovall, A.E.; Vorster, A.G.; Anderson, R.S.; Evangelista, P.H.; Shugart, H.H. Non-destructive aboveground
446 biomass estimation of coniferous trees using terrestrial LiDAR. Remote Sensing of Environment 2017, 200, 31 –
447 42. doi:10.1016/j.rse.2017.08.013.
448 
c 2020 by the authors. Submitted to Remote Sens. for possible open access publication
449 under the terms and conditions of the Creative Commons Attribution (CC BY) license
450 (http://creativecommons.org/licenses/by/4.0/).