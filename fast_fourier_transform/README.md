本文讲述了傅里叶变化的作用、基本原理以及离散傅里叶变换的使用。多图预警，如果文字不太好理解，那么看图也可以收获一些东西。

# 傅里叶变换的作用
傅里叶变换是整个通信行业的基石，并且广泛应用在图像处理、音视频处理、统计学、密码学等等行业。

傅里叶变换的作用是什么？

打个比方，[我们历史文章有说过声音](https://mp.weixin.qq.com/s?__biz=MzkwNTEyOTI0MA==&mid=2247483788&idx=1&sn=8aa108909a8a2e48191d2347399c9a15&chksm=c0fd3dfbf78ab4edeedb04465ef2974f51e296e8ddb891d8ceaafb652f65165d64c968f946e0&mpshare=1&scene=1&srcid=0317kmbEPRAWFSDmBw9f64mu&sharer_sharetime=1615991899306&sharer_shareid=741c39217c916aaf06bf9827e80dbff6&exportkey=AZda3kJCerGJGKbcL7VmxJE%3D&pass_ticket=%2Bv5BpD9m2hKjWcN2FExXz0zOnTYFXgfQooYE%2BF%2FYQOsBW17SEsNdSIFKj3pJgW8I&wx_header=0#rd)。不同人发声的频率是不一样的，男声的频率比较低，女声的频率比较高，同理尖叫频率会非常高。

此图上半部的横坐标是时间，纵坐标是幅度。此图下半部做了傅里叶变换之后的结果。

![图片源自https://www.bilibili.com/video/BV1pW411J7s8](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/noise_toohigh.png)

此图下半部分横坐标是频率，纵坐标是幅度，此时只需要把尖叫声相应的频率去掉，再做一下逆傅里叶变换，重新生成一个新的音频，这时的音频就没有尖叫声了。

![图片源自https://www.bilibili.com/video/BV1pW411J7s8](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/niose_high-down.png)

还有一个比方，就是三棱镜。初中的时候学过，白光是由多种颜色的光组成，不同颜色的光的波长不一样，频率也不一样。

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/three_mirror1.jpg)

声音是波，光也是波，不同的波长的波，频率不一样。傅里叶变换就如三棱镜，把不同频率的波给分解出来。

而想要了解傅里叶变换的思想，不得不不从公式说起……

这个公式，似乎有点眼熟，甚至是不少人的痛苦回忆……

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/fourier-translform-formula.png)

# 傅里叶级数
让我们先忘了上面那个公(tong)式(ku)，不妨先假设，傅里叶一开始是这么想的(傅里叶级数的公式)：

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/fourier-serises-formula.png)

任何周期性的波形，最终都可以由一个A0的直流分量，再加上不同频率的正弦波叠加而成。

？有点反直觉，下面这图，演示了如何让不同的正弦波，叠加成一个方波的：

![图像来自wiki百科](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin2square-from_wiki.gif)

