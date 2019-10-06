# Line Spectral Pairs

参考：http://www.lintech.org/webpapers/lsp_paper.pdf

## 什么是LSP(Line Spectral Pairs)？

LSP可以用来可以用来代表Line Prediction的参数，由于Line Prediction参数的稳定性差，在信号传输过程中，少量的噪声就可能导致恢复出来的信号包络与原始信号有较大的误差。同时它也与频谱特性相关，可以用来分析语音信号.

LSP中单个的Line叫做LSF(Line Spectral Frequency),两个LSF表示两个谐振条件(resonance conditions)，分别是声带为全开和全关的状态，这也就是LSP的物理意义。

LSP就是一对单位圆上的全极点滤波器，这两个滤波器的幅频响应线性叠加可以无限逼近于他们想要代表的LP滤波器。

<div align="center">
<img src="Graph/LSP_z1.jpg" width=300>
</div>

<div align="center">
<img src="Graph/LSP_z2.jpg" width=500>
</div>



## LSP如何获得？

LSP可以直接从LP参数中获得。p阶分析的表达如下：

<a href="https://www.codecogs.com/eqnedit.php?latex=A_{p}(z)=1&plus;a_{1}&space;z^{-1}&plus;a_{2}&space;z^{-2}&plus;\ldots&plus;a_{p}&space;z^{-p}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A_{p}(z)=1&plus;a_{1}&space;z^{-1}&plus;a_{2}&space;z^{-2}&plus;\ldots&plus;a_{p}&space;z^{-p}" title="A_{p}(z)=1+a_{1} z^{-1}+a_{2} z^{-2}+\ldots+a_{p} z^{-p}" /></a>

P，Q两组多项式表示声道状态为全关和全开的情况，所有实际情况下的A可以表示为这两组多项式的线性组合：

<a href="https://www.codecogs.com/eqnedit.php?latex=A_{p}(z)=\frac{P(z)&plus;Q(z)}{2}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?A_{p}(z)=\frac{P(z)&plus;Q(z)}{2}" title="A_{p}(z)=\frac{P(z)+Q(z)}{2}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\begin{aligned}&space;P(z)&space;&=A_{p}(z)-z^{-(p&plus;1)}&space;A_{p}\left(z^{-1}\right)&space;\\&space;Q(z)&space;&=A_{p}(z)&plus;z^{-(p&plus;1)}&space;A_{p}\left(z^{-1}\right)&space;\end{aligned}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\begin{aligned}&space;P(z)&space;&=A_{p}(z)-z^{-(p&plus;1)}&space;A_{p}\left(z^{-1}\right)&space;\\&space;Q(z)&space;&=A_{p}(z)&plus;z^{-(p&plus;1)}&space;A_{p}\left(z^{-1}\right)&space;\end{aligned}" title="\begin{aligned} P(z) &=A_{p}(z)-z^{-(p+1)} A_{p}\left(z^{-1}\right) \\ Q(z) &=A_{p}(z)+z^{-(p+1)} A_{p}\left(z^{-1}\right) \end{aligned}" /></a>

P，Q多项式的根就是LSF。而他们的复数根都落在Z域的单位圆上。可以用估计的方法近似获得。

## 如何从LSP获得LPC参数？

todo

## LSP的传统用法有什么？

### 1.LSP用于语音编码
LSP可以被用在语音编码当中，在一个通用的CELP编码器的编码端会解析出每帧语音的4个参数，分别是音高信息，码本索引，增益和LSP。这4个参数都有分别对应的物理含义。

* 由码本索引可以获得肺部产生的激励信号
* 音高信息表现声带的震动状态
* 增益代表声音的幅度大小
* LSP则包含了声道信息，其中包括嘴型，舌头位置以及鼻腔的作用。

### 2.LSP量化方法

