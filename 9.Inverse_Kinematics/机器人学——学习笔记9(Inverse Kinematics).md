# 机器人学——学习笔记9(Inverse Kinematics)

## 机械手臂 逆向运动学——Manipulator Inverse Kinematics

### 1. 引言: 手臂顺向运动学 Forward Kinematics (FK)

给予$$\theta_{i}$$(可计算出$$^{i}_{i-1}T$$)，求得$$\{H\}$$或$$^{W}P$$。
$$
\begin{align}
^{W}_{H}T &= f(\theta_{1}, \dots, \theta_{i}, \dots, \theta_{n}) \\
\\
^{W}P &= ^{W}_{H}T^{H}P
\end{align}
$$

### 2. 手臂逆向运动学 Inverse Kinematics(IK)

给予$$\{H\}$$或$$^{W}P$$，求得$$\theta_{i}$$。已知末端位置，反算手臂各个关节的角度。
$$
[\theta_{1}, \dots, \theta_{i}, \dots, \theta_{n}] = f^{-1}(^{W}_{H}T)
$$

### 3. 逆向运动学的求解

- 假设手臂有6Dofs；
- 6个未知的Joint Angles( $$\theta_{i}$$或者$$d_{i}$$ ，$$i = 1, \dots, 6$$)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\31.png)

<p align="center"><span style="color: gray; font-size: 15px;">{0} frame手臂基座，{6} frame末端</span></p>

- 在 $$^{W}_{H}T$$ 中，取出含未知数的$$^{0}_{6}T$$ ，16个数字：

$$
^{0}_{6}T = 
{\left[\begin{array}{c : c}
^{0}_{6}R_{\hspace{0.4em}_{3\times 3}} & ^{0}P_{6 \hspace{0.4em} org_{\hspace{0.4em} 3 \times 3}} \\
0 \hspace{0.8em} 0 \hspace{0.8em} & 1 \\
\end{array}\right]}_{4 \times 4}
=
\left[\begin{array}{c c c : c}
| & | & | & | \\
^{0}\hat{X}_{6} & ^{0}\hat{Y}_{6} & ^{0}\hat{Z}_{6} & ^{0}P_{6 \hspace{0.4em} org} \\
| & | & | & | \\
\hdashline
0 & 0 & 0 & 1 \\
\end{array}\right]
$$

<p align="center"><span style="color: gray; font-size: 15px;">9个未知数代表旋转，3个未知量代表第{6}frame相对于{0}frame的位置</span></p>

其中旋转矩阵中，9个数字，有6个条件：

- **两两垂直；**
- **单位向量；**

所以在求解过程中：

- 12个 $$Nonlinear \hspace{0.5em} Transcendental \hspace{0.5em} Equations$$（非线性超越方程组）方程式；
- 6个未知数，6个限制条件；

### 4. 几个求解概念

- **<u>Reachable Workspace</u> 可达工作空间**
  - 手臂可以用一种以上姿态到达的位置；
- **<u>Dexterous Workspace</u> 灵活工作空间**
  - 手臂可以用任何的姿态到达的位置；
  - 是RW的子集合；

**EX: 2Dof 机械手臂 , 其中** $$l_{1} > l_{2}$$ ；

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\EX1.png)

<p align="center"><span style="color: gray; font-size: 15px;">蓝色圈区域都是Reachable Workspace</span></p>

蓝色空间内任何一点，只有两个姿态可以到达，这种情况下不存在Dexterous Workspace；

**EX:2Dof 机械手臂，其中** $$l_{1} = l_{2}$$ ；

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\EX2.png)

<p align="center"><span style="color: gray; font-size: 15px;">蓝色是Reachable Workspace，其中原点是任何姿态都可以到达的位置，是Dexterous Workspace</span></p>

- **Subspace** ——手臂在定义头尾的T所能到达的变动范围

**EX：A RP Manipulator —— 2Dof，Variables:(x,y)**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\EX31.png)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\EX32.png)

