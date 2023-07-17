# 机器人学——学习笔记8(DH表达法总结)

## 1. Review: Denavit-Hartenberg表达法(Crag Version)

- $$ \textcolor{orange}{\alpha_{i-1}} $$：$$ \textcolor{blue}{以\hat{X}_{i-1}方向看，\hat{Z}_{i-1}和\hat{Z}_{i}之间的夹角} $$
- $$ \textcolor{green}{a_{i-1}} $$：$$ \textcolor{blue}{沿着\hat{X}_{i-1}方向，\hat{Z}_{i-1}和\hat{Z}_{i}之间的距离(a_{i} > 0)} $$
- $$ \textcolor{red}{\theta_{i}} $$：$$ \textcolor{blue}{以\hat{Z}_{i}方向看，\hat{X}_{i-1}和\hat{X}_{i}之间的夹角} $$
- $$ \textcolor{purple}{d_{i}} $$：$$ \textcolor{blue}{沿着\hat{Z}_{i}方向，\hat{X}_{i-1}和\hat{X}_{i}之间的距离} $$

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\geo_relation.jpg "几何关系")

<p align="center"><span style="color: gray; font-size: 15px;">1.几何关系</span></p>

在这个操作顺序下面：
$$
\begin{align}
\textcolor{red}{^{i-1}_{i}T} &\textcolor{blue}{=} \textcolor{green}{^{i-1}_{R}T} \textcolor{orange}{^{R}_{Q}T} \textcolor{purple}{^{Q}_{P}T} \textcolor{red}{^{P}_{i}T} \\
&\textcolor{blue}{=} \textcolor{green}{T_{\hat{X}_{i-1}(\alpha_{i-1})}} \textcolor{orange}{T_{\hat{X}_{R}}(a_{i-1})} \textcolor{purple}{T_{\hat{Z}_{Q}}(\theta_{i})} \textcolor{red}{T_{\hat{Z}_{P}}(d_{i})} \\
&\textcolor{blue}{
=
\begin{bmatrix}
c\theta_{i} & -s\theta_{i} & 0 & a_{i-1} \\
s\theta_{i}c\alpha_{i-1} & c\theta_{i}c\alpha_{i-1} & -s\alpha_{i-1} & -s\alpha_{i-1}d_{i} \\
s\theta_{i}s\alpha_{i-1} & c\theta_{i}s\alpha_{i-1} & c\alpha_{i-1} & c\alpha_{i-1}d_{i} \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
}
\end{align}
$$
这个表达法不是那么直观，因为只有 $$ \theta_{i}, d_{i} $$ 是在第 i 个link下面，而 $$ \alpha_{i-1}, a_{i-1} $$ 都是在第 i - 1 个Link下面。

## 2. Denavit-Hartenberg表达法(Standard)

- $$ \textcolor{red}{\theta_{i}} $$：$$ \textcolor{blue}{以\hat{Z}_{i-1}方向看，\hat{X}_{i-1}和\hat{X}_{i}之间的夹角} $$
- $$ \textcolor{purple}{d_{i}} $$：$$ \textcolor{blue}{沿着\hat{Z}_{i-1}方向，\hat{X}_{i-1}和\hat{X}_{i}之间的距离} $$
- $$ \textcolor{green}{a_{i}} $$：$$ \textcolor{blue}{沿着\hat{X}_{i}方向，\hat{Z}_{i-1}和\hat{Z}_{i}之间的距离(a_{i} > 0)} $$
- $$ \textcolor{orange}{\alpha_{i}} $$：$$ \textcolor{blue}{以\hat{X}_{i}方向看，\hat{Z}_{i-1}和\hat{Z}_{i}之间的夹角} $$

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\DH_STANDARD.png "每个Link所对应的Joint是放在这个Link的后方")

<p align="center"><span style="color: gray; font-size: 15px;">每个Link所对应的Joint是放在这个Link的后方</span></p>

与Crag Version的几个区别：

- Standard Ver更习惯用Joint而不是Axis;
- 每个Link所对应的Joint是放在这个Link的后方;
- $$X_{i}$$的定义不同， Std Ver下，$$X_{i}$$的方向是$$ Joint_{i-1} \rightarrow Joint_{i} $$的方向；
- 符号定义不同，$$Joint_{i}$$下，$$X_{i}$$与$$X_{i + 1}$$之间的夹角是$$\theta_{i + 1}$$;

采用Std Ver的好处，在进行Trans时，其Trans Matrix求法如下：
$$
\begin{align}
\textcolor{red}{^{i-1}_{i}T} &\textcolor{blue}{=} \textcolor{red}{^{i-1}_{R}T} \textcolor{purple}{^{R}_{Q}T} \textcolor{green}{^{Q}_{P}T} \textcolor{orange}{^{P}_{i}T} \\
&\textcolor{blue}{=} \textcolor{red}{T_{\hat{Z}_{i-1}}(\theta_{i})} \textcolor{purple}{T_{\hat{Z}_{R}}(d_{i})} \textcolor{green}{T_{\hat{X}_{Q}}(a_{i})} \textcolor{orange}{T_{\hat{X}_{P}}(\alpha_{i})} \\
&\textcolor{blue}{
=
\begin{bmatrix}
c\theta_{i} & -s\theta_{i}c\alpha_{i} & s\theta_{i}s\alpha_{i} & a_{i}c\theta_{i} \\
s\theta_{i} & c\theta_{i}c\alpha_{i} & -c\theta_{i}s\alpha_{i} & a_{i}s\theta{i} \\
0 &s\alpha_{i} & c\alpha_{i} & d_{i} \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
}
\end{align}
$$

