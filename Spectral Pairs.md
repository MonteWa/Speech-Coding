## Line Spectral Pairs

### 什么是LSP(Line Spectral Pairs)？

LSP可以用来可以用来代表Line Prediction的参数，由于Line Prediction参数的稳定性差，在信号传输过程中，少量的噪声就可能导致恢复出来的信号包络与原始信号有较大的误差。同时它也与频谱特性相关，可以用来分析语音信号.

LSP中单个的Line叫做LSF(Line Spectral Frequency),两个LSF表示两个谐振条件(resonance conditions)，分别是声带为全开和全关的状态，这也就是LSP的物理意义。

(修改)-> 形象的讲，LSP就是一对单位圆上的全极点滤波器，这两个滤波器的幅频响应相叠加无限逼近于他们想要代表的LP滤波器。

todo: 补图

### LSP如何获得？

LSP可以直接从LP参数中获得。p阶分析的表达如下：

todo：LP公式 A= 。。。

P，Q两组多项式表示声道状态为全关和全开的情况，所有实际情况下的A可以表示为这两组多项式的线性组合：

todo： 公式 A = 1/2（p+q）

todo: 公式 P = A - term

todo: 公式 Q = A + term

P，Q多项式的根就是LSF。而他们的复数根都落在Z域的单位圆上。可以用估计的方法近似获得。

### 如何从LSP获得LPC参数？

略

### LSP的传统用法有什么？