求$$^{W}_{2}T$$ （frame{2}相对于世界坐标）
$$
\hspace{11em}\textcolor{green}{^{0}\hat{Z}_{2}} \hspace{0.5em} \textcolor{orange}{^{0}P_{2 \hspace{0.4em} ORG}} \\
^{W}_{2}T =
\begin{bmatrix}
\frac{y}{\sqrt{x^{2} + y^{2}}} & 0 & \textcolor{green}{\frac{x}{\sqrt{x^{2} + y^{2}}}} & \textcolor{orange}{x} \\
\frac{-x}{\sqrt{x^{2} + y^{2}}} & 0 & \textcolor{green}{\frac{y}{\sqrt{x^{2} + y^{2}}}} & \textcolor{orange}{y} \\
0 & -1 & \textcolor{green}{0} & \textcolor{orange}{0} \\
0 & 0 & 0 & 1
\end{bmatrix}
$$
可以发现，该手臂，如果位置(x,y)决定了，其方向只与x,y有关，**所以方向有<u>唯一解</u>**。所以其他不满足该形式的位置，手臂均无法到达，会存在求解冲突。

### 5. 多重解

- 解读数目：由于式非线性超越方程组，6未知数、6方程式不代表具有唯一解；
- 是由Joint数目和Link的参数所决定；

$$
\textcolor{blue}{Ex: A \hspace{0.5em} RRRRRR \hspace{0.5em} manipulator} \\
\begin{array}{| c | c |}
\hline
a_{i} & 解的数目 \\
\hline
a_{1} = a_{3} = a_{5} = 0 & \leq 4 \\
\hline
a_{3} = a_{5} = 0 & \leq 8 \\
\hline
a_{3} = 0 & \leq 16 \\
\hline
All \hspace{0.5em} a_{i} \neq 0 & \leq 16 \\
\hline
\end{array}
$$

<p align="center"><span style="color: gray; font-size: 15px;">解的数目，与杆件几何的offset有很大关系e</span></p>

- 用一个实例来看会比较清楚；
- **EX: PUMA ( 6 Rotational Joints)**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\EXPUMA.webp)

<p align="center"><span style="color: gray; font-size: 15px;">PUMA 机械臂</span></p>

- **针对特定工作点，有8组解，下面进行详细分析（4x2=8组）**
- **前三轴，有4种姿态：**
  - 可以发现，前三轴基本可以决定“腕”的空间位置。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\PUMA.png)

- 每一个姿态中，具有**2组手腕转动姿态**：
  - $$ \theta^{'}_{4} = \theta_{4} + 180^{\circ} $$
  - $$ \theta^{'}_{5} = -\theta_{5} $$
  - $$ \theta_{6}^{'} = \theta_{6} + 180^{\circ} $$
- **P.S. 若手臂本身有几何限制，并非每一种解都可以运作。**
- **一般6DOFs手臂的形式如下（帮助理解）**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs1.webp)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs2.webp)

<p align="center"><span style="color: gray; font-size: 15px;">第五个是爪子的上下摆动</span></p>

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs3.png)

<p align="center"><span style="color: gray; font-size: 15px;">第六个自由度</span></p>

可以发现，当第五个摆角为0°时，第四个和第六个同轴。

- 前三轴决定到达位置；（双解特性）

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs4.webp)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs5.png)

- 特定位置只有单一解：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs6.png)

- 后三轴也具有多解性：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\6dofs7.webp)

<p align="center"><span style="color: gray; font-size: 15px;">4轴转180度具有相同末端姿态</span></p>

PUMA机械臂有8组的原因，是手臂的第二杆件存在侧移，侧移之后在左右两侧多了两个选择，所以一共是2x4=8组解。

#### 5.1 多重解的选择方式

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\511.png)

- **<u>选择方式一</u>**：选择下一个瞬间离目前状态最近的解：

  - 速度最快
  - 最节能
  - $$\dots$$

  ![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\512.png)

<p align="center"><span style="color: gray; font-size: 15px;">从A到B，选择上面姿态最快且节能</span></p>

- **<u>选择方式二</u>**：避障原则

![](C:\Users\EvanWong\Desktop\nao211\Robotics\9.Inverse_Kinematics\images\513.png)

#### 5.2 求解方法

1. **<u>解析法 Closed-form solutions</u>**

- 用代数**Algebraic**或几何**Geometric**方法

2.  **<u>数值法 Numerical solutions</u>** 六个方程，六个未知数

目前大多数机械手臂，设计成具有解析解:

- **Pieper's solution: 相邻三轴相交一点（后三轴）**

 