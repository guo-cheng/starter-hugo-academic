---
title: 生物信息学入门课：学习生信你需要了解的统计学
subtitle: 这篇学习笔记主要参考北大生科院孟浩巍博士在腾讯课堂开设的课程(十分推荐学习)

# Summary for listings and search engines
summary: 这篇学习笔记主要参考北大生科院孟浩巍博士在腾讯课堂开设的课程(十分推荐学习)

# Link this post with a project
projects: [Data Science]

# Date published
date: "2021-2-3T00:00:00Z"

# Date updated
lastmod: "2020-2-3T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: ''
  focal_point: ""
  placement: 2
  preview_only: false

authors:
- admin

tags:
- 生物信息
- 统计

categories:
- 统计学
- 教程
---

# 统计学习

这篇学习笔记主要参考北大生科院孟浩巍博士在腾讯课堂开设的课程(十分推荐学习)：

> 生物信息学入门课：学习生信你需要了解的统计学

本篇笔记截止到多重假设检验的矫正，过几天再更新余下内容。

**课程主要目录及内容**
**统计学基础知识：**

- 什么是概率？
- 什么是随机变量？
- 概率密度函数及累积分布函数；
- boxplot与histogram；
- 最重要的若干分布（离散+连续）；
  - 二项分布；
  - 泊松分布；
  - 负二项分布；
  - 几何分布；
  - 超几何分布；
  - 指数分布；
  - 正态分布；
  - Gamma分布；

**统计检验相关：**

- 什么是假设检验？
  - 女士品茶与Pearson拟合优度检验；
  - P value是什么意思？怎么计算？
  - 什么是第1类错误，什么是第2类错误？
  - P value使用常见错误有哪些？
  - 什么是统计检验的功效？
- t检验专题；
  - 为什么要用t检验？
  - t检验的前提是什么？
  - 如何进行配对t检验？
- 列联表检验问题
  - Fisher exact test和卡方检验；
  - 从2×2到m×n的推广；

**其他内容：**

- 多重检验的矫正问题；
  - 什么是P value什么是Q Value？
- 方差分析；
- 简单的回归分析；
  - 什么时候要用Pearson相关系数，什么时候用Spearman相关系数？
  - 什么是决定系数？
  - 回归分析中的P Value到底是怎么回事？
- 比较重要的非参检验方法；

## 随机变量与概率

### 随机变量和随机试验

> 随机试验的特点是：所有可能已知，但试验前结果未知。
>
> 𝑋是随着试验的结果的不同而变化的，也即它是样本点的一个函数，这个量称为随机变量。

### 离散型随机变量和连续型随机变量

> 随机变量所有可能取的值为有限个或至多可列个，即我们能把其可能的结果一一列举出来。
>
> 全部可能取值不仅无穷多，而且还不能一一列举，而是充满一个区间。

### 概率的公理化定义

- 经过大量试验，用频率代替概率。

