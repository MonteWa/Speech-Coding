## Spectral Envelope
### Linear Predictive Coding
LPC可以被看做是对声道的建模，但同时也可以看成是对spectral envelope的建模，频谱包络被LPC的系数编码。LPC的中心思想是使用过去的采样点，加权并求和并加上一个残差项来预测当前的采样点，公式表示如下：

<a href="https://www.codecogs.com/eqnedit.php?latex=\xi_{n}=-\sum_{k=1}^{M}&space;\alpha_{k}&space;\xi_{n-k}&plus;\varepsilon_{n}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\xi_{n}=-\sum_{k=1}^{M}&space;\alpha_{k}&space;\xi_{n-k}&plus;\varepsilon_{n}" title="\xi_{n}=-\sum_{k=1}^{M} \alpha_{k} \xi_{n-k}+\varepsilon_{n}" /></a>

ak为LPC系数，sigman为残差项，我们可以将a0定义为1，将残差项单独留在等号一边，公式就可以改写为：

<a href="https://www.codecogs.com/eqnedit.php?latex=\varepsilon_{n}=\sum_{k=0}^{M}&space;\alpha_{k}&space;\xi_{n-k}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\varepsilon_{n}=\sum_{k=0}^{M}&space;\alpha_{k}&space;\xi_{n-k}" title="\varepsilon_{n}=\sum_{k=0}^{M} \alpha_{k} \xi_{n-k}" /></a>

写可以写作向量相乘的形式：

<a href="https://www.codecogs.com/eqnedit.php?latex=\varepsilon_{n}=x_{n}^{T}&space;\alpha" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\varepsilon_{n}=x_{n}^{T}&space;\alpha" title="\varepsilon_{n}=x_{n}^{T} \alpha" /></a>

为了能够得到最佳的线性预测模型，我们希望残差越小越好，一般的方法是最小化残差的能量期望：

<a href="https://www.codecogs.com/eqnedit.php?latex=\min&space;_{a}&space;\mathscr{E}\left[\|e\|^{2}\right]=\min&space;_{a}&space;\mathscr{E}\left[a^{T}&space;x&space;x^{T}&space;a\right]=\min&space;_{a}&space;a^{T}&space;\mathscr{E}\left[x^{T}&space;x\right]&space;a=\min&space;_{a}&space;a^{T}&space;R&space;a" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\min&space;_{a}&space;\mathscr{E}\left[\|e\|^{2}\right]=\min&space;_{a}&space;\mathscr{E}\left[a^{T}&space;x&space;x^{T}&space;a\right]=\min&space;_{a}&space;a^{T}&space;\mathscr{E}\left[x^{T}&space;x\right]&space;a=\min&space;_{a}&space;a^{T}&space;R&space;a" title="\min _{a} \mathscr{E}\left[\|e\|^{2}\right]=\min _{a} \mathscr{E}\left[a^{T} x x^{T} a\right]=\min _{a} a^{T} \mathscr{E}\left[x^{T} x\right] a=\min _{a} a^{T} R a" /></a>

这里<a href="https://www.codecogs.com/eqnedit.php?latex=R=\mathscr{E}\left[x^{T}&space;x\right]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?R=\mathscr{E}\left[x^{T}&space;x\right]" title="R=\mathscr{E}\left[x^{T} x\right]" /></a>，是一个对称对角矩阵：

<a href="https://www.codecogs.com/eqnedit.php?latex=R=\left[&space;\begin{array}{cccc}{\rho_{0}}&space;&&space;{\rho_{1}}&space;&&space;{\dots}&space;&&space;{\rho_{M}}&space;\\&space;{\rho_{1}}&space;&&space;{\rho_{0}}&space;&&space;{\ddots}&space;&&space;{\vdots}&space;\\&space;{\vdots}&space;&&space;{\ddots}&space;&&space;{\ddots}&space;&&space;{\rho_{1}}&space;\\&space;{\rho_{M}}&space;&&space;{\dots}&space;&&space;{\rho_{1}}&space;&&space;{\rho_{0}}\end{array}\right]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?R=\left[&space;\begin{array}{cccc}{\rho_{0}}&space;&&space;{\rho_{1}}&space;&&space;{\dots}&space;&&space;{\rho_{M}}&space;\\&space;{\rho_{1}}&space;&&space;{\rho_{0}}&space;&&space;{\ddots}&space;&&space;{\vdots}&space;\\&space;{\vdots}&space;&&space;{\ddots}&space;&&space;{\ddots}&space;&&space;{\rho_{1}}&space;\\&space;{\rho_{M}}&space;&&space;{\dots}&space;&&space;{\rho_{1}}&space;&&space;{\rho_{0}}\end{array}\right]" title="R=\left[ \begin{array}{cccc}{\rho_{0}} & {\rho_{1}} & {\dots} & {\rho_{M}} \\ {\rho_{1}} & {\rho_{0}} & {\ddots} & {\vdots} \\ {\vdots} & {\ddots} & {\ddots} & {\rho_{1}} \\ {\rho_{M}} & {\dots} & {\rho_{1}} & {\rho_{0}}\end{array}\right]" /></a>

