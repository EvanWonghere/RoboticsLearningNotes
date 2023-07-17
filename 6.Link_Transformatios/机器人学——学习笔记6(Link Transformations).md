# 机器人学——学习笔记6(Link Transformations)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\img0.png)

## 进一步精准找到两个frame之间的变换关系（Transformation Matrix的量化表达式什么）

要如何借由DH表达法中的4个参数，求得Transformation Matrix?
$$
^{i-1}P = ^{i - 1}_{i}T^{i}P
$$

- step 1: 沿着$$X_{i - 1}$$ 进行旋转，使得$$ Z_{i-1} \rightarrow Z_{R} $$；其中$$Z_{R}$$ 与$$Z_{i}$$平行；
- step 2: 沿着$$X_{R}$$平移{R}， 使得平移后的$$Z_{Q}$$ 与$$Z_{i}$$重合;
- step 3: 沿着$$Z_{Q}$$旋转{Q}，使得旋转后的$$X_{P}$$ 与$$X_{i}$$平行；
- step 4: 沿着$$Z_{P}$$​平移{P}， 使得平移后{P}与{i}两个frame重合，完成变换。
- 过程图示如下：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\DH2TM.png)

- 其中，按照mapping后乘思想，可列出：

$$
^{i-1}P = ^{i-1}_{R}T^{R}_{Q}T^{Q}_{P}T^{P}_{I}T^{i}P
$$

- 继续拆解：

  - **step1若要满足，则需要旋转角度为：$$\alpha_{i-1}$$ **;
  - **step2若要满足，则需要平移距离为：** $$a_{i-1}$$;
  - **step3若要满足，则需要旋转角度为：**$$\theta_{i}$$;
  - **step4若要满足，则需要平移距离为**：$$d_{i}$$ 。
  - 有：

  $$
  \begin{align}
  ^{i-1}_{i}T &= ^{i-1}_{R}T^{R}_{Q}T^{Q}_{P}T^{P}_{i}T \\
  &= T_{\hat{X}_{i-1}}(\alpha_{i-1})T_{\hat{X}}(a_{i-1})T_{\hat{Z}_{Q}}(\theta_{i})T_{\hat{Z}_{P}}(d_{i})
  \end{align}
  $$

  

  <p align="center"><span style="color: gray; font-size: 15px;">连接{i}与{i-1}两个frame之间的关系</span></p>

  - Thus:

  $$
  \begin{align}
  ^{i-1}_{i}T &= T_{\hat{X}_{i-1}}(\alpha_{i-1})T_{\hat{X}}(a_{i-1})T_{\hat{Z}_{Q}}(\theta_{i})T_{\hat{Z}_{P}}(d_{i}) \\
  &=
  \begin{bmatrix}
  	c\theta_{i} & -s\theta_{i} & 0 & a_{i-1} \\
  	s\theta_{i}c\alpha_{i-1} & c\theta_{i}c\alpha_{i-1} & -s\alpha_{i-1} & -s\alpha_{i-1}d_{i} \\
  	s\theta_{i}s\alpha_{i-1} & c\theta_{i}s\alpha_{i-1} & c\alpha_{i-1} & c\alpha_{i-1}d_{i} \\
  	0 & 0 & 0 & 1 \\
  \end{bmatrix}
  \end{align}
  $$

  

  <p align="center"><span style="color: gray; font-size: 15px;">省略了推导过程</span></p>

  - 有了i与i-1的frame的转换关系后，连续的link transformations也很好求：

  $$
  ^{0}_{n}T = ^{0}_{1}T^{1}_{2}T^{2}_{3}T \dots ^{n-2}_{n-1}T^{n-1}_{n}T
  $$

  <p align="center"><span style="color: gray; font-size: 15px;">任何一个杆件对地状态</span></p>

  **Frame{n}相对于Frame{0}的空间集合关系清楚，且量化的定义在Frame{n}下表达的向量<u>可以转回到Frame{0}下来定义表达</u>（对地）。**

## Example 1

上面的详细推导过程：

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\EX!.png)

## Example 2: A RRR Manipulator

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\EX2.png)

## Example 3: A RPR Manipulator

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\EX3.jpg)

注意，当Z轴相交，即 $$a_{1} = 0$$ 的时候，有很多定义方式：

- $$Z_{2}$$有两个选择
- $$X_{1}$$有两个选择
- 单纯的两轴相交，就有4种选择，不是唯一解...

![](C:\Users\EvanWong\Desktop\nao211\Robotics\6.Link_Transformatios\images\final.webp)