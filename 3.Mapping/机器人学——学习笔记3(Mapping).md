# 机器人学——学习笔记3(Mapping)



## 1. 复习  该如何整合表达刚体的状态

在刚体（Rigid Body）上建立frame，常建立在质心上

- 移动：由body frame的「**原点位置**」判定；

$$
^{A}P_{B \hspace{0.2em} org} = 
\begin{bmatrix}
P_{B \hspace{0.2em} x} \\
P_{B \hspace{0.2em} y} \\
P_{B \hspace{0.2em} z} \\
\end{bmatrix}
= origin \hspace{0.5em} of \hspace{0.5em} \{B\} \hspace{0.5em} represented \hspace{0.5em} in \hspace{0.5em} \{A\}
$$



- 转动：由body frame的「**姿态**」判定；

$$
^{A}_{B}R = 
\begin{bmatrix}
\hat{X}_{B} \cdot \hat{X}_{A} & \hat{Y}_{B} \cdot \hat{X}_{A} & \hat{Z}_{B} \cdot \hat{X}_{A} \\
\hat{X}_{B} \cdot \hat{Y}_{A} & \hat{Y}_{B} \cdot \hat{Y}_{A} & \hat{Z}_{B} \cdot \hat{Y}_{A} \\
\hat{X}_{B} \cdot \hat{Z}_{A} & \hat{Y}_{B} \cdot \hat{Z}_{A} & \hat{Z}_{B} \cdot \hat{Z}_{A} \\
\end{bmatrix}
=
\begin{bmatrix}
| & | & | \\
^{A}\hat{X}_{B} & ^{A}\hat{Y}_{B} & ^{A}\hat{Z}_{B} \\
| & | & | \\
\end{bmatrix}
$$

- 整合后：

$$
{\{B\} = \{^{A}_{B}R, ^{A}P_{B \hspace{0.2em} org}\}}_{\textcolor{green}{但无法进行量化计算}}
$$

如何整合转动与移动，并可以进行量化计算？——把移动和转动排列在同一个矩阵里面，即Homogeneous Transformation Matrix(齐次变换矩阵)。
$$
\begin{align}
&{\left[
\begin{array}{c : c}
{^{A}_{B}R}_{\hspace{0.7em} 3 \times3} & {^{A}P_{B \hspace{0.2em} org { \hspace{0.7em} 3 \times 3}}} \\
\hdashline
0 \quad 0 \quad 0 & 1
\end{array}
\right]}_{4 \times 4} \\
\\&=
\left[
\begin{array}{c c c: c}
| & | & | & | \\
^{A}\hat{X}_{B} & ^{A}\hat{Y}_{B} & ^{A}\hat{Z}_{B} & ^{A}_{B}P_{B \hspace{0.2em} org} \\
| & | & | & | \\
\hdashline
0 & 0 & 0 & 1 \\
\end{array}
\right]\\
\\&= ^{A}_{B}T
\end{align}
$$

## 2. Mapping

以Mapping，转换向量（或点）的坐标系的方式来确认T的正确性

![21](C:\Users\EvanWong\Desktop\nao211\Robotics\3.Mapping\images\21.png "（1）仅有移动，等价，说明Trasformation Matrix没毛病")

<p align="center"><span style="color: gray; font-size: 15px;">（1）仅有移动，等价，说明Trasformation Matrix没毛病</span></p>

![22](C:\Users\EvanWong\Desktop\nao211\Robotics\3.Mapping\images\22.png "（1）仅有移动，等价，说明Trasformation Matrix没毛病")

<p align="center"><span style="color: gray; font-size: 15px;">（2）仅有转动，等价，说明Trasformation Matrix没毛病</span></p>

![23](C:\Users\EvanWong\Desktop\nao211\Robotics\3.Mapping\images\23.png "（3）转动+移动（一般情况），等价，说明Trasformation Matrix没毛病")

<p align="center"><span style="color: gray; font-size: 15px;">（3）转动 + 移动（一般情况），等价，说明Trasformation Matrix没毛病</span></p>

注意， Transformation Matrix也可以连续操作：
$$
^{A}_{B}T = ^{A}_{C}T ^{C}_{D}T ^{D}_{B}T \\
\textcolor{green}{Sequential \hspace{0.6em} Transformation}
$$

### EX 从mapping的角度，把一个point，从相对于{B}下的坐标转换到{A}下:

![EX](C:\Users\EvanWong\Desktop\nao211\Robotics\3.Mapping\images\EX.png "EX")