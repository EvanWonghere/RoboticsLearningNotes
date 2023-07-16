# 机器人学——学习笔记4(Operators)

## 1. Operators——对向量（或点）进行移动或转动

(1) 仅有移动：从P1 -> P2

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\11.png)

(2) 仅有转动：从P1 -> P2

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\12.png)

(3) 移动转动复合：P1→P12→P2 **注意，<u>先转动后移动≠先移动后转动</u> 先移后转 移动后的向量也要乘上旋转矩阵。**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\13.png)

### EX : Point $$ P_{1} = [3, 7, 0]^{T} $$先对Z轴转$$ 30^{\circ} $$，然后移动$$ [10, 5, 0]^{T} $$到$$P_{2}$$，求$$P_{3}$$

$$
\begin{bmatrix}
^{A}P_{2} \\
1 \\
\end{bmatrix}
=
\begin{bmatrix}
R_{\hat{K}}(\theta) & ^{A}Q \\
0 \hspace{0.2em} \hspace{0.2em} 0 \hspace{0.2em} 0 & 1 \\
\end{bmatrix}
\begin{bmatrix}
^{A}P_{1} \\
1 \\
\end{bmatrix}
=
\left[\begin{array}{c c c : c}
\frac{\sqrt{3}}{2} & -\frac{1}{2} & 0 & 10 \\
\frac{1}{2} & \frac{\sqrt{3}}{2} & 0 & 5 \\
0 & 0 & 0 & 1 \\
\hdashline
0 & 0 & 0 & 1 \\
\end{array}\right]
\begin{bmatrix}
3 \\ 7 \\ 0 \\ 1 \\
\end{bmatrix}
=
\begin{bmatrix}
9.098 \\ 12.562 \\ 0 \\ 1 \\
\end{bmatrix}
=
\begin{bmatrix}
| \\ ^{A}P_{2} \\ | \\ 1 \\
\end{bmatrix}
$$

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\EX1.png)

**<u>注意：</u>**Mapping，是把一个vector（point）从一个frame的表达，转动另外一个frame表达；Operator是对一个vector（point）进行一系列旋转平移操作。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\AT1.png "**注意：**Mapping，是把一个vector（point）从一个frame的表达，转动另外一个frame表达；Operator是对一个vector（point）进行一系列旋转平移操作。")

### 因为运动是相对的，Transformation Matrix对向量进行移动或转动操作，也可以想象成是<u>对frame进行「反向」的移动或转动的操作</u>。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\OP!.png)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\EX2.png)



## 2. Transformation Matrix的三种用法小结

1. 描述**一个frame相对一另一个frame**的空间状态：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\21.png)

2. 将point有某一个frame的表达转到另一个frame表达——**MAPPING！**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\22.png)

3.  将point(vector) 在同一个frame中进行移动和转动——**OPERATORS!**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\23.png)



## 3. Transformation 运算法则

### 3.1 连续运算

$$
 ^{A}P = ^{A}_{B}T^{B}P = ^{A}_{B}T(^{B}_{C}T^{C}P) = ^{A}_{B}T^{B}_{C}T^{C}P
$$

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\311.png)

列成Transformation Matrix的形式：
$$
\begin{align}
	&=
	\left[\begin{array}{c : c}
		^{A}_{B}R & ^{A}P_{B \hspace{0.2em} org} \\
		\hdashline
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1 \\
	\end{array}\right]
	\left[\begin{array}{c : c}
		^{B}_{C}R & ^{B}P_{C \hspace{0.2em} org} \\
		\hdashline
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1 \\
	\end{array}\right]
	{^{C}P} \\
	\\&=
	\begin{bmatrix}
		^{A}_{B}R^{B}_{C}R & ^{A}P_{B \hspace{0.2em} org} + ^{A}_B{}R^{B}P_{C 		\hspace{0.2em} org} \\
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1 \\
	\end{bmatrix}
	= ^{A}_{C}T^{C}P
\end{align}
$$
同理：若还有{D} frame:
$$
^{A}P_{B \hspace{0.2em} org} + ^{A}_{B}R^{B}P_{C \hspace{0.2em} org} + ^{A}_{B}R^{B}_{C}R^{C}P_{D \hspace{0.2em} org}
$$

$$
\begin{align}
	^{A}P &= ^{A}_{B}T^{B}_{C}T^{C}_{D}T^{D}P \\
	&=
	\begin{bmatrix}
		^{A}_{B}R^{B}_{C}R^{C}_{D}R & ^{A}P_{B \hspace{0.2em} org} + ^{A}_{B}R^{B}P_{C \hspace{0.2em} org} + ^{A}_{B}R^{B}_{C}R^{C}P_{D \hspace{0.2em} org} \\
		0 \hspace{1em} 0 \hspace{1em} 0 & 1 \\
	\end{bmatrix}
	= ^{A}_{D}T^{D}P
