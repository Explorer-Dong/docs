## 一、前置信息

### 1.1 竞赛场所

仙林校区化成楼（S3）计算中心一楼机房，预计9月7日下午5点起，可供已报名的各队使用

### 1.2 客户端界面

#### 1.2.1 赛前

<img src="https://s2.loli.net/2023/09/05/27b9w65kVrjGO4q.png" alt="image-20230905215738412" style="zoom:50%;" />

#### 1.2.2 赛中

<img src="https://s2.loli.net/2023/09/08/IR2yNjxpLT3Wmf6.png" alt="image-20230908084300721" style="zoom:50%;" />

### 1.3 国赛论文模板

[国赛论文模板](https://explorer-dong.lanzoum.com/ig9TM17hp56h) 密码：8888

### 1.4 学生竞赛手册

[学生手册](https://explorer-dong.lanzoum.com/ifwrs17hp5ab) 密码：8888

## 二、正式比赛

### 2.1 问题一

#### 2.1.1 太阳高度角 $\alpha_s$ 计算公式

$$
\sin{\alpha_s}=\cos{\delta}\cos{\phi}\cos{\omega}+\sin{\delta}\sin{\phi}
$$

$$
\sin{\delta}=\frac{\sin{2\pi D}}{365}\sin\frac{2\pi}{360}23.45 \\
\phi=39.4\frac{\pi}{180} \\
\omega=\frac{\pi}{12}(ST-12)
$$

#### 2.1.2 阴影遮挡效率 $\eta_{\cos}$ 计算公式

$$
% 数据输入格式 ：两个定日镜的坐标、高度、太阳方位角、定日镜法向量
\text{Input:}x_1,y_1,h_1,x_2,y_2,h_2,\gamma_s,\overrightarrow{n} \\

% 定日镜俯仰角 
\cos \alpha = \overrightarrow{n} \cdot (0,0,1)\\

% 投影点 A、B
A(x_1+\frac L 2 \cos \alpha \cos(\pi - \gamma_s) + \frac K 2 \sin(\pi - \gamma_s),
y_1+\frac L 2 \cos \alpha\sin(\pi - \gamma_s) - \frac K 2 \cos(\pi - \gamma_s),
h_1+\frac L 2 \sin \alpha )\\

B(x_1+\frac L 2 \cos \alpha\cos(\pi - \gamma_s) - \frac K 2 \sin(\gamma_s - \pi),
y_1+\frac L 2 \cos \alpha \sin(\pi - \gamma_s) + \frac K 2 \cos(\pi - \gamma_s),
h_1+\frac L 2 \sin \alpha )\\

% 投影点所在光线
\text{第一条光线：}\frac{x-x_A}{\frac{-x_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = \frac{y-y_A}{\frac{-y_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = \frac{z-76}{\frac{z_A}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = t_1 (1)\\

\text{第二条光线：}\frac{x-x_B}{\frac{-x_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = \frac{y-y_B}{\frac{-y_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = \frac{z-76}{\frac{z_A}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}}} = t_2 (2)\\

\text{化为参数方程，则有：}\\

% 第一条直线的参数方程
\left\{\begin{array}{l}
x = x_A - \frac{x_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_1 \\
y = y_A - \frac{y_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_1 \\
z = z_A+\frac{80-h_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_1 \\
\end{array}\right.(1')\\

% 第二条直线的参数方程
\left\{\begin{array}{l}
x = x_B - \frac{x_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_2 \\
y = y_B - \frac{y_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_2 \\
z = z_A+\frac{80-h_1}{\sqrt{x_1^2+y_1^2+(80-h_1)^2}} t_2 \\
\end{array}\right.(2')\\

\text{被遮挡的定日镜的平面方程：} n_x (x-x_2)+n_y(y-y_2)+n_z(z-h_2)=0 (3)\\

(1')(3)\text{联立，解得第一个投影点：E} \\
\left\{\begin{array}{l}
t_1 ,t_2\\
x = x_E\\
y = y_E\\
z = z_E\\
\end{array}\right.\\

(2')(3)\text{联立，解得第二个投影点：F} \\
\left\{\begin{array}{l}
t_1 ,t_2\\
x = x_F\\
y = y_F\\
z = z_F\\
\end{array}\right.\\

\text{其中有：}z_E = z_F\ (4)\\

\text{计算可得两个投影点所在直线为：}y = x\tan(\frac \pi 2 - \gamma_s) (5)\\

\text{联立(3)(4)(5)，解得被投影的定日镜的边界点：E',F'}\\
\left\{\begin{array}{l}
x = x_E'\\
y = y_E'\\
z = z_E'\\
\end{array}\right.\\

\left\{\begin{array}{l}
x = x_F'\\
y = y_F'\\
z = z_F'\\
\end{array}\right.\\



\text{判断:}x_E,x_F\text{是否在}x_E',x_F'\text{范围内,若在内部,根据记录的边角点,设计算法,求解遮挡面积,} \\
\text{最终计算可得参数：}\eta_{sb} = 1-\frac {\text{遮挡面积}}{K \times L}\\
$$

## 三、知识积累

### 3.1 python类的访问

- **类变量：**在类的方法中可以通过`类名.`的方式获取，也可以通过`self.`的方式获取。更推荐前者
- **实例变量：**在类的方法中只能通过`self.`的方式获取

### 3.2 python计算速度

- 大约是一秒 $10^4$ 级别
- 那么蓝桥杯py组给十秒就约等于十万级别的数据量，也就和C++组相同了

### 3.3 数据类型转换

- tensor张量转化为numpy：`.numpy`即可
- list列表转化为numpy：`np.array(my_list)`

### 3.4 numpy的一些用法

- 所有的反三角函数都是完整的语法，比如`alpha = np.arcsin(sin_alpha_value)`而非`np.asin`
- 计算`Euclidean`范数不是`np.norm(np.array([1, 2, 3]))`，而是`np.linalg.norm(np.array([1, 2, 3]))`
