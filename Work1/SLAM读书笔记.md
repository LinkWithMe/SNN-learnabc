# SLAM读书笔记

[TOC]

## 一、ch3-三维空间刚体运动

本章主要解决刚体在三维空间中的运动。直观上看，我们知道运动是由一次旋转加一次平移组成。平移比较简单，而旋转却比较难以处理。通过介绍旋转矩阵与变换矩阵、四元数、欧拉角、旋转向量等概念来进行学习。

### 1.旋转矩阵与变换矩阵

#### 1.1 点、向量和坐标系，旋转矩阵

##### 1.1.1 刚体基本概念

①三维空间由3个轴组成，则一个空间点的位置可以由3个坐标指定

②**刚体**，是一种在运动和受力作用后，形状和大小不变，而且内部各点的相对位置不变的物体，不光有**位置**，还有自己的**姿态**。

例如，相机可以看成是三维空间中的刚体，则：

**位置**：相机在空间中的哪个地方

**姿态**：相机的朝向

一个描述刚体的句子：相机处于空间(0,0,0)处，朝向正前方

##### 1.1.2 数学基本概念

点：

①点没有长度，没有体积

②点和点可以组成向量

向量：

①带指向性的箭头

②可以进行加法减法等运算

坐标：

指定坐标系后，可以讨论该向量在此坐标系下的坐标

坐标系：

构成线性空间的一组基，分为左手系和右手系

向量的运算可以由坐标运算来表示，分为加减法、内积、外积等。

##### 1.1.3 不同坐标系中的向量计算

坐标系的转换分两步：

①三个轴的旋转

②原点之间的平移

通过旋转与平移，将世界坐标系转化为相机坐标系：

![1660819906551](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/1)

平移是一个向量

旋转则是：