\end{align}
$$

<p align="center"><span style="color: gray; font-size: 15px;">越是后面的向量，越要乘上越多的旋转矩阵，最终回到Aframe上</span></p>

### 3.2 Transformation Matrix的逆矩阵

**<u>复习</u>**: 在学习Rotation Matrix的时候，我们发现，R的反矩阵就是其转置。那Transformation Matrix 的逆矩阵应该怎么求呢？
$$
逆矩阵 \quad ^{A}_{B}T =
\begin{bmatrix}
	^{A}_{B}R & ^{A}P_{B \hspace{0.2em} org} \\
	0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1\\
\end{bmatrix}
\qquad ^{B}_{A}T = ^{A}_{B}T^{-1} = ? \\
\\
\begin{align}
	&{^{A}_{B}T}{^{B}_{A}T} = {^{A}_{B}T}{^{A}_{B}T^{-1}} = I_{4 \times 4} \\
	&\begin {bmatrix}
		^{A}_{B}R & ^{A}P_{B \hspace{0.4em} org} \\
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1\\ 
	\end{bmatrix}
	\begin {bmatrix}
		^{B}_{A}R & ^{B}P_{A \hspace{0.4em} org} \\
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1 \\ 
	\end{bmatrix} \\
	\\&=
	\begin{bmatrix}
		^{A}_{B}R^{B}_{A}R & ^{A}P_{B \hspace{0.4em} org} + ^{A}_{B}R ^{B}P_{A \hspace{0.4em} org} \\
		0 \hspace{0.4em} 0 \hspace{0.4em} 0 & 1\\ 
	\end{bmatrix}
	=
	\begin{bmatrix}
		& & & 0 \\
		& I_{3 \times 3} & & 0 \\
		& & & 0 \\
		0 & 0 & 0 & 1\\
	\end{bmatrix}\\
\end{align}
$$

$$
\begin{align}
	&^{A}_{B}R^{B}_{A}R = I_{3 \times 3} \qquad ^{A}P_{B \hspace{0.4em} org} + ^{A}_{B}R ^{B}P_{A \hspace{0.4em} org} = 0 \\
	\\&\Rightarrow ^{B}_{A}R = ^{A}_{B}R^{T} \qquad \Rightarrow ^{B}P_{A \hspace{0.2em} org} = {-^{A}_{B}R^{T}}{^{A}P_{B \hspace{0.2em} org}} \\
	\\&\Longrightarrow ^{A}_{B}T^{-1} =
	\begin{bmatrix}
		^{A}_{B}R^{T} & {-^{A}_{B}R^{T}}{^{A}P_{A \hspace{0.2em} org}} \\
		0 \hspace{0.8em} 0 \hspace{0.8em} 0 & 1 \\ 
	\end{bmatrix}
\end{align}
$$

### 3.3 连续运算2——求未知的相对关系

任意一个frame未知，都可以利用其它已知的frame的相对关系求出这个未知。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\331.png)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\4.Operators\images\332.png)

连续运算，就是对矩阵“搬来搬去”的运算，所以前面需要求逆矩阵，有了逆矩阵，所有的运算都很方便了。

### 3.4 连续运算法则

- Initial condition: **{A}** and **{B}** coincide: (初始A，B两个frame 重合)

$$
^{A}_{B}T = I_{4 \times 4}
$$

- **{B}** 对 **{A}** 的转轴旋转：用"***Premultiply***"(自左乘)

  - 以operator来想，对某一个向量「以同一坐标为基准」，进行转动或移动的操作

  $$
  \textcolor{blue}{Ex: {B}依序经过T_{1},T_{2},T_{3}三次transformation} \\
  \textcolor{blue}{^{A}_{B}T = T_{3}T_{2}T_{1}I \qquad v^{'} = ^{A}_{B}Tv = T_{3}T_{2}T_{1}v}
  $$

  

- **{B}** 对 **{B}** 的自身转轴旋转：用"***Postmultiply***"(自右乘)
  - 以mapping来想，对某一个向量，从最后一个frame「逐渐转动或移动」来回到第一个frame

$$
\textcolor{blue}{Ex: {B}依序经过T_{1},T_{2},T_{3}三次transformation} \\
\textcolor{blue}{^{A}_{B}T = IT_{1}T_{2}T_{3} \qquad ^{A}P = ^{A}_{B}T^{B}P = IT_{1}T_{2}T_{3}^{B}P}
$$

- 以**固定的{A}**或**移动的{B}**为基准进行移动转动操作，transformation matrix应用不同的连乘方式。