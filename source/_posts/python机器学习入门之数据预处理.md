---
title: python机器学习入门之数据预处理
categories:
  - 机器学习
tags:
  - Python
  - 机器学习
  - 数据分析
date: 2019-04-16 17:18:19
---

##   python机器学习入门之数据预处理 
***
### 机器学习的流程
#### 数据处理阶段

1. 数据采集   
2. 数据清洗  

#### 训练模型阶段  

3. 数据预处理  
4. 选择模型（算法）  
5. 验证模型  
6. 训练模型  
7. 测试模型（模型不合适 -> 选择模型 或 训练模型）  

#### 实际业务场景  

8. 使用模型  
9. 维护和升级  

### 为什么使用 Python 进行机器学习  
#### Python 更专注于解决现实问题
Python 提高了大量数据处理（ NumPy、Pandas ）、机器学习（ Scikit_learn ）及深度学习（ TensorFlow、PyTorch 等）相关的依赖库，使用者可以更加专注算法的实现而不用从头开始实现基本算法。

#### Python 很慢？
Python 确实很慢，但是可以把计算量大的部分交由其他更高效（但更难使用）的语言（如 C 和 C++ ）进行处理。实际运用中，几乎所有程序都会使用 NumPy 去完成计算，所有没有必要担心程序的运行速度。  
NumPy 与原生 Python 的比较：

```
import numpy as np
from datetime import datetime
# 原生python
n = 100000
start = datetime.now()
A, B = [], []
for i in range(n):
    A.append(i ** 2)
    B.append(i ** 3)
C = []
for a, b in zip(A, B):
    C.append(a + b)
t1 = (datetime.now() - start).microseconds

# numpy
start = datetime.now()
A, B = np.arange(n) ** 2, np.arange(n) ** 3
C = A + B
t2 = (datetime.now() - start).microseconds

print(f'{t1/t2}倍')
# 大约是20倍
```

### 数据预处理
#### 均值移除（列处理）  
通过调整令样本矩阵中每一列（特征）的平均值为 0 ，标准差为 1 。  这样一来，所有特征对最终模型的预测结果都有接近一致的贡献，模型对每个特征的倾向性更加均衡。  