> 1. 0 ≤ P(A) ≤ 1
> 2. P(S) = 1
> 3. 若A~1~，A~2~, … , A~k~；两两互不相容，则![cFBOPo](https://cdn.liguocheng.top//uPic/cFBOPo.png)
>
> 若满足上述3点，则P(A) 称为事件A的概率。
>
> P(H) = P(T) 
>
> P(H) + P(T) = 1 
>
> 可解：P(H) = 0.5

### 随机变量的概率分布律与概率密度函数

- 离散型随机变量

![image-20210705163125383](https://cdn.liguocheng.top//uPic/image-20210705163125383.png)

- 离散型随机变量

![image-20210705163213763](https://cdn.liguocheng.top//uPic/image-20210705163213763.png)

### 概率分布函数

- 离散型随机变量和离散型随机变量都可以求概率分布函数F(X)。

![image-20210705163345612](https://cdn.liguocheng.top//uPic/image-20210705163345612.png)

![image-20210705163509873](https://cdn.liguocheng.top//uPic/image-20210705163509873.png)

## 常见离散随机分布

### 二项分布

> 假设一次试验中，事件A出现的概率为p。现进行n次试验，令X = 事件A出现的次数，则X服从二项分布。

![image-20210705222237260](https://cdn.liguocheng.top//uPic/image-20210705222237260.png)

- 重要性质：

1. 固定n, p时，b(k; n, p)先随k增加而增大，达到某一极大值后又逐渐下降。

2. 使b(k; n, p)取最大值的项b(m; n, p)称为b(k; n, p) 的中心项，而m称为最可能成功次数。

3. 固定n时，p越与1/2接近时，分布越接近对称。

- 生物信息学应用：

在基因组上进行mutation判断的时候，最简单的办法就是利用二项分布进行检验。

1. 计算genome-wide的所有突变率当做参数p

2. 计算某一个特定位点突变reads数目k和总reads数目n

3. 可以计算出一个在这种情况下满足条件的pvalue

4. 最终判断该位置是否为mutation，还是随机突变造成的。

> 二项分布缺点：计算量巨大。所以，通常情况下，在n很大，p很小，np不大的时候用泊松分布进行拟合，效果非常好。
>
> 一般p < 0.1; np < 10

### 泊松分布

若把在每次试验中出现概率很小的事件称作稀有事件,泊松分布可用来描述一定时间内稀有事件出现的次数.

- 全基因组测序，比对到某一个区域内的reads count

- RNA-Seq测序，1个gene内比对到的reads count

若随机变量X可取一切非负整数值，且相应的概率分布为：

![image-20210706095713855](https://cdn.liguocheng.top//uPic/image-20210706095713855.png)

#### 泊松定理

泊松定理表明，泊松分布是二项分布的极限分布，当n比较大，p比较小(一般p要求小于0.1)时，二项分布就可近似地看成是参数为np的泊松分布。

![image-20210706100124560](https://cdn.liguocheng.top//uPic/image-20210706100124560.png)

```R
n = 100000
p = 10^(-4)

# use binomial
pbinom(0,size = n,prob = p)
pbinom(1,size = n,prob = p)
pbinom(2,size = n,prob = p)

# use possion
ppois(0,lambda = n*p)
ppois(1,lambda = n*p)
ppois(2,lambda = n*p)
```

### 超几何分布

对N件产品（其中M 件次品）进行不放回抽样检查，现随机抽出n件产品，则这n件产品中出现的次品数服从超几何分布。

![image-20210706100626673](https://cdn.liguocheng.top//uPic/image-20210706100626673.png)

超几何分布以二项分布为极限分布。即二项分布是有放回的抽样，超几何分布是无放回的抽样。Fisher Exact Test就是利用超几何分布检验。

### 几何分布

在事件A发生的概率为p的重复伯努利试验(二项分布)中，若以X记A首次成功出现时的试验次数，则X服从几何分布,简记X~G(p)。

![image-20210706100828873](https://cdn.liguocheng.top//uPic/image-20210706100828873.png)

> q = (1 - p)

![image-20210706100914535](https://cdn.liguocheng.top//uPic/image-20210706100914535.png)

#### 几何分布的无记忆性

现在假定已知在前m次试验中没有出现成功，那么为了达到首次成功所再需要的等待时间与前面的失败次数m无关。

![image-20210706101046754](https://cdn.liguocheng.top//uPic/image-20210706101046754.png)

在离散型分布中，只有几何分布才具有无记忆性。

### 负二项分布(帕斯卡分布)

在成功的概率为p的伯努利试验中，若以X记第r次成功出现时的试验次数，则X服从参数为(r, p)的负二项分布（帕斯卡分布），X可取的值为r, r+1，…

![image-20210706101312469](https://cdn.liguocheng.top//uPic/image-20210706101312469.png)

![image-20210706103534882](https://cdn.liguocheng.top//uPic/image-20210706103534882.png)

#### Reads Count的泊松分布估计

Reads Count在genome上的分布，常认为是服从泊松分布的。主要理由是：genome很大，reads随机落到genome上任意一个位置是一个小概率的事情，因此可以认为服从泊松分布。

MACS2认为reads count服从泊松分布；Cuffdiff1 也认为RNA-Seq gene区的reads count服从泊松分布。

- 缺点：

泊松分布只有1个参数λ，且期望，方差都是λ；但实际问题中，我们不能保证随机抽样的结果就符合期望和方差相等。

#### Reads Count负二项分布修正

若X服从负二项分布，那么：

E(X)=μ

Var(X)=μ + μ2/𝑘

其中，k常被我们成为散度（dispersion），通过散度k的修正，能够帮助我们更好地进行统计检验。

目前，Cuffdiff2，DESeq2等基因差异表达检测的软件都已经在使用负二项分布进行参数估计。

#### Lander Waterman曲线

假设Genome长度为G；reads测序长度为L；总reads数目为N；测序乘数X = LN / G

那么在进行拼装的时候，如果两条reads重叠的区域超过Tbp，或者是用重叠比a = T / L来表示，那么能够拼成的contig数目M服从Lander Waterman曲线：

M= N𝑒^−(1−𝑎)𝑋^=𝐺/𝐿(X𝑒^−(1−𝑎)𝑋^)

## 常见连续随机分布

### 指数分布

![image-20210706110219758](https://cdn.liguocheng.top//uPic/image-20210706110219758.png)

- 指数分布也无记忆性。

### 伽马分布

![image-20210706110353116](https://cdn.liguocheng.top//uPic/image-20210706110353116.png)

- Gamma分布常用来作为某个事件总共出现N次的等待时间
- Gamma分布可以理解成负二项分布的连续推广；同理指数分布可以认为是几何分布的极限推广

### 正态分布(误差分布)

![image-20210706110702838](https://cdn.liguocheng.top//uPic/image-20210706110702838.png)



![image-20210706110825695](https://cdn.liguocheng.top//uPic/image-20210706110825695.png)

#### 正态分布的3 sigma 法则

![image-20210706110847699](https://cdn.liguocheng.top//uPic/image-20210706110847699.png)

#### Boxplot

![YWY7KR](https://cdn.liguocheng.top//uPic/YWY7KR.jpg)

#### 正态分布性质

1.正态分布是自然界最常见的一种分布，例如测量的误差、炮弹弹落点的分布、人的生理特征的尺寸（身高，体重等）、农作物的收获量等都近似服从正态分布。

2.一般说来，若影响某一数量指标的随机因素很多，而每个因素所起的作用不太大，则这个指标服从正态分布。

3.正态分布具有许多良好的性质，许多分布可用正态分布来近似，另外一些分布又可以通过正态分布来导出。

> 大数定律：随机变量的平均值能够收敛到期望值
>
> 中心极限定理：大量随机变量的加和之后的分布在某些条件下逼近正态分布

![KAm4Fd](https://cdn.liguocheng.top//uPic/KAm4Fd.jpg)

> 概率是频率随样本趋于无穷的极限
>
> 期望是平均数随样本趋于无穷的极限

## 假设检验

1. K. Pearson(卡尔·皮尔逊)

- 提出拟合优度检验

- 拟合卡方统计量的极限定理

2. SirRonald Aylmer Fisher(罗纳德·艾尔默·费希尔)

- 女士品茶

- 现代统计学的奠基人

- Fisher线性判别分析

- 极大似然估计法

- 实验设计

3. Neymanand E.Pearson

- 完善假设检验的步骤

### 女士品茶

有一个Young lady非常喜欢喝茶，然后还喜欢在茶（T）里加牛奶（M），同时号称能够尝出来是这两者先后加入的顺序，即能够通过品尝知道是TM（先加茶再加奶）还是MT（先加奶再加茶）。

Fisher觉得，有必要通过一定的统计学方法来确定这个事情是不是真实客观的。那怎么来做这件事情呢？他先准备好MT，TM各4杯，然后让妹子盲喝，最后妹子答对了8杯里的7杯。那么女士有没有说谎？

假设这位lady是随机蒙的，那么可以计算：

![image-20210706142235247](https://cdn.liguocheng.top//uPic/image-20210706142235247.png)

那么出现猜对7杯及7杯以上的概率为：

![image-20210706142305755](https://cdn.liguocheng.top//uPic/image-20210706142305755.png)

由计算可得，如果是随机猜的那么概率会很小，单次测试这种小概率事件不太可能发生，因此Fisher觉得女士没有说谎。

> 这个小概率的标准怎么确定？
>
> Fisher老爷子自己确定了0.05这个数值。

![统计方法](https://cdn.liguocheng.top//uPic/%E7%BB%9F%E8%AE%A1%E6%96%B9%E6%B3%95.png)

### 什么是假设检验

- 什么是假设？

假设可以是对总体的一种看法

比如：Fisher认为女士猜MT和TM是随机，通过这个假设我们可以确定总体的一些参数。

- 什么是检验？

对做出的假设，利用样本信息来判断原假设是否成立。

一般认为小概率事件在单次实验不发生。

#### 假设检验的一般思想

- 总体和抽样

μ1与μ0的差距很小，可能是抽样的误差导致的差异；

μ2与μ0的差距很大，是个小概率事件，因此认为μ2可能是来自不同总体

#### 假设检验的一般步骤

1. 提出原假设H0和备择假设H1

2. 确定适当的检验统计量

3. 计算出抽样结果的P Value

4. 如果P Value很小（一般cutoff=0.01或0.05），则拒绝H0接受H1

### Pvalue 计算

- PValue是观察到抽样结果的或者是比抽样结果更极端的概率

R中计算 Pvalue的函数：

![j1S5Cq](https://cdn.liguocheng.top//uPic/j1S5Cq.png)

> p for "probability", the cumulative distribution function (c. d. f.)
>
> q for "quantile", the inverse c. d. f.
>
> d for "density", the density function (p. f. or p. d. f.)
>
> r for "random", a random variable having the specified distribution
>
> p代表概率计算
>
> q代表分位数计算
>
> d代表概率密度函数
>
> r代表取对应分布的随机数

```R
# norm 
# q,p,d,r
# 概率
pnorm(-1.96,mean = 0,sd= 1)
# 分位数
qnorm(0.99,mean = 0,sd = 1) 
# 概率密度函数
plot(dnorm(seq(-10,10,length.out = 1000),mean = 0,sd = 1))
# 随机正态分布
set.seed(2021)
rnorm(10,mean = 5,sd = 2)
```

### 生信中的例子

BS-Seq也叫亚硫酸氢盐测序技术是一种能够进行全基因组5mC位点检测的技术。

![LfrPPt](https://cdn.liguocheng.top//uPic/LfrPPt.jpg)

当基因组序列中存在未甲基化的C的时候，经过亚硫酸氢盐的DNA会发生脱氨反应，由C变成dU，然后在测序的过程中会读成T；而5mC则不会发生脱氨反应，在测序结果中会被记录成C

已知在某次测序中，全基因组平均突变率为0.003，某个位点中测到带有C的reads 10条，带有T的reads 3条，那么该位点是否为一个甲基化位点？

可以使用二项分布计算pvalue，其中n=13，p=0.003

![image-20210706145949416](https://cdn.liguocheng.top//uPic/image-20210706145949416.png)

在生信分析中，经常会出现peak-calling的过程，比如ChIP-Seq。

![image-20210706150218059](https://cdn.liguocheng.top//uPic/image-20210706150218059.png)

目前ChIP-Seq peak-calling最常使用的软件是MACS2，MACS2中认为reads count在基因组某个区域的分布服从泊松分布，那么，假设你有Input，ChIP_PullDown2个样本，请问：

1. 怎么大概估计某一段区域的Possion分布的参数?

2. 假设已经估算出lambda = 5，如果一段区域内出现了20条reads 那么pvalue应该是多少？

![image-20210706150332319](https://cdn.liguocheng.top//uPic/image-20210706150332319.png)

```R
# methylation call 
binom.test(x = 3,n = 13,p = 0.003)

# Chip-seq call
ppois(q = 20,lambda = 5)
1 - ppois(q = 20,lambda = 5)
```

## 统计功效与假设检验的两类错误

### 两类错误

第一类错误α：假阳性；纳伪

第二类错误β：假阴性；去真

### 统计功效

> α容易控制，但β不容易确定
>
> 提高统计功效：
>
> 加大样本量和更改统计方法
>
> 当样本量确定时，需要trade-off

- 癌症诊断时，需要降低β，即提高α(降低假阴性)

- Call peak时，需要降低α，即提高β(降低假阳性)

![udEQ9m](https://cdn.liguocheng.top//uPic/udEQ9m.png)

### 使用P value常见的错误

1. 在任何时候都以0.05或者0.01为金标准

> 有些统计检验方法非常“敏感”容易出现非常小的P Value

2. 设定P Value阈值时忽略了第2类错误的犯错可能

> 如果H1是一种很严重的疾病，那么有的时候我们应该提高显著水平α，从而降低β

3. 在计算P Value的过程中，忽略了使用假设检验的基本条件

> T检验需要样本符合正态分布，且方差齐性。

4. 在使用P Value的时候，忽略了假设检验原假设

> 回归分析中的原假设是，两个变量没有相关关系，不能用P value的数值判断两个变量相关关系的大小。

## T test

![总体检验](https://cdn.liguocheng.top//uPic/%E6%80%BB%E4%BD%93%E6%A3%80%E9%AA%8C.png)

### Z score和 Z 变换

对任意一个变量X做下列变化均得出的均为Z-Score，如果X服从正态分布，那么Z~N(0,1)，符合标准正态分布。

![wVUJuX](https://cdn.liguocheng.top//uPic/wVUJuX.png)某机床厂加工一种零件，根据经验知道，该厂加工零件的椭圆度近似服从正态分布，其总体均值为0.081mm，总体标准差为0.025 。今换一种新机床进行加工，抽取n=200个零件进行检验，得到的椭圆度为0.076mm。试问新机床加工零件的椭圆度的均值与以前有无显著差异？（α＝0.05)

![f1874Q](https://cdn.liguocheng.top//uPic/f1874Q.png)

-2.83落在-1.96左侧，为小概率事件，拒绝原假设，新机床加工零件的椭圆度的均值与以前有显著差异。

### 为什么有了Z test还要 t test ？

现实生活中，我们很难获得总体方差，往往想通过样本方差代替总体方差。

当样本量很大时，n大于50，认为是大样本，符合正态分布，可以用Z test。

但当样本量很小时，认为样本符合自由度(df)为(n - 1)的t分布。

![4a5zN6](https://cdn.liguocheng.top//uPic/4a5zN6.png)

### T test的四种形式

1. 有1组试验样本数据，与已知标准均值作比较

已知某个基因A在人群中的FPKM稳定在15左右，现测量5个病人的FPKM为：13.1，16.2，14.9，15.8，17.7。试分析病人该基因A有没有显著升高？

```R
# 1. compare with mu
res = c(13.1, 16.2, 14.9, 15.8, 17.7)
t.test(res,mu = 15,alternative = "greater")

One Sample t-test

data:  res
t = 0.71114, df = 4, p-value = 0.2581
alternative hypothesis: true mean is greater than 15
95 percent confidence interval:
 13.9212     Inf
sample estimates:
mean of x 
    15.54 
```



2. 2组独立随机试验结果，在方差相等时作比较

> 在一次细胞实验中，ctrl样本的geneB FPKM为 ：12.33,7.56,11.47 ,9.82 ,9.14
>
> 经过低氧处理后的基因 FPKM为：10.41,14.82,14.13,15.81,13.62
>
> 分析基因B在经过低氧处理后表达量有无显著变化？

```R
# 2. compare two sample 
geneB.ctrl = c(12.33,7.56,11.47 ,9.82 ,9.14)
geneB.deoxy = c(10.41,14.82,14.13,15.81,13.62)
t.test(geneB.ctrl,geneB.deoxy)

Welch Two Sample t-test

data:  geneB.ctrl and geneB.deoxy
t = -2.9671, df = 7.9519, p-value = 0.01807
alternative hypothesis: true difference in means is not equal to 0
95 percent confidence interval:
 -6.5679755 -0.8200245
sample estimates:
mean of x mean of y 
   10.064    13.758 
```



3. 配对样本的均值比较

   > - 比如同组实验样本的前后进行比较
   >
   > - 相当于两个数值做差，然后再进行与数值0的比较

一个以减肥为主要目标的健美俱乐部声称，参加其训练班至少可以使减肥者平均体重减重8.5公斤以上。为了验证该宣称是否可信，调查人员随机抽取了10名参加者，得到他们的体重记录如下表：

![xiL6Xt](https://cdn.liguocheng.top//uPic/xiL6Xt.png)

![5GGsKy](https://cdn.liguocheng.top//uPic/5GGsKy.png)

分别计算样本差值的均值和样本差值的标准差，再与0作比较。

![qw5Js8](https://cdn.liguocheng.top//uPic/qw5Js8.png)

```R
# 3. paried t-test 
before.fitness = c(94.5,101,110,103.5,97,88.5,96.5,101,104,116.5)
after.fitness = c(85,89.5,101.5,96,86,80.5,87,93.5,93,102)
after.fitness.fix = after.fitness + 8.5
## 计算训练前后的体重差值是否显著大于8.5
t.test(before.fitness,after.fitness.fix,paired = T,alternative = "greater")

Paired t-test

data:  before.fitness and after.fitness.fix
t = 1.9413, df = 9, p-value = 0.04207
alternative hypothesis: true difference in means is greater than 0
95 percent confidence interval:
 0.07521379        Inf
sample estimates:
mean of the differences 
                   1.35 
```

4. 2组独立随机试验结果，在方差不相等的情况下作比较

> 在一次细胞实验中，ctrl样本的geneB FPKM为：16.46,13.56,15.05,12.76,11.88
>
> 经过低氧处理后的基因 FPKM为：4.59,13.65,22.66,18.02,7.52
>
> 分析基因B在经过低氧处理后表达量有无显著变化？

- 首次应该使用F检验检查方差是否相等，在方差不相等的情况下，应该使用t'检验或者Wilcox秩和检验。

```R
# 4. compare two sample
geneB.ctrl = c(16.46,13.56,15.05,12.76,11.88)
geneB.deoxy = c(4.59,13.65,22.66,18.02,7.52)

# test var 
var.test(geneB.ctrl,geneB.deoxy)

F test to compare two variances

data:  geneB.ctrl and geneB.deoxy
F = 0.060939, num df = 4, denom df = 4, p-value = 0.01904
alternative hypothesis: true ratio of variances is not equal to 1
95 percent confidence interval:
 0.006344836 0.585292073
sample estimates:
ratio of variances 
        0.06093917
# t-test 
t.test(geneB.ctrl,geneB.deoxy,var.equal = F)

# t = 0.19175, df = 4.4857, p-value = 0.8563
# wilcox test 
wilcox.test(geneB.ctrl,geneB.deoxy)

Wilcoxon rank sum exact test

data:  geneB.ctrl and geneB.deoxy
W = 12, p-value = 1
alternative hypothesis: true location shift is not equal to 0
```

### 使用 T test需要注意

有两批实验结果，需要比较是否有差异

- 两个样本是独立的随机样本

- 两个总体都是正态分布

- 两个总体方差未知但相等

配对T-test

- 两个总体都服从正态分布

- 如果不服从正态分布，可用正态分布来近似(n1 > 30, n2 >30 )

## 列联表检验

### K. Pearson 与拟合优度检验

> 如果已知一个分布F(x)，获得了一个抽样结果X，那么怎么判断X是不是服从F(x)分布？

H0: X具有分布F

H1: X不具有分布F

将(−∞,+∞)分成m个区间：

𝐼1=(−∞,𝑎1), 𝐼2=[𝑎1,𝑎2), 𝐼3=[𝑎2,𝑎3) … 𝐼𝑚=[𝑎𝑚−1,+∞)

对应区间的理论概率分布分别为：

𝑝1,𝑝2,𝑝3,…𝑝𝑚

记𝑛𝑖为𝑋1,𝑋2,𝑋3…𝑋𝑚中落在区间𝐼𝑖中的观测个数，那么在原假设成立的情况下，(𝑛𝑖-𝑛𝑝𝑖)的差值应该不会很大。

*K.Pearson* 发现若原假设成立的情况下，当n趋向无穷大则会有下面的关系式：

![jkCHB3](https://cdn.liguocheng.top//uPic/jkCHB3.png)

因此，当在大样本水平下，我们可以认为统计量K近似服从卡方分布，从而进行卡方检验。

- 卡方分布与自由度有关

![image-20210706215350943](https://cdn.liguocheng.top//uPic/image-20210706215350943.png)

### 卡方检验应用

1. 大麦的杂交后代无芒：长芒：短芒= 9:3:4，实际观测数据为335:125:160.请问试验结果是否符合理论假设？

> 相当于是与理论分布的概率进行比较，还是chisq.test函数，不过需要使用p参数。

```R
# chisq test 2 
ratio_vec = c(335,125,160)
prob_vec = c(9,3,4) / 16
chisq.test(ratio_vec,p = prob_vec)

Chi-squared test for given probabilities

data:  ratio_vec
X-squared = 1.362, df = 2, p-value = 0.5061
# 无法拒绝原假设，试验结果符合理论假设。
```

2. 在GO、KEGG等富集分析中经常会遇到检验问题：

一次RNA-Seq中，差异表达基因有255个，其中55个基因能够被注释成通路A，请问差异显著的基因是否在通路A有富集？

![UZErva](https://cdn.liguocheng.top//uPic/UZErva.png)

如果两者没有差异，那么两者在通路A与不在通路A的基因数目的比值应该相同。也就是说

- 理论上在通路A中的基因概率= （55 + 200）/ （255 + 20800）= 0.012111

- 理论上不在通路A中的概率= 1 -0.012111

- 因此可以计算列联表中对应位置在“没有差异”这个假设下的理论数值T

> 差异表达基因在通路A

T~11~= (55 + 200) * 0.012111= 3.088

> 差异表达基因不在通路A

T~12~= (55 + 200) * (1 -0.012111) = 251.912 

T~21~= (200 + 19800) * 0.012111 = 251.912

T~22~= (200 + 19800) * (1 -0.012111) = 20548.088 

![6ChveX](https://cdn.liguocheng.top//uPic/6ChveX.png)

- 自由度= (R-1) * (C-1); 其中R为行数，C为列数

统计量𝐾=(55−3)^2/3 + (200−252)^2/252 + (200−252)^2/252 + (19800−20548)^2/20548

```R
# chisq test 3
test_mat = matrix(c(55,200,200,19800),ncol = 2,nrow = 2)
chisq.test(test_mat)

Pearson's Chi-squared test with Yates' continuity correction

data:  test_mat
X-squared = 840.46, df = 1, p-value < 2.2e-16

Warning message:
In chisq.test(test_mat) : Chi-squared approximation may be incorrect
```

只有当在大样本水平下，我们可以认为统计量K近似服从卡方分布，否则使用Fisher’s exact test

- 当表格中理论频数小于5，或者是加和样本总数n小于40时，我们应该使用Fisher精确检验，而不是卡方检验。

### Fisher’s exact test

Fisher精确检验本质上是超几何分布检验，本问题可以理解成：

- 在21055个基因中，有255个在通路A，20800不在通路A；

- 随机抽取255个基因，观察到在通路A中的基因个数为55个；

- 那么出现该抽样结果，或者是更罕见情况的概率就是最终的P Value

```R
# fisher test 
test_mat = matrix(c(55,200,200,19800),ncol = 2,nrow = 2)
fisher.test(test_mat)

Fisher's Exact Test for Count Data

data:  test_mat
p-value < 2.2e-16
alternative hypothesis: true odds ratio is not equal to 1
95 percent confidence interval:
 19.19368 38.12413
sample estimates:
odds ratio 
  27.19726 
```

### 列联表检验总结

- 卡方检验以卡方分布和拟合优度检验为理论依据，使用范围非常广泛，大家可以灵活运用；

- 当应用卡方检验时，理论频数小于5，或者是试验总数小于40时，应当使用Fisher精确检验；

- 生物信息学中比较常见的列联表检验：

  富集分析，如GO，KEGG，DO富集分析等；

  判断某位点是否为突变位点；

## 多重假设检验的矫正

### P value 的矫正

当有超过2次检验以上的情况时，就要做多重假设检验校正。

> 虽然我们会控制第1类错误α的犯错概率在一个很小的范围内，但是我们在进行多次检验的过程中，不可避免地会碰到第1类错误。
>
> 因此，我们需要对P Value进行一个放大，从而更加严格地筛选我们检验结果。

- 对于Bonferroni校正，是将p-value扩大n倍作为校正结果，但往往太过严格；

- 目前最常用的方法是BH(Benjaminiand Hochberg)校正方法，具体操作：

对每个pvalue做校正，转换为q-value：

q value = p value * n / rank，其中rank是指p value从小到大排序后的次序；

q value 有时也被称为FDR（False Discover Rate），从数学上可以严格证明，经过BH矫正后的qvalue可以，如果选择α作为qvalue的阈值，那么FDR不会超过α

![R1rPCC](https://cdn.liguocheng.top//uPic/R1rPCC.jpg)

```R
# pvalue adj
set.seed(2021)

test_pvalue = abs(rnorm(20,mean = 0.01,sd=0.05))
test_pvalue

[1] 0.013051940 0.065686970 0.027708780 0.031860281 0.064468110 0.021540743 0.037773877 0.067085612 0.035555654
[10] 0.066281983 0.014434349 0.049363589 0.070608399 0.005990335 0.032993882 0.001408913 0.089477719 0.048231967
[19] 0.105222468 0.029829408

test_pvalue < 0.05
table(test_pvalue < 0.05)

FALSE  TRUE 
    7    13

qvalue = p.adjust(test_pvalue,method = "BH")
table(qvalue < 0.05)
# 实际显著性的只有一个
FALSE  TRUE 
   19     1 
```

### 方差分析(离散型随机变量)

#### 为什么进行方差分析？

例子：某研究者在某单位工作人员中进行了体重指数（BMI）抽样调查，随机抽取不同年龄组男性受试者各16名，测量了被调查者的身高和体重值，由此按照BMI=体重/身高2公式计算了体重指数，请问，不同年龄组的体重指数有无差异？

![LaZV1x](https://cdn.liguocheng.top//uPic/LaZV1x.png)

- 两两比较，然后再进行校正（P Value校正）

- 直接比较，看是是否组间差异大于组内差异（方差分析）

> 本质上，方差分析也是为了避免多重检验的一种解决方案。
>
> 要分清“因素”与“水平”的概念。本例子中的年龄为该试验的因素，3个不同年龄段为3个不同水平。

总变异= 组间变异+ 组内变异

总变异(Total variation):  全部测量值与𝑋𝑖𝑗总均数𝑋间的差异

组间变异(between group variation ):  各组的均数𝑋𝑖.与总均数𝑋间的差异

组内变异（within group variation )：每组的每个测量值𝑋𝑖𝑗与该组均数𝑋𝑖.的差异

### 均方差

变异程度除与离均差平方和的大小有关外，还与其自由度有关，由于各部分自由度不相等，因此各部分离均差平方和不能直接比较，须将各部分离均差平方和除以相应自由度，其比值称为均方差，简称均方(mean square，MS)。

组间均方和组内均方的计算公式为:

MS~组间~ = SS~组间~/v~组间~         MS~组内~ = SS~组内~/v~组内~

> 一般自由度等于可以相对可以自由变换的随机变量的个数。

### 方差分析的假设检验思想

**问题转换**

- 所以我们就可以计算MS组间与MS组内的比值；

- 如果上述各组来自同一个总体，那么所有的误差应该都是来自于随机抽样，因此MS组间与MS组内的比值应该在1附近；

- 如果来自于不同总体，也即某个因素确实对抽样的结果产生了影响，那么MS组间与MS组内的比值应该有比较大的差异；

- 因此，最终我们将方差分析的问题转换成了MS组间与MS组内的比值检验问题。

构建F统计量为组间与组内MS的比值：

F = MS~组间~ / MS~组内~

如果各组样本的总体均数相等（H0成立），即各处理组的样本来自相同总体，无处理因素的作用，则组间变异同组内变异一样，只反映随机误差作用的大小。组间均方与组内均方的比值称为F统计量。

F值接近于1，就没有理由拒绝H0；反之，F值越大，拒绝 H0的理由越充分。数理统计的理论证明，当H0成立时，F统计量服从F分布。

> F分布由两个自由度决定
>
> F分布其实与卡方分布密不可分

![0vjuj5](https://cdn.liguocheng.top//uPic/0vjuj5.png)

最后查F分布对应自由度的表可以知，α~0.05~在3.20到3.21左右，小于8.87，因此要拒绝H0，即该年龄因素对BMI产生了影响。

```R
# 方差分析
set.seed(2021)
BMI.1 = rnorm(16,22,9)

set.seed(2021)
BMI.2 = rnorm(16,26,8)

set.seed(2021)
BMI.3 = rnorm(16,28,7)

BMI.df = data.frame(BMI=c(BMI.1,BMI.2,BMI.3),
                    group = factor(rep(c(1,2,3),each=16)))

BMI.aov = aov(BMI ~ group, data = BMI.df)
BMI.aov

Call:
   aov(formula = BMI ~ group, data = BMI.df)

Terms:
                    group Residuals
Sum of Squares   378.7787 2309.4096
Deg. of Freedom         2        45

Residual standard error: 7.163813
Estimated effects may be unbalanced

summary(BMI.aov)

           Df Sum Sq Mean Sq F value Pr(>F)  
group        2  378.8  189.39    3.69 0.0328 *
Residuals   45 2309.4   51.32                 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```



### 方差分析使用的基本条件

- 各组服从正态分布；

- 各组符合方差齐性；

- 独立性；

> 最大方差与最小方差之比<3，初步认为方差齐

> 比如在做同因素不同水平的RNA-Seq，就可以使用方差分析来判定该因素是否对某个基因有影响。

## 回归分析

> 回归是指在某个区间内来回波动的现象。

### 分类

- 一元线性回归

- 多元线性回归

- 广义线性回归
  - Logistic 回归等

- 不符合最小二乘法的线性回归
  - 岭回归
  - Lasso回归

### 重要概念

变量(Variable)

- 描述事物特征或特质的数量指标，这些指标因条件不同而按一定的规律变化，故称变量。

变量间的关系(Relationships between variables)

- 函数关系(Functional relationship)

​     比如欧姆定律。

- 相关关系(Correlation relationship) 

  当一个变量取定某个数值时，另一个变量出现的对应值不是完全确定的。如：施氮量和作物。

### 一元线性回归

我们观察到的变量，因为会受到随机因素的影响，从而出现一定的波动，因此在分析问题的时候，我们通常会把观察到的Y值理解成“能解释的部分”与“随机误差部分”的加和

`𝑦 = 𝑎𝑥 + 𝜀`

我们一般认为，随机误差部分是服从正态分布的。

![QZVnbF](https://cdn.liguocheng.top//uPic/QZVnbF.png)



### 回归系数与截距的确定

一般通过最小二乘原则来确定𝑏与𝑏0即使得，Q达到最小.

![7PPBIg](https://cdn.liguocheng.top//uPic/7PPBIg.png)

![image-20210707110217198](https://cdn.liguocheng.top//uPic/image-20210707110217198.png)

### 回归关系的显著性检验

与方差分析类似，我们也可以把回归结果的差异分成2个部分：

总变异= 离回归变异（剩余变异) + 回归变异

与方差分析类似，我们就可以计算SS回归/Df回归与SS剩余/Df剩余的比值，其中Df表示对应的变量的自由度，因为是一元线性回归，所以自由度分别是1与n-2，n为回归的点个数

![image-20210707110513285](https://cdn.liguocheng.top//uPic/image-20210707110513285.png)

H0: 两个变量间的线性关系不显著

H1: 两个变量间的线性关系显著

![image-20210707110814468](https://cdn.liguocheng.top//uPic/image-20210707110814468.png)

如果比值F比较小，说明回归能解释的部分与不能解释的部分没有显著差异，证明线性关系不显著，否则就需要接受H1，认为两个变量有显著的相关性。

```R
#线性回归分析
input.df = read.csv(file = "/Users/chandoubaxiaochongzi/Desktop/统计学习/生物信息学入门课：学习生信你需要了解的统计学/lm_data.csv")
plot(input.df$x,input.df$y)

# run regression
lm.res = lm(y ~ x,data = input.df)
lm.res
Call:
lm(formula = y ~ x, data = input.df)

Coefficients:
(Intercept)            x  
   -0.82952      0.03789

summary(lm.res)

Call:
lm(formula = y ~ x, data = input.df)

Residuals:
    Min      1Q  Median      3Q     Max 
-2.2882 -1.5233 -0.1802  0.8935  6.3038 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -0.82952    0.72304  -1.147    0.263    
x            0.03789    0.00503   7.534 1.18e-07 ***
---

abline(lm.res$coefficients)
```

![q8IJpu](https://cdn.liguocheng.top//uPic/q8IJpu.jpg)

### 两个变量的相关系数

Pearson相关系数，也叫Pearson r

![image-20210707112402406](https://cdn.liguocheng.top//uPic/image-20210707112402406.png)

### 决定系数

如何衡量我们的回归好不好？

这时候我们就需要计算R^2^ = SS回归/SS总

- R2就是所谓的“决定系数”

- 在一元线性回归时，R^2^ = r^2^

![image-20210707112928720](https://cdn.liguocheng.top//uPic/image-20210707112928720.png)

### Spearman 相关系数

对两个变量排序以后的rank进行计算Pearson相关系数，被称为Spearman相关系数。

Pearson相关系数只能衡量线性程度(等于0并不一定是非相关)，而Spearman相关系数则偏重考察两个变量是否有正相关或者负相关。

```R
# correlation 
x = c(1:100)
y = x^10

cor(x,y,method = "pearson")
## [1] 0.6630055
cor(x,y,method = "spearman")
## [1] 1
```

## 非参数检验

### 什么是非参数检验

- 和数据本身的总体分布无关的检验称为非参数检验。

- 不假定总体的具体背景分布形式；

- 多根据数据观测值的相对大小建立检验统计量，然后找到在零假设下这些统计量的分布，看这些统计量的数据实现是否在零假设下属于小概率事件。

### 优缺点

- 优点

> 在总体分布未知时，如果还假定总体有诸如正态分布那样的已知分布，在进行统计推断就可能产生错误甚至灾难。
>
> 非参数检验总是比传统检验安全（更不容易拒绝原假设）。

- 缺点

> 但是在总体分布形式已知时，非参数检验不如传统方法power高。

### 重要的非参数检验

基于K. Pearson拟合优度的检验

- 列联表检验

- 是否服从已知分布的检验

Wilcox秩和检验

Kolmogorov-Smirnov检验（KS检验）

### Wilcox秩和检验

#### 什么是秩(rank)？

一般来说，秩就是该数据按照升序排列之后，每个观测值的位置。如果有排序相同的情况下，就取排序位置处的平均值。

![WLKgwL](https://cdn.liguocheng.top//uPic/WLKgwL.png)

#### Wilcox秩和检验步骤(配对样本)

- 计算样本中成对数据的差值；

- 计算差值绝对值的秩；

- 分别计算出差值序列中正数的秩和W +以及负数的秩和W -

- 如果两个秩和比较接近，则说明之前和之后没有显著差异.

样本量很大时，可以根据W计算Z统计量，使用正态分布来近似计算

![6CJRhz](https://cdn.liguocheng.top//uPic/6CJRhz.png)

#### Wilcox秩和检验步骤(非配对样本)

- 将两个样本的数据混合在一起排秩；

- 分别计算两个样本中数据的秩和（rank sum）

- 如果两个总体具有相同的统计分布，则来自两个样本的秩和应该比较接近。

```R
# wilcox test 
pre_ps = c(15.8,14.9,15.2,15.8,15.5,14.6,15,14.9,15.1,15.4)
after_ps = c(14.6,15.5,15.5,14.7,15.2,14.8,14.8,14.6,15.3,15.5)
wilcox.test(pre_ps,after_ps,paired = T)

Wilcoxon signed rank test with continuity correction

data:  pre_ps and after_ps
V = 33, p-value = 0.6091
alternative hypothesis: true location shift is not equal to 0
# 非配对样本
wilcox.test(pre_ps,c(after_ps,10,20,30),paired = F)
```

### KS 检验

- Kolmogorov-Smirnov检验属于拟合优度检验的一种；

- KS检验可以非常方便地检验抽样是否服从某一已知分布；

- KS检验也可以直接比较2个试验结果是否来自同一总体。

```R
# KS test
## 是否属于某个理论分布
time_vec = c(420,500,920,1380,1510,1650,1760,2100,2300,2350)
ks.test(time_vec,"pexp",1/1500)

One-sample Kolmogorov-Smirnov test

data:  time_vec
D = 0.30148, p-value = 0.2654
alternative hypothesis: two-sided

## 两组试验是否来自同一总体
b =rexp(1000,rate = 1/1500)
ks.test(time_vec,b)

Two-sample Kolmogorov-Smirnov test

data:  time_vec and b
D = 0.317, p-value = 0.2728
alternative hypothesis: two-sided
```



## Reference

https://ke.qq.com/course/395709

## License

Copyright 2021 [Guo-Cheng Li](https://moths.com). Released under the [MIT](https://github.com/gcushen/hugo-academic/blob/master/LICENSE.md) license.

