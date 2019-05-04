## Speech Production

### 词汇的构成

* 音位(phoneme):音位使最小的具有独一无二**语言意义**的发声单位
* 语音(phone):语音是最小的具有独一无二**声学信号**的发声单位

多个不同的语音可能会被归为同一个音位，也就是是发声不同，但是意义相同这种情况，例如方言。所以音位是处理语言学系统的最小单位，而语音是处理语音感知系统的最小单位。

### 语音产生的生理基础
语音的产生从肺部收缩产生气流开始，这股气流有两个作用，第一，它会使声带产生震动，声带的周期性开合使得气流获得了一个周期性的波形；第二在声道收缩时，这股气流又可以产生噪声湍流。所以**振动的声带**，**湍流噪声**，以及**声道对声音塑形**这三个因素据定了语音的特征。

下表展示了不同种类的语音对应的产生机理:

<div align="center">
<img src="graph/moa.JPG" width=500>
</div>

总的来说，声道形状是语音信号的一个重要特征，因为不同的声道形状就能产生不同的共振，而共振峰就是用来描述这个特征的，一般前两个共振峰最重要，这个两个共振峰就足够确定一个元音。如下图所示：

<div align="center">
<img src="graph/vowelformants.JPG" width=500>
</div>

### 声道建模

根据声音产生的生理基础，我们可以对声道进行建模，这样只用少量参数就能够描述语音信号。一般使用管道(tube model)模型来对声道建模：

<div align="center">
<img src="graph/tubemodel.jpg" width=300>
</div>

而管道模型的一个好处是它可以等效成一个线性预测器，(on hold)

前面除了声道还提到了**振动的声带**，**湍流噪声** 这两种激励，所以将他们和声道模型整合起来形成系统模型，在生成响音(sonorant)时，激励由冲击序列主导，生成阻断音(obstruent)时，激励由白噪声主导，这两种激励是可以同事产生的，再通过声道塑形星成最终的波形，数学描述如下：

<img src="https://latex.codecogs.com/gif.latex?S(z)=\left[F_{0}(z)&space;G(z)&plus;X(z)&space;N(z)\right]&space;V(z)&space;L(z)" title="S(z)=\left[F_{0}(z) G(z)+X(z) N(z)\right] V(z) L(z)" />


F(z)表示基础频率脉冲序列，G(z)是单个声门脉冲的波形，X(z)是白噪声，N(z)表示一个滤波器，V(z)和L(z)分别是声道模型和辐射模型的传递函数。

<div align="center">
<img src="graph/systemmodel.jpg" width=300>
</div>

将声道模型和唇辐射模型整合在一起，都包含在这个线性预测器上，这两种信号叠加就模拟了气流在人体产生声音时的两种情况。数学描述如下：

<img src="https://latex.codecogs.com/gif.latex?S(z)=\left[F_{0}(z)&plus;X(z)\right]&space;H(z)=\left[F_{0}(z)&plus;X(z)\right]&space;A^{-1}(z)" title="S(z)=\left[F_{0}(z)+X(z)\right] H(z)=\left[F_{0}(z)+X(z)\right] A^{-1}(z)" /></a>

这个模型可以被看作是解码端的解码器，所以在编码端需要提取并传递的信息就是能确定这个系统中各个部件状态的参数。