又或者：
![图片来源: http://1ucasvb.tumblr.com](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin2square-from-tumblr.gif)
可见只要正弦波够多，方波就可以足够“方”。

在数学领域，正弦波可以有无限多个，最终肯定能组成一个真正的方波。

在工程领域，由于采样点数有限，不需要制造一个完美的方波，近似就可以了。所以是可行的。

那么问题来了，三角函数里面，我们学过sin或者cos。那么sin、正弦波到底是什么？

![正弦信号的两种图形化表示 (图片来源: http://1ucasvb.tumblr.com)](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin-and-circle-from-tumblr.gif)

有一个长度为A的极细的木棍，一直在基于坐标原点旋转，随着时间旋转，这个点在纵坐标的投影，就是sin正弦波；这个点在横坐标的投影，就是cos余弦波。

回到这个公式，
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/fourier-serises-formula.png)

有点明白了。公式中的An是木棍的长度（幅度Amplitude），nω是这个木棍的旋转速度（频率Frequency），φn是木棍刚开始旋转时的位置（相位Phase Angle）。如下图：

![图片源自https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/descirbe-spin-circle-sin-from-betterexplain.png)


# 如何检测波是否存在

如果我们想要知道，某个频率的正弦波，是否存在于这段信号中，并且该频率的正弦波的幅度和相位是多少。

这就利用到正弦波的正交性，正交性的一个特点是：

**不同频率的正弦波相乘，在一定周期内积分后，结果为0。**

**相同频率的正弦波相乘，在一定周期内积分后，结果不为0。**

那么在这段信号中，想要获得某个频率的正弦波的幅度和相位，检波手段即为，**该频率的正弦波和目标信号，相乘后积分即可**。

[具体推导过程可以参考DBinary在zhihu上的回答](https://www.zhihu.com/question/22085329/answer/774074211) : https://www.zhihu.com/question/22085329/answer/774074211。

此刻我们需要记住的是：**相乘**后**积分**

# 傅里叶变换
我们知道傅里叶变换是一个神奇的频率分离器……

刚才也说到检波的手段是：相乘再积分……

这个时候再看看，傅里叶正变换的公式……

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/multiply-integral.png)

我们看到了，相乘——目标函数的相乘，也有看到了积分。

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin-e-format.png)

但是这个e没看懂，难道这就代表正弦波？

没错，准确来讲是一个复平面的正弦波。

介绍一下复平面，如下：
![图源自Wiki](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/complex_conjugate.png)

横轴是实轴，纵轴是虚轴。z=x+iy, z就代表：横轴位置是x，纵轴位置是y，这样一个向量。

还记得刚才旋转的木棍吗？z是一根木棍，此时此刻，我们获得了一根静止的木棍。

但我们得让它旋转起来，这样才能得到正弦波啊。

在复数中，1乘以i，就等于i, i 再乘以 i，就是-1。
![图片源自betterexplained](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/i-rotate.jpeg)

这是因为乘以一个i，就相当于逆时针旋转90度。

另外欧拉公式定义如下：
![图源自Wiki](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/Euler_formula-from-wiki.png)

![图源自betterexplained](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/euler-formula.png)

$e^{i\pi}$ 代表了旋转了180度的位置，横坐标为-1，纵坐标为0

$e^{2\pi i}$ 代表了旋转了360度的位置，横坐标为1，纵坐标为0

$e^{2\pi it}$ 其中t代表时间，如果t的值是0.5秒的话，横坐标为-1，纵坐标为0。如果t的值是1秒的话，横坐标为1，纵坐标为0。这代表啥，代表这个木棍它随着时间转动了，而且转动频率是，一秒转动一圈。

如果加上f这个代表频率的变量，如$e^{2i\pi ft}$ 如果此刻f是2的话，那么转动频率是，一秒转动两圈。

当然你想要让它顺时针旋转的话，加个负号就可以了：$e^{-2i\pi ft}$ 

有时候会看到，用j替代i的情况，$e^{-2j\pi ft}$  都是一个意思，用j表示，只是因为在物理或者电子领域，i通常被表示成电流了。

也有时候 f 即频率也会被其它字母表示（ξ），t也可能被其它字母表示，如：
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin-e-format.png)

没关系，都是一个意思。

此刻我们创建的是，正弦波的复平面信号，那么它其实是立体的，三维的。

复指数信号随时间的变化轨迹：

![复指数信号随时间的变化轨迹，图自《深入浅出通信原理》](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/complex-sin.jpg)

复指数信号在复平面上的投影：

![复指数信号在复平面上的投影，图自《深入浅出通信原理》](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/complex-sin-shadow.jpg)

复指数信号在实轴上的投影随时间变化的曲线:

![图自《深入浅出通信原理》](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/real_shadow.jpg)

复指数信号在虚轴上的投影随时间变化的曲线:

![图自《深入浅出通信原理》](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/image_shadow.jpg)

如果不太好想象的话，网上有大佬做的动画，更加直观一点：