假设样本为 $[a, b, c]$
均值：$m=\frac{(a+b+c)}{3}$
标准差：$s=\sqrt{\frac{(a-m)^2+(b-m)^2+(c-m)^2}{3}}$
均值 -> 0：$a^{'}=a-m$ , $b^{'}=b-m$ , $c^{'}=c-m$
标准差 -> 1：$a^{"}=\frac{a^{'}}{s}$ , $b^{"}=\frac{b^{'}}{s}$ , $c^{"}=\frac{c^{'}}{s}$
得到标准样本 $[a^{"}, b^{"}, c^{"}]$

```
import numpy as np
raw_samples = np.array([
    [3, -1.5,  2,   -5.4],
    [0,  4,   -0.3,  2.1],
    [1,  3.3, -1.9, -4.3]])

# numpy
# 均值移除
std_samples = raw_samples.copy()
for col in std_samples.T:
    col_mean = col.mean()
    col_std = col.std()
    col -= col_mean
    col /= col_std
print(std_samples)
print(std_samples.mean(axis=0))
print(std_samples.std(axis=0))

# sklearn
# 均值移除
import sklearn.preprocessing as sp
std_samples = sp.scale(raw_samples)
print(std_samples)
print(std_samples.mean(axis=0))
print(std_samples.std(axis=0))
```
输出结果

```
# std_samples
[[ 1.33630621 -1.40451644  1.29110641 -0.86687558]
 [-1.06904497  0.84543708 -0.14577008  1.40111286]
 [-0.26726124  0.55907936 -1.14533633 -0.53423728]]

# std_samples.mean(axis=0)
[0. 0. 0. 0.]

# std_samples.std(axis=0)
[1. 1. 1. 1.]
```

#### 范围缩放（列处理）
将样本矩阵每一列的元素经过线性变换，使得所有列的元素都处在同样的范围区间内。  
样本矩阵中每一列的最大值和最小值为某个给定的值 ([0, 1]) ，其他元素线性变换。

变换原理： $y = kx + b$
最小值：$y_{min} = kx_{min} + b$
最大值：$y_{max} = kx_{max} + b$
矩阵运算： $ \begin{vmatrix} x_{min} & 1 \\ x_{max} & 1 \\ \end{vmatrix} * \begin{vmatrix} k \\ b \\ \end{vmatrix} = \begin{vmatrix} y_{min} \\ y_{max} \\ \end{vmatrix}$

```
import numpy as np
raw_samples = np.array([
    [3, -1.5,  2,   -5.4],
    [0,  4,   -0.3,  2.1],
    [1,  3.3, -1.9, -4.3]
])

# 最小值,最大值
print(raw_samples.min(axis=0)) # [0. -1.5 -1.9 -5.4]
print(raw_samples.max(axis=0)) # [3.  4.   2.   2.1]

# numpy
# 范围缩放
mms_samples = raw_samples.copy()
for col in mms_samples.T:
    col -= col.min()
    col /= col.max()
print(mms_samples)
print(mms_samples.min(axis=0))
print(mms_samples.max(axis=0))

# sklearn
# 范围缩放
import sklearn.preprocessing as sp
mms = sp.MinMaxScaler(feature_range=(0, 1))  # 缩放器对象
mms_samples = mms.fit_transform(raw_samples)
print(mms_samples)
print(mms_samples.min(axis=0))
print(mms_samples.max(axis=0))

```
输出结果

```
# mms_samples
[[1.         0.         1.         0.        ]
 [0.         1.         0.41025641 1.        ]
 [0.33333333 0.87272727 0.         0.14666667]]

# mms_samples.min(axis=0)
[0. 0. 0. 0.]

# mms_samples.max(axis=0)
[1. 1. 1. 1.]
```

#### 归一化（行处理）  
用每个样本各个特征值除以该样本所有特征值绝对值之和，使得的处理后的样本矩阵中各行所有元素的绝对值之和为 1 。

|-|Python|C++  |Java|PHP|SUM
|-|------|-----|----|---|-
|处理前|20    |30   |40  |10 |100
|处理后|0.2   |0.3  |0.4 |0.1|1

```
import numpy as np
raw_samples = np.array([
    [3, -1.5,  2,   -5.4],
    [0,  4,   -0.3,  2.1],
    [1,  3.3, -1.9, -4.3]])

# numpy
# 归一化
nor_samples = raw_samples.copy()
for row in nor_samples:
    row /= np.abs(row).sum()
print(nor_samples)
print(abs(nor_samples).sum(axis=1))

# sklearn
# 归一化
import sklearn.preprocessing as sp
nor_samples = sp.normalize(raw_samples, norm='l1')
# norm='ln' 为 ln范数，表示列元素的绝对值的n次方之和
print(nor_samples)
print(abs(nor_samples).sum(axis=1))
```
输出结果

```
# nor_samples
[[0.25210084 -0.12605042  0.16806723 -0.45378151]
 [0.          0.625      -0.046875    0.328125  ]
 [0.0952381   0.31428571 -0.18095238 -0.40952381]]

# abs(nor_samples).sum(axis=1)
[1. 1. 1.] 
```
#### 二值化
根据事先给定阈值，将样本矩阵中高于阈值的元素设置为 1 ，否则设置为 0 ，得到一个完全由 1 和 0 组成的二值矩阵。

```
import numpy as np
raw_samples = np.array([
    [3, -1.5,  2,   -5.4],
    [0,  4,   -0.3,  2.1],
    [1,  3.3, -1.9, -4.3]
])

bin_samples = raw_samples.copy()

# numpy
# 二值化 -> 掩码
# 阈值为1.4
bin_samples[bin_samples <= 1.4] = 0
bin_samples[bin_samples > 1.4] = 1
print(bin_samples)

# sklearn
# 二值化
import sklearn.preprocessing as sp
bin = sp.Binarizer(threshold=1.4)  # 二值化对象，阈值为1.4
bin_samples = bin.transform(raw_samples)
print(bin_samples)
```
输出结果

```
# bin_samples
[[1. 0. 1. 0.]
 [0. 1. 0. 1.]
 [0. 1. 0. 0.]]
```
#### 独热编码  
用一个只包含一个 1 和若干个 0 的序列来表达每个特征值的编码方式，借此既保留了样本矩阵的所有细节，同时又得到一个只含有 1 和 0 的稀疏矩阵，既可以提高模型的容错性，同时还能节省内存空间。

处理前|a|b|c|处理后|a|b|c
-|-|-|-|-|-|-|-|-
A|1|2|0|A|10|100|1000
B|4|5|3|B|01|010|0100
C|1|8|6|C|10|001|0010
D|4|2|9|D|01|100|0000

映射规则

a|b|c  
-|-|-  
1-> 10|2-> 100|0-> 1000
4-> 01|5-> 010|3-> 0100  
-|8-> 001|6-> 0010
-|-|9-> 0001

```
import numpy as np
raw_samples = np.array([
    [1, 2, 0],
    [4, 5, 3],
    [1, 8, 6],
    [4, 2, 9]
])

# 建立编码字典列表
code_tables = []
for col in raw_samples.T:
    # 针对一列的编码字典
    code_table = {}
    for val in col:
        code_table[val] = None
    code_tables.append(code_table)

# 为编码字典列表中每个编码字典添加值
for code_table in code_tables:
    size = len(code_table)
    for one, key in enumerate(sorted(code_table.keys())):
        code_table[key] = np.zeros(shape=size, dtype=int)
        code_table[key][one] = 1

# 根据编码字典表对原始样本矩阵做独热编码
ohe_samples = []
for raw_sample in raw_samples:
    ohe_sample = np.array([], dtype=int)
    # i: 原始样本的列数, key: 编码字典中的编码值
    for i, key in enumerate(raw_sample):
        ohe_sample = np.hstack((ohe_sample, code_tables[i][key]))
    ohe_samples.append(ohe_sample)
# 编码完成
ohe_samples = np.array(ohe_samples)
print(ohe_samples)

# sklearn
# 独热编码
import sklearn.preprocessing as sp
ohe = sp.OneHotEncoder(sparse=False, dtype=int)  # 独热编码器对象
ohe_samples = ohe.fit_transform(raw_samples)
print(ohe_samples)
# spares = False, 记录0和1,输出原数组
[[1 0 1 0 0 1 0 0 0]
 [0 1 0 1 0 0 1 0 0]
 [1 0 0 0 1 0 0 1 0]
 [0 1 1 0 0 0 0 0 1]]

# sparse = True, 只记录1的索引,使用内存空间较少
(0, 5)        1
(0, 2)        1
(0, 0)        1
(1, 6)        1
(1, 3)        1
(1, 1)        1
(2, 7)        1
(2, 4)        1
(2, 0)        1
(3, 8)        1
(3, 2)        1
(3, 1)        1
```
#### 标签编码  
标签编码即是将文本形式的特征值转换为数值形式的特征值，其编码数值源于标签字符串的字典排序，与标签本身的含义无关。

语言（文本形式）|编码（数值形式）
-|-
C|0
Java|1
PHP|2
Python|3

```
import numpy as np
import sklearn.preprocessing as sp

# 原始样本
raw_samples = np.array(
    ['C', 'Java', 'C', 'Python','Java', 'PHP', 'Python', 'PHP']
)

# 标签编码器，编码时会记录标签和编码的映射，用于反查
lbe = sp.LabelEncoder() 

# 标签编码
lbe_samples = lbe.fit_transform(raw_samples)
print(lbe_samples)
[0 1 0 3 1 2 3 2]

# 反查
raw_samples = lbe.inverse_transform(lbe_samples)
print(raw_samples)
['C' 'Java' 'C' 'Python' 'Java' 'PHP' 'Python' 'PHP']
```

### 总结
数据预处理是机器学习训练模型阶段的起点。通过数据预处理，我们可以得到理想的用于训练和测试模型的数据集，之后就可以开始训练算法模型了。  
希望这篇文章对于大家入门机器学习能有一定的帮助！
