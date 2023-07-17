# 机器人学——学习笔记7(Link Trans Examples & Summarization)

## Example 1: A PRRR Manipulator 晶圆机器人

1个translation,3个rotation,$$d_{1}, \theta_{2}, \theta_{3}, \theta_{4}$$为驱动关节。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX1.png)

<p align="center"><span style="color: gray; font-size: 15px;">晶圆机器人DH Table</span></p>

## Example 2:  A RRRP Manipulator SCARA机器人

3个rotation,1个translation $$ \theta_{1}, \theta_{2}, \theta_{3}, d_{4} $$ 为驱动关节。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX2.jpg)

<p align="center"><span style="color: gray; font-size: 15px;">SCARA机器人 DH Table</span></p>

可以发现，这类比较简单的架构下，可以直接读图（目视法）来建立DH Table。

## In-Video QUIZ RP 手臂

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\IVQUIZRP.png)

## 本章小结：

### 1. Actuator, Joint, and Cartesian Spaces

Cartesian Space: 一个点在世界坐标系下，用一个直角坐标系来表示；

Joint Space: 有多少驱动关节，有多少自由度。

- **Forward Kinematics 顺向运动学**

从Joint Space$$(\theta_{1}, \theta_{2}, \dots, \theta_{n})$$顺向推出**末端执行器的**$$W_{P}$$的Cartesian space。

- **Inverse Kinematics 逆向运动学**

从**末端执行器的**$$W_{P}$$的Cartesian space反推出Joint Space$$(\theta_{1}, \theta_{2}, \dots, \theta_{n})$$状态。

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\kinematics.png)

<p align="center"><span style="color: gray; font-size: 15px;">顺、逆运动学</span></p>

- **Actuator Space 与 Joint Space**
  - **由连接制动器和joint的机构决定；**
  - Motor转角与Joint转角的关系→末端执行器的位置。(Motor后一般会带一个reducer减速器)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\sum1.png)

## **Example3（For** Actuator Space 与 Joint Space**）: A leg-wheel transformable robot**

**轮式末端与脚式末端的自由度不一样：**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX31.png)

详细分析：轮式一般比较简单，现在主要分析一下“脚式”：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX32.png)

<p align="center"><span style="color: gray; font-size: 15px;">脚式，需要2个自由度，旋转与平移</span></p>

- Kinematic Mapping
- 输入：Motor Speeds: $$ \phi_{1}^{'}, \phi_{2}^{'} $$

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX33.png)

<p align="center"><span style="color: gray; font-size: 15px;">Motor皮带轮带动外部实现转动</span></p>

![](C:\Users\EvanWong\Desktop\nao211\Robotics\7.Link_Trans_Examples&Summarization\images\EX34.png)

<p align="center"><span style="color: gray; font-size: 15px;">Motor+齿轮齿条，转换为直线运动</span></p>

- 输出：脚轮的运动：$$ \theta^{'}, r^{'} $$ ，在极坐标系中，存在以下关系：

$$
\dot{\xi} =
\begin{bmatrix}
	\dot{\theta} \\
	\dot{\gamma} \\
\end{bmatrix}
= 
\begin{bmatrix}
	1 & 0 \\
	-p & p \\
\end{bmatrix}
\begin{bmatrix}
	\dot{\phi_{1}} \\
	\dot{\phi_{2}} \\
\end{bmatrix}
$$

其中:$$ [\theta^{'}, \gamma^{'}]^{T} $$ 为Joint Space，$$ [\phi_{1}^{'}, \phi_{2}^{'}]^{T} $$为Actuator Space**，这是一个经典的Actuator Space ↔ Joint Space转换的例子**。