LSP代替LP参数的优点是具有更加的稳定性，可以使用较少的bit来量化，从下表中可以看出当使用的比特数少时，量化LSP能获得较高的信噪比；缺点是会引入更多的计算复杂度。

对不同位置的LSF进行分组量化是一种更好的选择，例如对于一个10阶的系统，可以采用（2，3，3，2）或者（3，3，4）的分组方式，不同的Line可以用不同长度的bit来量化，这样做的原因是:
1. 不同位置的Line有着不同的重要性，重要性较低的LSF不需要高精度的量化，如低阶的LSF一般代表前几个共振峰，重要性更高，
2. 也有不同的统计分布，例如1，10这样位置上的Line拥有更加集中的分布，也就是说相比5，6Line，用较少的bit就可以更精准的表示。

<div align="center">
<img src="Graph/lsp_table1.jpg" width=500>
</div>

<div align="center">
<img src="Graph/lsp_table2.jpg" width=500>
</div>

所以采用非均一划分的方式去量化LSFs更加高效；甚至可以采用动态选择量化方法的方式进一步提高效率，例如指定两套量化方案，对不同帧采用更合适的量化方案，但这样需要一个额外的比特位来表示采用了哪种方案；引入Vector Quantization可以进一步压缩使用的bit数。

## LSP的高阶用法

1. 窄间隔的LSP可以指示出谱峰的位置
2. 由于LSP表示声道特征，所以可以结合其他参数来检测语音
3. 可以通过控制LSP的方法控制共振峰

### LSP的瞬时分析

1. shift

<a href="https://www.codecogs.com/eqnedit.php?latex=S&space;h&space;i&space;f&space;t[n]=\left\{\sum_{i=1}^{p}&space;\omega_{i}[n]\right\}-\left\{\sum_{i=1}^{p}&space;\omega_{i}[n&plus;1]\right\}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?S&space;h&space;i&space;f&space;t[n]=\left\{\sum_{i=1}^{p}&space;\omega_{i}[n]\right\}-\left\{\sum_{i=1}^{p}&space;\omega_{i}[n&plus;1]\right\}" title="S h i f t[n]=\left\{\sum_{i=1}^{p} \omega_{i}[n]\right\}-\left\{\sum_{i=1}^{p} \omega_{i}[n+1]\right\}" /></a>

应用：shift峰值可以指示出语音时域波形的转换，例如从静音段转换至语音段时，必将伴随一个较大的LSPs转换，这时会出现一个较大的差值，也就是一个shift峰值。如下图所示。

---

2. bias

<a href="https://www.codecogs.com/eqnedit.php?latex=\operatorname{Bias}[n]=\frac{1}{p}&space;\sum_{i=1}^{p}&space;\omega_{i}[n]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\operatorname{Bias}[n]=\frac{1}{p}&space;\sum_{i=1}^{p}&space;\omega_{i}[n]" title="\operatorname{Bias}[n]=\frac{1}{p} \sum_{i=1}^{p} \omega_{i}[n]" /></a>

应用：bias的高值能指示出摩擦音、清音，因为bias本身能体现出当前帧的频率高低的一种趋势，如果bias值较高说明主导当前帧的高频成分较多。如下图所示。

---

3. Dev（有疑问，清音部分不是更应该与，较小的deviation值应该指代清音？清音应该没有明显峰值，接近预设的等间隔LSP？）

<a href="https://www.codecogs.com/eqnedit.php?latex=\operatorname{Dev}[n]=\sum_{i=1}^{p}\left(\omega_{i}[n]-\bar{\omega}_{i}\right)^{\beta}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\operatorname{Dev}[n]=\sum_{i=1}^{p}\left(\omega_{i}[n]-\bar{\omega}_{i}\right)^{\beta}" title="\operatorname{Dev}[n]=\sum_{i=1}^{p}\left(\omega_{i}[n]-\bar{\omega}_{i}\right)^{\beta}" /></a>