当坐标系(e1,e2,e3)通过一次旋转变为(e1',e2',e3')时，这个坐标系中的某个向量a的坐标也要进行相应的计算重新得出：

在新旧坐标系中，必须保持该等式的成立

![1660820243388](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/2)

在等式两侧的左侧同时乘以(e1,e2,e3)的转置，可得：

![1660820288107](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/3)

可以简单看成**a=Ra'**，其中R称之为**旋转矩阵**

##### 1.1.4 旋转矩阵 

取行列式为1的正交矩阵为旋转矩阵，数学表达如下：

![1660820855745](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/4)

通过旋转矩阵来描述两个坐标的变换关系：

![1660820942474](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/5)

当加上平移t后，两个坐标系的刚体运动可以由R，t完全描述：

![1660820972021](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/6)

#### 1.2 齐次坐标和变换矩阵

上述过程，已经完整描述了欧式空间的旋转和平移，但其并不是一个线性关系。

例如，从a->b->c的变换过程：

![1660821360195](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/7)

不难发现，若经历多次旋转变化，那么式子叠加起来会越来越复杂，因此通过加1，改写为如下形式：

![1660821429851](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/8)

相关概念：

这种用**四个数**表达**三维向量**的做法称为齐次坐标，引入齐次坐标后，旋转和平移可以放在同一个矩阵中，称为**变换矩阵**，数学上的表达如下：

![1660821549322](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/9)

为4x4矩阵，且R属于三阶的旋转矩阵

因此，反向的变换就是T的逆：

![1660821638282](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/10)

旋转矩阵与变换矩阵存在的问题：

①表达方式冗余，旋转矩阵9个量，但只有3个自由度，而变换矩阵有16个量，但只有6个自由度

②旋转矩阵必须是正交矩阵且行列式为1，这些约束使得求解更加困难

### 2.旋转向量

解决旋转矩阵的冗余问题，提出了旋转向量的概念，旋转向量的三自由度，由R3向量表示，方向为旋转轴，长度为转过的角度，计算公式如下：

![1660832008784](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/11)

旋转向量只有三个量，没有约束

旋转向量转换为旋转矩阵，通过罗德里格斯公式：

![1660832071754](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/12)

旋转矩阵转旋转向量时，通过角度和轴进行计算：

![1660832125501](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/13)

无论是旋转矩阵还是旋转向量，它们都能描述旋转，但是对于我们理解起来是非常不直观的。当看到一个旋转矩阵或者是旋转向量时，难以想象出来这个旋转究竟是什么样。

### 3.欧拉角

欧拉角提供了一种非常直观的，表示旋转的方式。

- 欧拉角将旋转分解为**三次**，不同轴上的转动，以方便理解
- 根据轴顺序的不同，存在许多种定义不同的欧拉角

一种欧拉角的旋转过程如下：

①绕物体的Z轴旋转，得到偏航角yaw

②绕**旋转之后**的Y轴旋转，得到俯仰角pitch

③绕**旋转之后**的X轴旋转，得到滚转角roll

如下图，需要注意的是，第一次旋转是针对全局的Z轴，之后的两次旋转都是针对自己的轴：

![1660917936144](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/14)

欧拉角存在的问题是**万向锁**，在ZYX顺序中，若第二次旋转得到的为正负90°，则第三次和第一次绕同一个轴，使得系统丢失了一个自由度，存在**奇异性问题**。如图：

![1660918242983](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/15)

经过第二次绕y轴旋转±90°后，X轴变成与地面垂直的轴，这时的X轴与之前的Z轴重合。

万向锁的一些特点：

- 由于万向锁，欧拉角不适合插值和迭代，往往用于人机交互中
- 当用三个实数来表达三维旋转时，会不可避免碰到奇异性问题
- SLAM程序中很少直接用欧拉角来表示姿态

并且，我们找不到不带奇异性的三维向量描述方式

### 4.四元数

#### 4.1 基本概念

四元数的一些基本概念：

- 一种扩展的复数，既是紧凑的也没有奇异性
- 有三个虚部，可以表达三维空间中的旋转

数学上的定义如下：

![1660919419695](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/16)

一些基本性质：

![1660919450982](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/17)

四元数的一些运算性质：

![1660919711958](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/18)

其中两种乘法亦有差距，其中点乘为对应位置数值相乘，乘法原理如下：

![1660919837296](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/19)

#### 4.2 四元数表示旋转

用四元数表示旋转的方式如下：

![1661502548282](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/20)

并且，变换之后的V的实部为0，虚部是罗德里格斯公式的结果

四元数和旋转向量之间的关系：

旋转向量到四元数：

![1661516297940](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/21)

四元数到旋转向量：

![1661516394060](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/22)

四元数与旋转矩阵的关系：

![1661516463049](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/23)

可以在q2的左侧或者右侧乘以矩阵q1

## 二、ch5-相机与图像

### 1.单目相机模型

在计算机中，一张照片由许多像素组成，每个像素记录了色彩或者亮度的信息。三维世界中的一个物体反射出的光线，穿过相机光心后，投影在相机的成像平面上。

而**相机模型**是将三维世界中的**坐标点**(以米为单位)映射到**二维图象平面**(以像素为单位)的过程

一些特点和问题：

- 这一过程中丢失了"**距离**"维度上的信息
- 当用几何模型来描述时，最简单的是**针孔模型**
- 在投影过程中，由于相机镜头上的透镜的存在，使得光线投影到成像平面会产生**畸变**

描述整个投影过程，就构成了**针孔和畸变模型**，它们构成了相机的**内参数**。

#### 1.1 针孔相机模型

##### 1.1.1 相机坐标系-成像平面

设O-x-y-z为相机坐标系，z轴指向相机前方，x轴向右(我们应该站在左侧看右侧)，y轴向下，O为摄像机的光心，也是针孔模型中的针孔。

物理成像平面为O'-x'-y'

在此情况下，相机模型就是：现实世界的空间P经过小孔o投影之后，落在物理成像平面O'-x'-y'上，成像点为P'，图示如下：

![1661520515582](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/24)

在这个成像过程中，可以有以下三角形：

![1661520763796](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/25)

根据相似三角形原理，可以得到公式：

![1661520814971](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/26)

公式描述了P(单位：米)和它的成像P'(单位：米)之间的空间关系，其中f表示焦距，单位同样为米。负号表示，成像与实际物体刚好相反，如下图所示：

![1661520839867](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/27)

##### 1.1.2 相机坐标系-像素坐标系

相机中，最终获得的是一个个像素，于是在物理成像平面上固定一个像素平面o-u-v，就可以在像素平面得到P'的像素坐标。

像素坐标系的定义方式：原点o'位于图像的左上角，u轴向右与x轴平行，v轴向下与y轴平行(从左侧视角来看)。

像素坐标系与成像平面之间，相差了一个**缩放和一个原点的平移**。图示如下：

![1661526607739](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/28)

成像平面到像素坐标的坐标转换公式如下：

![1661526677436](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/29)

其中，α和β为缩放比例，cx和cy为平移距离。

详细点从矩阵角度来看，将u，v写成齐次坐标的形式，即可写成如下形式:

![1661527029743](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/30)

其中，矩阵K为内参数，PC～为相机坐标系位置。

将等式两边同时乘以Z，写成传统形式如下：

![1661527214962](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/31)

该式子描述了相机坐标系中点(单位：米)和像素坐标系中(单位：像素)的关系。且fx，fy，cx和cy的单位都为像素。

从上述式子可以看出，当Z扩大时，那Pc里面三个维度的值都要扩大，而XYZ表示的是物体的相机坐标，因此，可以形象的看出，是p变得更远，但是p2在像素坐标系下的坐标还是(u v 1)没有变，因此丢失了深度信息。

另外，当取Z为1时，实际规定为归一化平面，带入Z=1可得：

![1661527940162](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/32)

K为内参，一般在相机生产时就已经确定。除去内参外，相机坐标系和世界坐标系还相差一个变化：

![1661591318890](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/33)

其中R，t为外参，即在相机生产之后，随着相机运动而发生变化。右侧式子隐含了一次非齐次到齐次的变换

外参也是SLAM估计的目标，而内参已知

总体成像过程：由世界中的物体坐标->相机坐标->归一化平面->像素坐标

##### 1.1.3 畸变

通过模型设置参数的方式来对畸变进行修正。

主要的畸变类型：径向畸变和切向畸变。径向畸变导致桶型失真和枕形失真

可以用归一化坐标来对畸变进行描述，径向和切向如下：

![1661592399061](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/34)

其中r为根号下x的平方+y的平方

将切向畸变与径向畸变放在一起，就可以建立一个描述畸变的模型：

![1661592465778](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/35)

小孔成像的一般流程：

![1661592552109](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/36)

其中，畸变的坐标处理放在3-4步之间

### 2.双目相机模型

双目相机模型中，左右相机中心距离称为基线，左右两个相机水平放置，抽象成几何模型如下：

![1661593626773](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/37)

根据相似三角形，可以得到左右像素的几何关系：

![1661593653955](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/38)

根据整理后的公式，我们就可以根据左右不同相机像素的坐标，来计算出P点距离相机的距离z(f为焦距，b为基线，d为视差[描述同一个点在左右目上成像的距离])。

d最小为1个像素，且双目能测量的距离z存在最大值：fb

距离公式简单，但是d不容易进行计算，因为不知道左右目中的同一物体的像素

### 3.RGB-D相机模型

RGB-D相机通过物理手段来测量深度，主要有ToF和结构光两种测量深度的方式：

![1661611658681](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/39)

这种方式需要打光以及采集返回光，当一些物体吸收光线，收集不到返回的光线时，这种方法就会出现问题。

### 4.图像

相机成像后，便生成了图像。图像在计算机中以二维数组的形式进行存储，而一个像素点的具体信息，可以根据灰度图还是彩色图的不同，来规定不同的格式：

![1661613029081](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/40)

## 三、ch7-视觉里程计

### 1.特征点法

在经典SLAM模型中，是以**位姿—路标**来描述SLAM过程。

路标(Landmark)一般是三维空间中固定不变的点，且能够在特定位姿下观察到，一般有以下特点：

- 数量充足，以便实现良好的定位
- 区分性较好，以便实现数据关联

在视觉SLAM中，可以利用**图像特征点**(一般是图像中一些比较容易区分的点)来作为SLAM中的路标。

#### 1.1 特征点

选取的特征点要具有以下特点：

- 可重复性
- 可区别性(不同的特征点之间可以有效区分)
- 高效
- 本地

在如下所示图中：

![1661615728127](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/41)

一般选择**角点**为特征点，在一些情况下也可以选择边缘为特征点，但一般不选择区块

特征点的信息包括：

位置、大小、方向、评分等—**关键点**

特征点周围的图像信息(用**特征点周围的信息**，来区分不同的特征点)—**描述子**(一个好的**特征描述**要在光照、视角发生少量变化是仍能保持一致)

有了描述子，便可以进行特征匹配，但是根据描述子的质量，所需要消耗的算力也不尽相同

#### 1.2 ORB特征

以ORB特征算法举例，ORB算法的提取关键点的算法是Oriented FAST，用BRIEF来进行描述。

##### 1.2.1 FAST

在提取FAST时，通过比较一个点周围像素连续N个点的灰度值的差异来进行选取：

![1661667225329](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/42)

如图所示，在P点周围选取16个点，若这些连续的N个点的灰度值比P点大或者小一个阈值，就将P看作是关键点。

而Oriented FAST能够根据图像中的灰度差异，来计算出图像的梯度方向，为图像赋予角度

##### 1.2.2 BRIEF

BRIEF描述子，用来区分不同的特征点。

BRIEF是一种二进制描述，用汉明距离来进行度量。如BRIEF-128就是128bit，包含128对两点之间的比较结果，结果有0与1的区别。根据两点的取的分布的不同，可以有以下pattern：

![1661667862067](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/43)

#### 1.3 特征匹配

得到描述子之后，通过特征匹配，根据描述子的差异来在两张图中判断哪些特征点为同一个点。暴力匹配方法，通过逐个比较图1和图2中的特征点来进行匹配。也可以通过加速的FLANN快速最近邻方法。

### 2.对极几何

#### 2.1 引入

根据特征匹配之后的结果不同，得到了特征点之间不同的对应关系：

![1661668772920](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/44)

基本的示意图、几何关系如下：

![1661671109615](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/45)

其中：

- T12为I2的坐标乘以T12变化为I1的坐标
- p1,p2为已知的像素坐标点

根据两个已知的像素坐标点，怎么去求相机运动和实际物体的位置，就是**定位和建图**的问题。

#### 2.2 数学语言描述

关于点P的空间位置坐标，以及p1，p2像素点的位置坐标设置，有如下解释和规定：

![1661687976436](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/46)

对于如上的s1p1，s2p2两个公式，我们进行如下操作：

![1661696182456](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/47)

- 第一行是乘以k的逆
- 第三行中的x1，x2，实际上是转化成了归一化坐标
- 第六行中，为了消去第五行中的式子中的t，在等式两边同时叉乘了t。**一个向量叉乘自身，结果为0**。
- 第八行中，左侧的式子形成混合积，且结果为0.
- 第九行中，向量叉乘，这里把t转化为了**t的对称矩阵**
- 第十行，得出结果，带有**p1p2像素坐标的对极几何**

#### 2.3 对极几何概念

##### 2.3.1 对极约束

![1661698890318](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/48)

相机位姿的估计问题分为以下两步：

![1661698930353](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/49)

##### 2.3.2 本质矩阵

本质矩阵的性质如下：

![1661699594849](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/50)

本质矩阵E的求解方法：

![1661699612840](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/51)

八点法求E的过程中，将E看作通常意义下的3x3矩阵，去掉尺度因子后剩余八个自由度：

![1661774503924](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/52)

对于如上的矩阵方程组，根据矩阵的性质，可以对解的结果进行初步判断：

![1661775092656](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/53)

如果给定的匹配点多于8，且方程彼此间存在线性相关，则该方程就构成了一个**超定方程**，即不一定存在e使得上式成立。此时可以选择最小二乘法来进行求解。

![1661776447349](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/54)

但是，在可能存在**误匹配**的情况下，更倾向于采用**随机采样一致性**来求。该算法适用于很多带误匹配的数据。

最小二乘法的缺点：当异常点比较多时，由于最小二乘是使得所有点离目标函数最近，为了保证这一点，拟合直线会使得模型鲁棒性较差，进而得出较差的拟合结果。

而随机采样一致性：

![1661777000760](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/55)

算法的基本流程：

![1661777219733](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/56)

![1661777253231](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/57)

![1661777278563](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/58)

![1661777304277](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/59)

整体的流程表述如下：

![1661777374309](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/60)

##### 2.3.3 从E中分解R和t

对E进行奇异值分解：

![1661777533534](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/61)

##### 2.3.4 单应矩阵H

八点法在特征点共面时会发生退化

涉疫对匹配好的特征点位于某平面上，则这个平面P满足方程：

![1661782981565](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/62)

那么两个图像特征点的关系可以如下表示：

![1661783056718](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/63)

线性方程求H：

![1661783173888](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/64)

其中，去掉第三行是因为线性相关

通过单应矩阵来恢复R，t：

![1661783252749](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/65)

##### 2.3.5 尺度不确定性

![1661783389873](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/66)

简单来说，我们只能获取图像的轮廓和结构，而无法了解其真实尺寸。

### 3.三角测量

 结合先前的部分内容，我们已经学习到了对极几何，结合知识梳理的算法处理流程如下：

![1661786148234](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/67)

使用三角测量的原因：

![1661786313804](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/68)

三角测量是指**通过在两处观察同一个点的夹角，从而确定该点的距离**。

在如下图中：

![1661786416935](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/69)

由于噪声的影响，无法比较准确地定位实际物体的地点，因此通过最小二乘法来求解。

根据R和t来求解深度s1，s2：

![1661786646258](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/70)

三角化中存在的问题，以及详细的解释：

![1661786764592](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/71)

### 4.PNP

#### 4.1 PNP问题描述

已知：

- 3D：上一帧的相机坐标系下的3D点
- 2D：两幅图片的匹配点信息，像素坐标
- 其他：相机内参

![1661787049702](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/72)

求：

**两帧之间的变换矩阵R，t**

解决方法：

- 直接线性变换(DLT)
- P3P
- EPNP
- UPNP
- 非线性优化

#### 4.2 直接线性变换(DLT)

数学语言描述如下：

![1661787343004](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/73)

由此可见，一对匹配点可以得到**两个关于t的线性约束**，因此：

![1661787522005](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/74)

局限性主要是因为，**旋转矩阵有其自身的限制**，而得出的R矩阵可能不满足该限制，因此需要寻找一个最好的R矩阵来进行近似。

#### 4.3 P3P

 数学语言描述如下：

输入数据为**3对**3D-2D匹配点

在如下图中：

![1661847511450](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/75)

已知：

- O是相机光心
- 3D点为A,B,C(世界坐标系的点)
- 2D点为a,b,c(A,B,C在相机成像平面上的投影)

需要求解的信息是，该点在相机坐标系下的3D坐标

根据相似三角形原理可得：

![1661847685711](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/76)

最后得到一个关于x和y的二元二次方程，通过吴消元法进行求解：

![1661847844535](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/77)

P3P通过三角形相似原理，将问题最终转化为3D-3D的位姿估计问题，但是存在问题

- P3P只能利用3个点的信息，当给定的配对点多于3组时，难以利用更多的信息
- 如果3D或者2D点受到噪声影响，或者存在误匹配，则算法失效

#### 4.4 非线性优化解决PNP

思想：前面说的线性方法，往往都是先求相机位姿，再求空间点的位置，非线性优化则是把它们都看成优化变量，放在一起优化，因此可以构建一个定义于李代数上的**非线性最小二乘问题**。

目标函数定义分析：

![1661848801547](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/78)

已知的是通过模型计算出的**“预测”投影像素坐标**以及**“实际”投影的像素坐标**：

![1661850790632](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/79)

针对目标函数进行求解：

![1661872700806](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/80)

![1661872831383](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/81)

重投影误差的来源和计算思路：

![1661872870947](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/82)

除去优化位姿(R和t)，还希望优化特征点的空间位置，则需要讨论e关于空间点P的导数：

![1661873235177](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/83)

#### 4.5 ICP

ICP是根据前后两帧图像中匹配好的**特征点**在**相机坐标系**下的**三维图标**，求解**相机帧间运动**的一种算法。

问题描述以及问题求解：

![1661873455396](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/84)

##### 4.5.1 SVD求解方法

定义误差与目标函数：

![1661873807442](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/85)

优化，先对t进行求偏导，消去t后带入原式继续化简：

![1661874117856](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/86)

通过公式对R进行进一步的简化：

![1661874527667](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/87)

得出最后的R矩阵，应该为uv的转置

##### 4.5.2 非线性优化方法

算法和求解ICP的特点如下：

![1661874761481](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/88)



## 四、ch8-视觉里程计2直接法

### 1.引入

目前特征点法存在的问题：

![1661913494444](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/89)

解决方案：

保留特征点，但只计算关键点，不计算描述子。使用**光流法**跟踪特征点的运动。把匹配描述子替换成了光流追踪，但这依然要求我们对提取到的关键点具有可区别性。

只计算关键点，不计算描述子。使用**直接法**计算特征点在下一时刻图像中的位置，这样可以省去光流时间。根据图像的**像素灰度信息**同时估计相机运动和点的投影。在直接法中，并不需要像特征点法中那样，通过最小化重投影误差来优化相机运动，并不需要知道点与点之间的对应关系，而是可以通过**最小化光度误差**来求得他们。

直接法根据像素的亮度信息估计相机的运动，可以完全不用计算关键点和描述子，于是,既避免了特征的计算时间,也避免了特征缺失的情况。只要场景中存在明暗变化(可以是渐变,不形成局部的图像梯度),直接法就能工作。根据使用像素的数量，直接法分为稀疏、稠密和半稠密三种。与特征点法只能重构稀疏特征点（稀疏地图）相比，直接法还具有恢复稠密或半稠密结构的能力。

### 2.2D光流

光流是一种描述像素随时间在图像之间运动的方法。计算部分像素运动的称为**稀疏光流**，计算所有像素的称为**稠密光流**。SLAM中主要使用以**LK光流**为代表的稀疏光流，可以追踪特征点的位置。

#### 2.1 LK光流

定义：

![1661921313428](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/90)

把图像看成了关于位置与时间的函数，它的值域就是图像中像素的灰度。

光流法的基本假设-**灰度不变假设**：同一个空间点的像素灰度值，在各个图像中是固定不变的。

由该假设可以以下关系式：

![1661921771116](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/91)

注意灰度不变假设是一个很强的假设,实际中很可能不成立。事实上,由于物体的材质不同,像素会出现高光和阴影部分;有时，相机会自动调整曝光参数，使得图像整体变亮或变暗。这时灰度不变假设都是不成立的，因此光流的结果也不一定可靠。然而，从另一方面来说，所有算法都是在一定假设下工作的。如果我们什么假设都不做，就没法设计实用的算法。所以，让我们暂且认为该假设成立,看看如何计算像素的运动。

![1661921935302](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/92)

我们想计算的是像素的运动u,v，但是该式是带有两个变量的一次方程，仅凭它无法计算出u,v。因此，必须引入额外的约束来计算u,u。在LK光流中，我们假设**某一个窗口内的像素具有相同的运动**。

因此可以得到方程：

![1661922200644](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/93)

这是一个关于u,v的超定线性方程，传统解法是求最小二乘解。最小二乘经常被用到:

![1661922257116](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/94)

这样就得到了像素在图像间的运动速度u, v。当t取离散的时刻而不是连续时间时，我们可以估计某块像素在若干个图像中出现的位置。由于像素梯度仅在局部有效、所以如果一次迭代不够好，我们会多迭代几次这个方程。在SLAM中，LK光流常被用来跟踪角点的运动。

光流法可以加速基于特征点的视觉里程计算法，避免计算和匹配描述子的过程,但要求相机运动较平滑(或采集频率较高)。

### 2.直接法

在光流中，我们会首先追踪特征点的位置，再根据这些位置确定相机的运动。这样一种两步走的方案，很难保证全局的最优性。但是能不能在后一步中，调整前一步的结果呢?例如，如果认为相机右转了15°，那么光流能不能以这个15°运动作为初始值的假设，调整光流的计算结果呢?直接法就是遵循这样的思路得到的结果。

如下图中：

![1661928388956](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/95)

![1661928404446](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/96)

![1661928417376](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/97)

在直接法中，由于没有特征匹配，我们无从知道哪一个p2与p1对应着同一个点。直接法的思路是根据当前相机的位姿估计值寻找p2的位置。但若相机位姿不够好,p2的外观和p1会有明显差别。于是，为了减小这个差别，我们优化相机的位姿，来寻找与p1更相似的p2。这同样可以通过解一个优化问题完成，但此时最小化的不是重投影误差，而是** **，也就是P的两个像素的亮度误差:

![1661928621497](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/98)

注意，这里的优化变量是相机位姿T，而不像光流那样优化各个特征点的运动。为了求解这个优化问题，我们关心误差e是如何随着相机位姿T变化的，需要分析它们的导数关系。设定中间量，并经过泰勒展开和求导，得到雅可比矩阵：

![1661928883266](https://github.com/LinkWithMe/SNN-learnabc/blob/main/Work1/image/99)

然后使用高斯牛顿法或者列文伯格-马夸尔特方法计算增量。

直接法的优点：

- 可以省去计算特征点、描述子的时间。
- 只要求有像素梯度即可，不需要特征点。
- 可以构建半稠密乃至稠密的地图，这是特征点法无法做到的。

缺点：

- 非凸性。非凸性。直接法完全依靠梯度搜索，降低目标函数来计算相机位姿。其目标函数中需要取像素点的灰度值，而图像是强烈非凸的函数。这使得优化算法容易进入极小，只在运
- 单个像素没有区分度。直接法在选点时通常需要500个以上的点。
- 灰度值不变是很强的假设，在实际使用过程中会受到很多因素影响。