- step1: 绕$$Z_{i-1}$$轴旋转$$\theta_{i}$$使$$X_{i-1}, X_{i}$$平行；
- step2: 沿$$Z_{R}$$移动$$d_{i}$$使$$Z_{R}, Z_{i}$$平行；
- step3: 沿$$X_{Q}$$移动$$a_{i}$$, 使移动后{P}frame与{i}frame重合；
- step4: 绕$$X_{P}$$轴旋转$$\alpha_{i}$$使$$Z_{i}, Z_{P}$$重合，完成变换;

## Example-1 Craig DH&Std DH方式表达差异(A RRR Manipulator)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\EX11.png)

Craig DH 情况下的Transformation Matrix分别是：( $$\theta$$ 用 $$t$$ 代替），**可以发现，Crag DH的一个优点就是关于Translation的描述会更加干净（与下面Std Ver对比）**。
$$
\textcolor{blue}{^{0}_{1}T} \qquad
\begin{pmatrix}
\cos{t_{1}} & -\sin{t_{1}} & 0 & 0 \\
\sin{t_{1}} & \cos{t_{1}} & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \
\end{pmatrix} \\
\\\textcolor{blue}{^{1}_{2}T} \qquad
\begin{pmatrix}
\cos{t_{2}} & -\sin{t_{2}} & 0 & L_{1} \\
\sin{t_{2}} & \cos{t_{2}} & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \
\end{pmatrix} \\
\\\textcolor{blue}{^{2}_{3}T} \qquad
\begin{pmatrix}
\cos{t_{3}} & -\sin{t_{3}} & 0 & L_{2} \\
\sin{t_{3}} & \cos{t_{3}} & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \
\end{pmatrix} \\
$$

<p align="center"><span style="color: gray; font-size: 15px;">该例题下的3个Trans Matrix</span></p>



现在把Trans Matrix乘开，进行进一步地分析

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\Craig1.png)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\Craig2.png)

<p align="center"><span style="color: gray; font-size: 15px;">最终Craig的形式，做一个L3的平移，才会跑到Std Ver 末端的原点</span></p>

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\std1.png)

<p align="center"><span style="color: gray; font-size: 15px;">殊途同归！！！</span></p>

可以发现，因为是取Link3相对于地的Trans Matrix，所以其**<u>旋转坐标部分一模一样</u>**；



## CLASSICAL EXAMPLE: PUMA 560

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\PUMA1.jpg)

<p align="center"><span style="color: gray; font-size: 15px;">6轴手臂，可以达到任意一点(6 DOF）</span></p>

1. **Craig 法： 先找Axis→Z→X(**<u>$$ Z_{1}, Z_{2} $$ 相交， $$X$$选择和两者都垂直的方向</u>）**→Y→<u>补上frame{0}与frame{n}</u>→确定几何关系→目视法确定Craig DH Table：**

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\PUMA_Craig1.jpg)
$$
\begin{array}{| c | c | c | c | c |}
\hline
i & \alpha_{i-1} & a_{i-1} & d_{i} & \theta_{i} \\
\hline
1 & 0^{\circ} & 0 & 0 & \theta_{1} \\
\hline
2 & -90^{\circ} & 0 & 0 & \theta_{2} \\
\hline
3 & 0^{\circ} & a_{2} & d_{3} & \theta_{3} \\
\hline
4 & -90^{\circ} & a_{3} & d_{4} & \theta_{4} \\
\hline
5 & 90^{\circ} & 0 & 0 & \theta_{5} \\
\hline
6 & -90^{\circ} & 0 & 0 & \theta_{6} \\
\hline
\end{array}
$$

<p align="center"><span style="color: gray; font-size: 15px;">Craig DH Table</span></p>
- $$\textcolor{blue}{Transformation \quad Matrices}$$
$$
\textcolor{blue}{
^{0}_{1}T = 
\begin{bmatrix}
c\theta_{1} & -s\theta_{1} & 0 & 0 \\
s\theta_{1} & c_{\theta_{1}} & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\qquad \qquad
^{3}_{4}T =
\begin{bmatrix}
c\theta_{4} & -s\theta_{4} & 0 & a_{3} \\
0 & 0 & 1 & d_{4} \\
-s\theta_{4} & -c\theta_{4} & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
}\\
\textcolor{blue}{
^{1}_{2}T =
\begin{bmatrix}
c\theta_{2} & -s\theta_{2} & 0 & 0 \\
0 & 0 & 1 & 0 \\
-s\theta_{2} & -c\theta_{2} & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\qquad \qquad
^{4}_{5}T =
\begin{bmatrix}
c\theta_{5} & -s\theta_{5} & 0 & 0 \\
0 & 0 & -1 & 0 \\
s\theta_{5} & c\theta_{5} & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
}\\
\textcolor{blue}{
^{2}_{3}T = 
\begin{bmatrix}
c\theta_{3} & -s\theta_{3} & 0 & a_{2} \\
s\theta_{3} & c_{\theta_{3}} & 0 & 0 \\
0 & 0 & 1 & d{3} \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
\qquad \qquad
^{5}_{6}T =
\begin{bmatrix}
c\theta_{6} & -s\theta_{6} & 0 & 0 \\
0 & 0 & 1 & 0 \\
-s\theta_{6} & -c\theta_{6} & 0 & 0 \\
0 & 0 & 0 & 1 \\
\end{bmatrix}
}\\
$$

<p align="center"><span style="color: gray; font-size: 15px;">变换矩阵</span></p>

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\TM1.png)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\TM2.webp)

![](C:\Users\EvanWong\Desktop\nao211\Robotics\8.DH_Summarization\images\TM3.png)

<p align="center"><span style="color: gray; font-size: 15px;">Final Result</span></p>