<a href="https://www.codecogs.com/eqnedit.php?latex=\bar{\omega}_{i}=i&space;\pi&space;/(p&plus;1)&space;\quad&space;\text&space;{&space;for&space;}&space;\quad&space;i=1&space;\ldots&space;p" target="_blank"><img src="https://latex.codecogs.com/gif.latex?\bar{\omega}_{i}=i&space;\pi&space;/(p&plus;1)&space;\quad&space;\text&space;{&space;for&space;}&space;\quad&space;i=1&space;\ldots&space;p" title="\bar{\omega}_{i}=i \pi /(p+1) \quad \text { for } \quad i=1 \ldots p" /></a>

(wi是一组等间距的预设值。)

应用：Dev的高值同样可以指示出清音和摩擦应，Dev表示的是当前帧的LSP与标准值的分布有多接近，值越小则越接近；同时Dev有着较号的抗噪特性，因为噪声部分的Dev计算结果应该是接近于0的。如下图所示。

<div align="center">
<img src="Graph/inst_analysis.jpg" width=400>
</div>

## LSP调整

通过LSP参数恢复出的频谱有办法通过调整来减弱量化噪声带来的影响，虽然LSP相比LP参数已经有着很好的稳定性，但是肯定还是会有量化噪声的引入，不过我们可以通过调整LSP的值来减弱这种失真。 一种已经被证明有效的方法是基于感知的LSF平移调整，参考:
https://www.researchgate.net/publication/3717438_LSP-based_speech_modification_for_intelligibility_enhancement

## 非语音应用

todo

## 代码实现
### matlab

```
% from lsp to lpc
function a = lsp_lpc(theta)
p = length(theta);
q = cos(theta(1:p));
f1(10) = 1; f1(9) = 0;
for i = 1:p/2
    f1(10+i) = -2*q(2*i-1)*f1(10+i-1)+2*f1(10+i-2);
    for k = i-1:-1:1
        f1(10+k)=f1(10+k)-2*q(2*i-1)*f1(10+k-1)+f1(10+k-2);
    end
end
f2(10) = 1; f2(9) = 0;
for i = 1:p/2
    f2(10+i) = -2*q(2*i)*f2(10+i-1)+2*f2(10+i-2);
    for k = i-1:-1:1
        f2(10+k)=f2(10+k)-2*q(2*i)*f2(10+k-1)+f2(10+k-2);
    end
end
f1b(1)=f1(11)+1;
f2b(1)=f2(11)-1;
for i = 2:p/2
    f1b(i) = f1(10+i) + f1(10+i-1);
    f2b(i) = f2(10+i) - f2(10+i-1);
end
for i =1:p/2
    a2(i)=0.5*(f1b(i)+f2b(i));
    a2(i+p/2)=0.5*(f1b(p/2)-i+1 - f2b(p/2-i+1));
end
a = [1,a2];
```
```
% from lpc to lsp
function lsp = lpc_lsp(a)
p = length(a);
% derive the coefficients for P'(Z) and P'(Q)
A(1) = 1; B(1) = 1;
for k = 2:p
    A(k) = (a(k)-a(p+2-k) + A(k-1));
    B(k) = (a(k)+a(p+2-k) + B(k-1));
end
r1 = roots(A);
r2 = roots(B);
for k = 1:p-1
    if (real(r1(k))<0)
        theta1(k) = pi-abs(atan(imag(r1(k))/real(r1(k))));
    else
        theta1(k) = abs(atan(imag(r1(k))/real(r1(k))));
    end
    if(real(r2(k)))<0
        theta2(k)=pi-abs(atan(imag(r2(k))/real(r2(k))));
    else
        theta2(k) = abs(atan(imag(r2(k))/real(r2(k))));
    end
end
%Test vectors
p = p-1;
for k = 1:p/2
    theta(k) = theta1(k*2);
    theta(k+2/2) = theta2(k*2);
end
lsp = sort(theta);
```
### python

todo
