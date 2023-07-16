# 机器人学——学习笔记5(顺向运动学&DH表达法)

## 1. 定义

- **运动学（Kinematics）: 讨论运动状态本身，未连接到产生运动的「力」**
- 位置(x)，速度(v), 加速度(a)与时间(t)之间的关系：

$$
v = \frac{d}{dt}x \qquad a = \frac{d}{dt}v \\
a = \frac{d^{2}}{dt^{2}}x \quad vdv = ads
$$

- 移动——位置、速度、加速度；
- 转动——姿态、角速度、角加速度；
- **动力学（Dynamics）: 讨论力/力矩如何产生运动**
- Newton's 2nd Law:

$$
\sum F = ma
$$

- Work & Energy 功、能法：

$$
T_{1} + V_{1} + U_{1- 2}^{'} = T_{2} + V_{2}
$$

<p align="center"><span style="color: gray; font-size: 15px;">能量守恒</span></p>

- Impulse & Momentum 冲量、动量守恒：

$$
\int \sum F dt = G_{2} - G_{1}
$$

<p align="center"><span style="color: gray; font-size: 15px;">合力对时间的积分=动量的变化量</span></p>

### 1.1 机械手臂

- 多个杆件(link)相串联，具有复杂的几何外形；
- 杆件间可以相对移动(prismatic)或转动(revolute)，由驱动器来达成：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\MachineArm.png "只考虑平面运动")

<p align="center"><span style="color: gray; font-size: 15px;">只考虑平面运动</span></p>

### 1.2 对应关系

- 需求： **手臂末端点状态(**位置：P，速度...)
- 达成方式：驱动各个致动器：$$ ^{W}P = f(\theta_{1}, \theta_{2}, \dots, \theta_{n}) $$
- 若反过来，由期望手臂末端点状态，反推各个驱动器状态——逆向运动学

### 1.3 描述手臂状态方法

- 找出杆件间的相对几何状态
- 在各个杆件上**建立frame**，以**frame状态**来代表杆件状态

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\1.3.png)



## 2. 手臂几何描述方式

### 2.1 Joint（关节）

- 移动——prismatic
- 转动——revolute
- 每个revolute或prismatic的joint具有1DOF——简化
- 每个joint对 **某特定的axis** 进行rotation或 translation

### 2.2 Link（杆件）

- 连接joints的杆件，为刚体（rigid body）
- 编号方式：
  - Link 0: 地杆，不动的杆件
  - Link 1: 和Link0相连，第一个可动的杆件
  - Link 2: 第二个可动的杆件
  - ... 依序

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\link.png)

### 2.3 一般情况

针对空间中2个任意方向的axes，两axes之间具有一线段和此2个axes都相互垂直。Link Length即为线段长度，再加上两个axes之间的角度差，即为Link Twist（连杆扭角）\

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\LinkTwist.png)

<p align="center"><span style="color: gray; font-size: 15px;">若要找到相邻Link Length和Link Twist的关系，还需要知道别的条件</span></p>

若要**多杆串联，**则另需要两个参数$$ d_{i}, \theta{i} $$，来描述相邻线段$$ a_{i}, a_{i - 1} $$之间的关系。

- $$d_{i}$$: Link Offset;
- $$ \theta _{i} $$: Joint Angle
- 若Axis i 是转动轴，即Revolute Joint，则$$ (\alpha_{i}, a_{i}, d_{i}, \theta{i}) $$中，只有$$ \theta _{i} $$是变化的
- 若Axis i 是移动轴，即Prismatic Joint，则$$ (\alpha_{i}, a_{i}, d_{i}, \theta{i}) $$中，只有$$d_{i}$$是变化的

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\duoganchuanlian.png)

### 2.4 在杆件上建立Frame

- Z: 转动或移动axis的方向；
- X: 沿着$$\alpha$$的方向($$ if \hspace{0.5em} \alpha \neq 0 $$)，和$$ Z_{i}, Z_{i - 1} $$两者都垂直($$ if \hspace{0.5em} \alpha \neq 0 $$)
- Y: 与X和Z两者垂直，遵循右手定则；

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\frameonLink.png)

### 2.5 特殊情况1——地杆Link(0)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\Link0.png)

此时：

- Frame {0} coincides with frame {1} 即：$$ a_{0} = 0 \hspace{0.4em} \& \hspace{0.4em} \alpha_{0} = 0  $$

- 若 1 是 Revolute Joint: $$\theta_{1}$$任意，$$d_{1} = 0$$
- 若 1 是 Prismatic Joint: $$d_{1}$$任意，$$\theta_{1} = 0$$

### 2.6 特殊情况2——最后一个Link(n)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\linkn.png)

- $$X_{n}$$与$$X_{n - 1}$$同方向， $$a_{n} = 0 \hspace{0.4em} \& \hspace{0.4em} \alpha_{n} = 0$$
- 若 n 是 Revolute Joint: $$\theta_{n}$$任意，$$d_{n} = 0$$
- 若 n 是 Prismatic Joint: $$d_{n}$$任意，$$\theta_{n} = 0$$



## 3. DH表达法——Denavit-Hartenberg(Craig Version)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\5.Manipulator_Forward_Kinematics\images\DH.png)

- $$\alpha_{i-1}$$ : 以$$ X_{i-1} $$方向看，是$$Z_{i - 1}, Z_{i}$$间的夹角；（逆时针为正）
- $$a_{i-1}$$: 沿着$$ X_{i-1} $$方向，是$$Z_{i - 1}, Z_{i}$$间的距离；
- $$\theta_{i}$$:  以$$Z_{i}$$方向看，$$X_{i-1}, X_{i}$$之间的夹角；（逆时针为正）
- $$d_{i}$$:  沿着$$Z_{i}$$方向，$$X_{i-1}, X_{i}$$之间的距离