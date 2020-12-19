---
title: '一次数据清洗的记录'
date: 2020-10-04
published: true
slug: 9-data-clean
tags: ['总结']
cover_image: "./images/p9.jpg"
canonical_url: false
description: '最近在清洗一批数据，大约有一千多个文件。'
---

# 1.0 背景

最近在清洗一批数据，大约有一千多个文件。如果手动处理，一个文件最快的话需要一分钟。但是这个状态需要精神高度集中，手速不能慢，无法持续太久。

数据的形状如下：

![](https://gitee.com/weijiew/pic/raw/master/img/p9-1.png)

![](https://gitee.com/weijiew/pic/raw/master/img/p9-2.png)

需要清除的点则是：

![](https://gitee.com/weijiew/pic/raw/master/img/p9-3.png)

圆柱体周围有很多噪点，需要清除。

# 2.0 思考

我最初的想法步骤如下：

1. 根据坐标得到圆柱体的的中心。
2. 根据 Z 轴得到圆柱体的高的范围，根据范围将圆柱体分为多层。
3. 抽出每一层，极限的思想，想象成一个圆环（虽然高存在，这也是误差的一部分）。
4. 根据 Y 轴将圆切成两半，扫描完一半在扫描另一半。
5. 根据 X 轴的范围来切分数据，将数据分为多多份。
6. 此时存在多份数据，计算每一份数据距离中心的均值，将和均值差别较大的删除。

误差来源：

1. 因为高的存在，所有以根据三角函数，剔除的数据需要设定一个合理范围，范围大了删不掉异常值，范围小了会将圆环的上下部分删掉。
2. 因为是无差别的切分，所以异常点单独为一份无法计算均值剔除，这也是误差最严重的部分！
3. 写代码的时候存在一个 python 返回值的问题，索引方面的问题。
4. 在我手动清理的时候发现数据范围差别较大，即时调试好参数也只是在当前的数据上表现良好。

”残废“代码如下：

```python
import os
import math
import pandas as pd
import numpy as np

# 输入文件所在路径批量修改文件后缀

# txt 转 csv
def txt_to_csv(txtFile,csvFile):
    d_read = pd.read_csv(txtFile, index_col=False, skiprows=2, sep=' ')
    d_write = d_read.replace(' ', ',')
    d_write.to_csv(csvFile, index=False)

# vtx => txt  
def modifyFile(path):
    for filename in os.listdir(path):
        os.rename(os.path.join(path, filename), os.path.join(path, str(filename[:-4])) + ".txt")
        txt_to_csv(os.path.join(path, str(filename[:-4])) + ".txt",os.path.join(path, str(filename[:-4])) + ".csv")

def readPath(path):
    for p in os.listdir(path):
        fullPath = path + "\\"+ p 
        modifyFile(fullPath)

def calZData(df):
    # 计算 Z 轴最值，
    z_max, z_min = df.iloc[:, 2].max(), df.iloc[:, 2].min()
    index = np.arange(z_min, z_max, 0.5)
    i = 1
    delFrame = []
    while i < len(index):
        tmpData = df.loc[(index[i-1] <= df.iloc[:, 2]) & (df.iloc[:, 2] < index[i])]
        delData1 = calXData(tmpData)
        delFrame = pd.concat(delFrame,delData1)
        i += 1
        print("已经删除一圈啦！！！")
    return delFrame

def calXData(df_z):

    # 切分 X 轴
    x_max, x_min = df_z.iloc[:, 0].max(), df_z.iloc[:, 0].min()
    index = np.arange(x_min, x_max, 2)

    i = 1
    frame = []
    while i < len(index):
        
        # 处理 Y 轴
        midValue =  df_z.iloc[:,1].mean()
        
        df_xy = df_z.loc[(df_z.iloc[:, 0] >= index[i - 1]) &
                    (df_z.iloc[:, 0] < index[i]) &
                    (df_z.iloc[:,1] > midValue)]

        df_xy = df_z.loc[(df_z.iloc[:, 0] >= index[i - 1]) &
                    (df_z.iloc[:, 0] < index[i]) &
                    (df_z.iloc[:,1] <= midValue)]
        
        del1 = delData(df_xy)
        del2 = delData(df_xy)
        frame = [del1, del2]
        i += 1
    frame = pd.concat(frame)
    return frame

def delData(df_xy):
    global x_mean, y_mean, z_mean, df

    val = np.sqrt(np.square((df_xy.iloc[:, 0]) - (x_mean)) +
                  np.square((df_xy.iloc[:, 1]) - (y_mean)) +
                  np.square((df_xy.iloc[:,2]) - (z_mean)))

    val_mean = np.mean(val)
    
    delIndex = val.loc[((val / val_mean) < 0.95) | ((val / val_mean) > 1.05)]
    print("已经删除一块啦！")
    if np.isnan(delIndex) == False:
        return delIndex
    


if __name__ == "__main__":
    filePath = "F:\\DA\\test\\1728_002_Stem_Data(Undo)\\1728_002_Stem_Data_0.csv"
    df = pd.read_csv(filePath)
    x_mean , y_mean, z_mean = df.iloc[:,0].mean(), df.iloc[:,1].mean(), df.iloc[:,2].mean()
    print("Delete data bofore ：", len(df))
    delIndexFrmae = calZData(df)
    df_c = df.drop(delIndexFrmae)
    print("Delete data after  ：", len(df_c))
```

# 3.0 总结

通过这次失败的尝试，学习了 python 的数据格式转换。

python 对于 vtx 数据格式的文件没有很好的处理库，通过修改文件名的方式我将其转为了 txt 格式。

采用 pandas 将 txt 格式的数据转为了 csv 格式，然后处理 csv 数据。

pandas 的 drop 函数在删数据的时候是传值，而我需要的则是传址，pandas 中没有找到一个合适的 API 。 del 似乎可以解决，当我意识到误差无法避免时就没有接着处理这个问题了。

再次学习了 pandas 的条件索引，确实非常强大，不需要循环就可以搞定！

最近粗读了一下《python 数据分析》，pandas 作者出的书，有一些帮助。