![图源自https://www.bilibili.com/video/av19086191/](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/sin-signal-complex-moving.gif)

那么问题来了，复平面信号如何和目标函数相乘。两个复数的乘法在极坐标下的表示最简单，笛卡尔坐标转换到极坐标的过程:
![图源自https://1ucasvb.tumblr.com](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/Cartesian_to_polar.gif)

至于相乘的过程，B站的3blue1brown的动画，也的的确确完美地表现了出来:
![图源自3blue1brown](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/complex-signal-multuply.gif)

额，有点超纲了。[在上一篇快速学习方法中](https://mp.weixin.qq.com/s?__biz=MzkwNTEyOTI0MA==&mid=2247483924&idx=1&sn=4d41c22301fbe9ccc328eaf18fa55a0f&chksm=c0fd3e63f78ab775ca88d1ae1f04066f3ade9dfc4a95963fb871606ec287776bfd964df9110c&mpshare=1&scene=1&srcid=0317CwT8WADVziXndxIelldW&sharer_sharetime=1615991886240&sharer_shareid=741c39217c916aaf06bf9827e80dbff6&exportkey=AXa2%2B2q7hQL9sINROMmbfU8%3D&pass_ticket=%2Bv5BpD9m2hKjWcN2FExXz0zOnTYFXgfQooYE%2BF%2FYQOsBW17SEsNdSIFKj3pJgW8I&wx_header=0#rd)，在学习的过程中，我们已经掌握了最小启动知识，此刻我们需要练习，那么我们可以使用离散傅里叶变换进行练习，加深理解。

# 离散傅里叶变换
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/discrete-fourier-translform.png)

如上，相比连续傅里叶变换中的公式，以上的离散傅里叶变换中，积分变成了上面的累加，连续的频率变化，也变成了上面的各个k个频率的采样。

[better explained中有个吊炸天的网页插件，可以很方便的观察时域和频域的变化。](https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/) : https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/

因为众所周知的墙的原因，我把这个插件下载下来，放在我的github页面上了：[https://github.com/Binfun/fourier_transform](https://github.com/Binfun/fourier_transform)

下载打开html就可操作，如下：

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/interact-fourier-demo1.gif)

左上角的Cycles框所显示的1 2，即为频率数据。
右上角的Time框所显示的3 -1，即为时域数据。

Cycles框中的第一个数字（图中的1）即为0Hz的直流分量的幅值，

第二个数字（图中的2）即为1Hz的正弦波的幅值，

如果有第三个数字的话，那么就代表2Hz的正弦波的幅值，以此类推3Hz 4Hz...等等。

Time框中的3 -1分别代表第1个和第2个采样点的值，即黄色的点。

灰色的直线代表0Hz的直流分量，绿色的曲线代表1Hz的正弦波，蓝色的曲线代表他们叠加的结果。

我们可以用这个工具做很多事情，比如我们可以用它来观察，为什么时域的数据移位，等同于频域的相位变化：
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/interact-fourier-demo2.gif)

没错Cycle框数字中，冒号后面的那个数字就代表相位。

甚至于，我们可以使用这段动画去理解，观察频域补零，时域插值的现象：
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/interact-fourier-demo3.gif)

好像说的有点远了，强烈建议自己亲手尝试，会有不同的感觉。

回到离散傅里叶变换的公式：
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/discrete-fourier-translform.png)

如果有时域数据: [1, 2, 3] 的话，
那么代入公式算得频域数据的结果为: 

[6 + 0i, -1.5 + 0.86603i, -1.5 - 0.86603i]

因为时域/频域数据有3个，也就是相同频点上累加了3次，我们需要除以N就是3，进行复原：

[2 + 0i, -0.5 + 0.28868i, -0.5 - 0.28868i]

这三个复数，就代表了下面图中Cycles框中的：

2 0.58:150 0.58:-150 

![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/interact-demo4.png)

还记得上次说的那个木棍吗？-0.5 + 0.28868i就代表旋转速度为1Hz的木棍的起始点。

计算幅度（木棍长度）：![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/square_complex.gif)

计算相位（木棍起始位置）：
arctan(0.28868/-0.5) = 150度

至于代表0Hz的2 + 0i怎么理解，只有这一根木棍是不旋转的，静止的，是直流分量。

说道直流分量，我们就可以大胆地猜测，离散傅里叶变换，就是将时域信号当成周期信号处理，算得傅里叶级数的系数啊。

细心的同学可能发现，图中的正弦波当相位是0的时候，其实是cos函数，而不是sin函数。

这是因为cos反应的是实数域的情况。这个页面中，时域数据中的虚部都是0，验证也简单，把Cycle中的相位都加个90度试试，看看Time是不是都是0，这是因为时域的虚部的值都是0呀。
![](https://cdn.jsdelivr.net/gh/Binfun/articles/fourier_transform/demo5.png)

读到这里，可能你觉得有些细节不太够。但也许我们已经有了点全局模糊的认识，整体框架已对，再了解细节，事半功倍。

具体细节可以参阅参考资料，巨人的肩膀，非常的精彩。

# 巨人的肩膀
- 直观的数学: https://zhuanlan.zhihu.com/p/48305950
- DBinary的回答: https://www.zhihu.com/question/22085329/answer/774074211
- 傅里叶级数的推导: https://zhuanlan.zhihu.com/p/41455378
- 动图合集: https://1ucasvb.tumblr.com/page/4
- 直观的傅里叶变换: https://betterexplained.com/articles/an-interactive-guide-to-the-fourier-transform/
- 3Blue1Brown出品: https://www.bilibili.com/video/BV1pW411J7s8
- 正弦波三维动画: https://www.bilibili.com/video/av19086191/
- 陈爱军：《深入浅出通信原理》