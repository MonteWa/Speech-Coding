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