且：

<a href="https://www.codecogs.com/eqnedit.php?latex=\rho_{k}=\mathscr{E}\left[\xi_{n}&space;\xi_{n-k}\right]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\rho_{k}=\mathscr{E}\left[\xi_{n}&space;\xi_{n-k}\right]" title="\rho_{k}=\mathscr{E}\left[\xi_{n} \xi_{n-k}\right]" /></a>

（参数的求解 hold on）

在编码端，求解好的LPC，其实是一个全零点滤波器，变换的Z域的公式如下：

<a href="https://www.codecogs.com/eqnedit.php?latex=e[n]=s[n]-\sum_{k=1}^{M}&space;\alpha_{k}&space;s(n-k)&space;\rightarrow&space;A(z)=1-\sum_{k=1}^{M}&space;\alpha_{k}&space;z^{-k}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?e[n]=s[n]-\sum_{k=1}^{M}&space;\alpha_{k}&space;s(n-k)&space;\rightarrow&space;A(z)=1-\sum_{k=1}^{M}&space;\alpha_{k}&space;z^{-k}" title="e[n]=s[n]-\sum_{k=1}^{M} \alpha_{k} s(n-k) \rightarrow A(z)=1-\sum_{k=1}^{M} \alpha_{k} z^{-k}" /></a>

对于一帧数据的处理效果如下，A(z)是LPC滤波器的幅频响应，可以看到原信号SS在被滤波器处理后得到了残差信号ee，而ee更接近一个白噪声信号，而A(z)就包含了原信号的包络信息，所以对LPC参数进行傅里叶变换就可以得到信号频谱的包络信息：

<div align="center">
<img src="Graph/lpc_filter.jpg" width=700>
</div>

在解码端的LPC滤波器就变成了一个全极点滤波器，公式如下：

<a href="https://www.codecogs.com/eqnedit.php?latex=s[n]=\sum_{k=1}^{M}&space;a_{k}&space;s(n-k)&plus;e[n]&space;\rightarrow&space;H(z)=\frac{1}{1-\sum_{k=1}^{M}&space;a_{k}&space;z^{-k}}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s[n]=\sum_{k=1}^{M}&space;a_{k}&space;s(n-k)&plus;e[n]&space;\rightarrow&space;H(z)=\frac{1}{1-\sum_{k=1}^{M}&space;a_{k}&space;z^{-k}}" title="s[n]=\sum_{k=1}^{M} a_{k} s(n-k)+e[n] \rightarrow H(z)=\frac{1}{1-\sum_{k=1}^{M} a_{k} z^{-k}}" /></a>

当解码端的LPC变回一个全极点滤波器之后，就可以对白噪声做一个整形，如下图将ee残差信号变回ss原始信号。

<div align="center">
<img src="Graph/inverse_lpc.jpg" width=700>
</div>

在被LPC处理之前，一帧语音要经过加窗处理，常用的窗如下，后两种窗的延时比较小：

<div align="center">
<img src="Graph/window.jpg" width=700>
</div>

### LPC参数的量化

当获得LPC的参数之后，我们需要将其量化再传输，但是LPC的参数对误差非常敏感，很少的误差就会造成结果的偏差很大，如果为了提高量化精度，就需要很多的bit来传输LPC参数。而最佳传输参数的方式是在Line Spectral Frequency域来量化参数，公式如下(K一般为1)：

<a href="https://www.codecogs.com/eqnedit.php?latex=\left\{\begin{array}{l}{P(z)=A(z)&plus;z^{-M-K}&space;A\left(z^{-1}\right)}&space;\\&space;{Q(z)=A(z)-z^{-M-K}&space;A\left(z^{-1}\right)}\end{array}\right." target="_blank"><img src="https://latex.codecogs.com/gif.latex?\left\{\begin{array}{l}{P(z)=A(z)&plus;z^{-M-K}&space;A\left(z^{-1}\right)}&space;\\&space;{Q(z)=A(z)-z^{-M-K}&space;A\left(z^{-1}\right)}\end{array}\right." title="\left\{\begin{array}{l}{P(z)=A(z)+z^{-M-K} A\left(z^{-1}\right)} \\ {Q(z)=A(z)-z^{-M-K} A\left(z^{-1}\right)}\end{array}\right." /></a>

P(z)和Q(z)的极点交错的出现在单位圆上，小的误差不会对结果造成很大的影响。A(z)=(P(z)+Q(z))/2
<div align="center">
<img src="Graph/lsf_z.jpg" width=400>
</div>

<div align="center">
<img src="Graph/lsf_z2.jpg" width=500>
</div>
