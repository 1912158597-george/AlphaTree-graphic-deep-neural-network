# GAN 生成式对抗网络


![GAN](https://github.com/weslynn/graphic-deep-neural-network/blob/master/map/Art&pic/ganpic.png)

-----------------------------------

“在机器学习过去的10年里，GAN是最有趣的一个想法。”
                                         ——Yann LeCun

----------------------------------
## GAN

2019年，是很重要的一年。在这一年里，GAN有了重大的进展，出现了 BigGan，StyleGan 这样生成高清大图的GAN，也出现了很多对GAN的可解释性方法，包括 苏剑林的OGAN。 还有GAN都被拿来烤Pizza了……(CVPR2019还有个PizzaGAN,[demo](http://pizzagan.csail.mit.edu/) [paper](https://arxiv.org/abs/1906.02839))

这一切预示着GAN这个话题，马上就要被勤勉的科学家们攻克了。

从目标分类的被攻克，人脸识别的特征提取和loss改进，目标检测与分割的统一…… 深度学习的堡垒一个接一个的被攻克。一切都迅速都走上可应用化的道路。


深度学习的发展惊人，如果说互联网过的是狗年，一年抵七年，深度学习的发展一定是在天宫的，天上一天，地上一年。


生成式对抗网络（GAN, Generative Adversarial Networks ）是近年来深度学习中复杂分布上无监督学习最具前景的方法之一。
监督学习需要大量标记样本，而GAN不用。
模型包括两个模块：生成模型（Generative Model）和判别模型（Discriminative Model），通过模型的互相博弈学习产生相当好的输出。原始 GAN 理论中，并不要求 G 和 D 都是神经网络，只需要是能拟合相应生成和判别的函数即可。但实用中一般均使用深度神经网络作为 G 和 D 。


![basic](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/basic.png)

GAN的目标,就是G生成的数据在D看来，和真实数据误差越小越好，目标函数如下：

![basictarget](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/basictarget.png)

从判别器 D 的角度看，它希望自己能尽可能区分真实样本和虚假样本，因此希望 D(x) 尽可能大，D(G(z)) 尽可能小， 即 V(D,G)尽可能大。从生成器 G 的角度看，它希望自己尽可能骗过 D，也就是希望 D(G(z)) 尽可能大，即 V(D,G) 尽可能小。两个模型相对抗，最后达到全局最优。

从数据分布来说，就是开始的噪声noise，在G不断修正后，产生的分布，和目标数据分布达到一致：

![data](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/data.png)


   [1] Ian Goodfellow. "Generative Adversarial Networks." arXiv preprint arXiv:1406.2661v1 (2014). [pdf] (https://arxiv.org/pdf/1406.2661v1.pdf)

   http://www.iangoodfellow.com/

https://github.com/goodfeli/adversarial

![human](https://github.com/weslynn/graphic-deep-neural-network/blob/master/famous/goodfellow.jpg)

-----------------------------------------------------------------------------
“酒后脑洞”的故事…

2014 年的一个晚上，Goodfellow 在酒吧给师兄庆祝博士毕业。一群工程师聚在一起不聊姑娘，而是开始了深入了学术探讨——如何让计算机自动生成照片。



当时研究人员已经在使用神经网络（松散地模仿人脑神经元网络的算法），作为“生成”模型来创建可信的新数据。但结果往往不是很好：计算机生成的人脸图像要么模糊到看不清人脸，要么会出现没有耳朵之类的错误。

针对这个问题，Goodfellow 的朋友们“煞费苦心”，提出了一个计划——对构成照片的元素进行统计分析，来帮助机器自己生成图像。

Goodfellow 一听就觉得这个想法根本行不通，马上给否决掉了。但他已经无心再party了，刚才的那个问题一直盘旋在他的脑海，他边喝酒边思考，突然灵光一现：如果让两个神经网络互相对抗呢？

但朋友们对这个不靠谱的脑洞深表怀疑。Goodfellow 转头回家，决定用事实说话。写代码写到凌晨，然后测试…



Ian Goodfellow：如果你有良好的相关编程基础，那么快速实现自己的想法将变得非常简单。几年来，我和我的同事一直在致力于软件库的开发，我曾用这些软件库来创建第一个 GAN、Theano 和 Pylearn2。第一个 GAN 几乎是复制-粘贴我们早先的一篇论文《Maxout Networks》中的 MNIST 分类器。即使是 Maxout 论文中的超参数对 GAN 也相当有效，所以我不需要做太多的新工作。而且，MNIST 模型训练非常快。我记得第一个 MNIST GAN 只花了我一个小时左右的时间。

参考：

https://baijiahao.baidu.com/s?id=1615737087826316102&wfr=spider&for=pc


http://www.heijing.co/almosthuman2014/2018101111561225242

-----------------------------------------------------------------------------


和监督学习的的网络结构一样，GAN的发展 也主要包含网络结构性的改进 和loss、参数、权重的改进。

Avinash Hindupur建了一个GAN Zoo，他的“动物园”里目前已经收集了近500种有名有姓的GAN。
主要是2014-2018年之间的GAN。
https://github.com/hindupuravinash/the-gan-zoo

那么问题来了：这么多变体，有什么区别？哪个好用？

于是，Google Brain的几位研究员（不包括原版GAN的爸爸Ian Goodfellow）对各种进行了loss，参数，权重修改的GAN做一次“中立、多方面、大规模的”评测。
在此项研究中，Google此项研究中使用了minimax损失函数和用non-saturating损失函数的GAN，分别简称为MM GAN和NS GAN，对比了WGAN、WGAN GP、LS GAN、DRAGAN、BEGAN，另外还对比的有VAE（变分自编码器）。为了很好的说明问题，研究者们两个指标来对比了实验结果，分别是FID和精度（precision、）、召回率（recall）以及两者的平均数F1。

其中FID（Fréchet distance(弗雷歇距离) ）是法国数学家Maurice René Fréchet在1906年提出的一种路径空间相似形描述，直观来说是狗绳距离：主人走路径A，狗走路径B，各自走完这两条路径过程中所需要的最短狗绳长度，所以说，FID与生成图像的质量呈负相关。

为了更容易说明对比的结果，研究者们自制了一个类似mnist的数据集，数据集中都是灰度图，图像中的目标是不同形状的三角形。

最后，他们得出了一个有点丧的结论：

No evidence that any of the tested algorithms consistently outperforms the original one.
：

都差不多……都跟原版差不多……


Are GANs Created Equal? A Large-Scale Study
Mario Lucic, Karol Kurach, Marcin Michalski, Sylvain Gelly, Olivier Bousquet
https://arxiv.org/abs/1711.10337


http://www.dataguru.cn/article-12637-1.html

这些改进是否一无是处呢？当然不是，之前的GAN 训练很难， 而他们的优点，主要就是让训练变得更简单了。 

那对于GAN这种无监督学习的算法，不同的模型结构改进，和不同的应用领域，才是GAN大放异彩的地方。


此外，谷歌大脑发布了一篇全面梳理 GAN 的论文，该研究从损失函数、对抗架构、正则化、归一化和度量方法等几大方向整理生成对抗网络的特性与变体。
作者们复现了当前最佳的模型并公平地对比与探索 GAN 的整个研究图景，此外研究者在 TensorFlow Hub 和 GitHub 也分别提供了预训练模型与对比结果。
https://arxiv.org/pdf/1807.04720.pdf

原名：The GAN Landscape: Losses, Architectures, Regularization, and Normalization

现名：A Large-Scale Study on Regularization and Normalization in GANs

Github：http://www.github.com/google/compare_gan

TensorFlow Hub：http://www.tensorflow.org/hub

翻译 参见 http://www.sohu.com/a/241299306_129720

--------------------------------------------------------------

GAN的很多研究，都是对Generative modeling生成模型的一种研究，主要有两种重要的工作：
1 Density Estimation 对原有数据进行密度估计，建模，然后使用模型进行估计
2 Sampling 取样，用对数据分布建模，并进行取样，生成符合原有数据分布的新数据。


![gang](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/gang.jpg)


----------------------------------------------
Others' collection:

https://github.com/kozistr/Awesome-GANs

https://github.com/nightrome/really-awesome-gan



------------------------------------------------
参考Mohammad KHalooei的教程，我也将GAN分为4个level，第四个level将按照应用层面进行拓展。 


# Level 0: Definition of GANs



|Level|	Title|	Co-authors|	Publication|	Links|
|:---:|:---:|:---:|:---:|:---:|
|Beginner|	GAN : Generative Adversarial Nets|	Goodfellow & et al.|	NeurIPS (NIPS) 2014	|[link](https://papers.nips.cc/paper/5423-generative-adversarial-nets.pdf) |
|Beginner|	GAN : Generative Adversarial Nets (Tutorial)|	Goodfellow & et al.|	NeurIPS (NIPS) 2016 Tutorial|	[link](https://arxiv.org/pdf/1701.00160.pdf)|
|Beginner|	CGAN : Conditional Generative Adversarial Nets|	Mirza & et al.|	-- 2014	|[link](https://gist.github.com/shagunsodhani/5d726334de3014defeeb701099a3b4b3) |
|Beginner|	InfoGAN : Interpretable Representation Learning by Information Maximizing Generative Adversarial Nets|	Chen & et al.|	NeuroIPS (NIPS) 2016	||


模型结构的发展：

在标准的GAN中，生成数据的来源一般是一段连续单一的噪声z, 在半监督式学习CGAN中，会加入c的class 分类。InfoGan 找到了Gan的latent code 使得Gan的数据生成具有了可解释性。

![ganmodule](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/ganmodule.png)





## CGAN

[1411.1784]Mirza M, Osindero S,Conditional Generative Adversarial Nets [pdf](https://arxiv.org/pdf/1411.1784.pdf) 

通过GAN可以生成想要的样本，以MNIST手写数字集为例，可以任意生成0-9的数字。

但是如果我们想指定生成的样本呢？譬如指定生成1，或者2，就可以通过指定C condition来完成。

条件BN首先出现在文章 Modulating early visual processing by language 中，后来又先后被用在 cGANs With Projection Discriminator 中，目前已经成为了做条件 GAN（cGAN）的标准方案，包括 SAGAN、BigGAN 都用到了它。


https://github.com/znxlwm/tensorflow-MNIST-cGAN-cDCGAN
![cgan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/cgan.png)

应用方向 数字生成， 图像自动标注等

## LAPGAN 
Emily Denton & Soumith Chintala, arxiv: 1506.05751

是第一篇将层次化或者迭代生成的思想运用到 GAN 中的工作。在原始 GAN和后来的 CGAN中，GAN 还只能生成32X32 这种低像素小尺寸的图片。而这篇工作[16] 是首次成功实现 64X64 的图像生成。思想就是，与其一下子生成这么大的（包含信息量这么多），不如一步步由小转大，这样每一步生成的时候，可以基于上一步的结果，而且还只需要“填充”和“补全”新大小所需要的那些信息。这样信息量就会少很多，而为了进一步减少信息量，他们甚至让 G 每次只生成“残差”图片，生成后的插值图片与上一步放大后的图片做加法，就得到了这一步生成的图片。


## IcGAN
Invertible Conditional GANs for image editing

通常GAN的生成网络输入为一个噪声向量z,IcGAN是对cGAN的z的解释。

利用一个encoder网络,对输入图像提取得到一个特征向量z,将特征向量z,以及需要转换的目标attribute向量y串联输入生成网络,得到生成图像,网络结构如下,

![icgan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/icgan.png)


https://arxiv.org/pdf/1611.06355.pdf
https://github.com/Guim3/IcGAN


## ACGAN

为了提供更多的辅助信息并允许半监督学习，可以向判别器添加额外的辅助分类器，以便在原始任务以及附加任务上优化模型。

和CGAN不同的是，C不直接输入D。D不仅需要判断每个样本的真假，还需要完成一个分类任务即预测C


添加辅助分类器允许我们使用预先训练的模型（例如，在ImageNet上训练的图像分类器），并且在ACGAN中的实验证明这种方法可以帮助生成更清晰的图像以及减轻模式崩溃问题。 使用辅助分类器还可以应用在文本到图像合成和图像到图像的转换。

![acgan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/acgan.png)


## SemiGan /SSGAN  Goodfellow

Salimans, Tim, et al. “Improved techniques for training gans.” Advances in Neural Information Processing Systems. 2016.


![ssgan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/semi.png)



Theano+Lasagne https://github.com/openai/improved-gan

tf: https://github.com/gitlimlab/SSGAN-Tensorflow

https://blog.csdn.net/shenxiaolu1984/article/details/75736407


----------------------
## InfoGan OpenAI

InfoGAN - Xi Chen, arxiv: 1606.03657

提出了latent code。

单一的噪声z，使得人们无法通过控制z的某些维度来控制生成数据的语义特征，也就是说，z是不可解释的。

以MNIST手写数字集为例，每个数字可以分解成多个维度特征：数字的类别、倾斜度、粗细度等等，在标准GAN的框架下，是无法在维度上具体指定生成什么样的数字。但是Info Gan 通过latent code的设定成功让网络学习到了可解释的特征表示（interpretable representation）

把原来的噪声z分解成两部分：一是原来的z；二是由若干个latent variables拼接而成的latent code c，这些latent variables会有一个先验的概率分布，且可以是离散的或连续的，用于代表生成数据的不同特征维度，如数字类别（离散），倾斜度（连续），粗细度（连续）等。通过找到对信息影响最大的c，来得到数据中最重要的特征。


InfoGAN: Interpretable Representation Learning by Information Maximizing Generative Adversarial Nets，NIPS 2016。

![info](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/infogan.png)


https://arxiv.org/abs/1606.03657

https://github.com/openai/InfoGAN


----------------------


# Level 1: Improvements of GANs training

然后看看 loss、参数、权重的改进：

|Level|	Title|	Co-authors|	Publication|	Links|
|:---:|:---:|:---:|:---:|:---:|
|Beginner	|LSGAN : Least Squares Generative Adversarial Networks	|Mao & et al.|	ICCV 2017|[link](https://ieeexplore.ieee.org/document/8237566)|	
|Advanced	|Improved Techniques for Training GANs	|Salimans & et al.|	NeurIPS (NIPS) 2016	|[link](https://ceit.aut.ac.ir/http://papers.nips.cc/paper/6125-improved-techniques-for-training-gans.pdf)|	
|Advanced	|WGAN : Wasserstein GAN	|Arjovsky & et al.|	ICML 2017|[link](http://proceedings.mlr.press/v70/arjovsky17a/arjovsky17a.pdf)|
|Advanced	|WGAN-GP : improved Training of Wasserstein GANs|	 2017|[link](https://arxiv.org/pdf/1704.00028v3.pdf)|
|Advanced	|Certifying Some Distributional Robustness with Principled Adversarial Training	|Sinha & et al.|ICML 2018|[link](https://arxiv.org/pdf/1710.10571.pdf) [code](https://github.com/duchi-lab/certifiable-distributional-robustness)|

Loss Functions:

## LSGAN(Least Squares Generative Adversarial Networks)

LS-GAN - Guo-Jun Qi, arxiv: 1701.06264

   [2] Mao et al., 2017.4 [pdf](https://arxiv.org/pdf/1611.04076.pdf)

 https://github.com/hwalsuklee/tensorflow-generative-model-collections
 https://github.com/guojunq/lsgan

用了最小二乘损失函数代替了GAN的损失函数,缓解了GAN训练不稳定和生成图像质量差多样性不足的问题。

但缺点也是明显的, LSGAN对离离群点的过度惩罚, 可能导致样本生成的'多样性'降低, 生成样本很可能只是对真实样本的简单模仿和细微改动.

## WGAN
WGAN - Martin Arjovsky, arXiv:1701.07875v1

WGAN：
在初期一个优秀的GAN应用需要有良好的训练方法，否则可能由于神经网络模型的自由性而导致输出不理想。 

为啥难训练？  令人拍案叫绝的Wasserstein GAN 中做了如下解释 ：
原始GAN不稳定的原因就彻底清楚了：判别器训练得太好，生成器梯度消失，生成器loss降不下去；判别器训练得不好，生成器梯度不准，四处乱跑。只有判别器训练得不好不坏才行，但是这个火候又很难把握，甚至在同一轮训练的前后不同阶段这个火候都可能不一样，所以GAN才那么难训练。

https://zhuanlan.zhihu.com/p/25071913

WGAN 针对loss改进 只改了4点：
1.判别器最后一层去掉sigmoid
2.生成器和判别器的loss不取log
3.每次更新判别器的参数之后把它们的绝对值截断到不超过一个固定常数c
4.不要用基于动量的优化算法（包括momentum和Adam），推荐RMSProp，SGD也行

https://github.com/martinarjovsky/WassersteinGAN


## WGAN-GP
Regularization and Normalization of the Discriminator:

![wgangp](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/wgangp.png)

WGAN-GP：

WGAN的作者Martin Arjovsky不久后就在reddit上表示他也意识到没能完全解决GAN训练稳定性，认为关键在于原设计中Lipschitz限制的施加方式不对，并在新论文中提出了相应的改进方案--WGAN-GP ,从weight clipping到gradient penalty,提出具有梯度惩罚的WGAN（WGAN with gradient penalty）替代WGAN判别器中权重剪枝的方法(Lipschitz限制)：

[1704.00028] Gulrajani et al., 2017,improved Training of Wasserstein GANs[pdf](https://arxiv.org/pdf/1704.00028v3.pdf)

Tensorflow实现：https://github.com/igul222/improved_wgan_training

pytorch https://github.com/caogang/wgan-gp



参考 ：

https://www.leiphone.com/news/201704/pQsvH7VN8TiLMDlK.html


----------------------


# Level 2: Implementation skill



![face](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/face.jpg)



GAN的实现

|Title|	Co-authors|	Publication|Links| size |FID/IS|
|:---:|:---:|:---:|:---:|:---:|:---:|
|Keras Implementation of GANs|	Linder-Norén|	Github	|[link](https://github.com/eriklindernoren/Keras-GAN)|||
|GAN implementation hacks|	Salimans paper & Chintala|	World research	|[link](https://github.com/soumith/ganhacks) [paper](https://ceit.aut.ac.ir/~khalooei/tutorials/gan/#gan-hack-paper-2016)|||
|DCGAN : Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks|	Radford & et al.|2015.11-ICLR 2016	|[link](https://github.com/carpedm20/DCGAN-tensorflow) [paper](https://arxiv.org/pdf/1511.06434.pdf)| 64x64 human||
|ProGAN:Progressive Growing of GANs for Improved Quality, Stability, and Variation|Tero Karras|2017.10|[paper](https://arxiv.org/pdf/1710.10196.pdf) [link](https://github.com/tkarras/progressive_growing_of_gans)|1024x1024 human|8.04|
|SAGAN：Self-Attention Generative Adversarial Networks| Han Zhang & Ian Goodfellow|2018.05|[paper](https://arxiv.org/pdf/1805.08318.pdf) [link](https://github.com/taki0112/Self-Attention-GAN-Tensorflow)|128x128 obj|18.65/52.52|
|BigGAN:Large Scale GAN Training for High Fidelity Natural Image Synthesis|Brock et al.|2018.09-ICLR 2019|[demo](https://tfhub.dev/deepmind/biggan-256) [paper](https://arxiv.org/pdf/1809.11096.pdf) [link](https://github.com/AaronLeong/BigGAN-pytorch)|512x512 obj|9.6/166.3|
|StyleGAN:A Style-Based Generator Architecture for Generative Adversarial Networks|Tero Karras|2018.12|[paper](https://arxiv.org/pdf/1812.04948.pdf) [link]( https://github.com/NVlabs/stylegan)|1024x1024 human|4.04|


指标：

1 Inception Score (IS，越大越好) IS用来衡量GAN网络的两个指标：1. 生成图片的质量 和2. 多样性

2 Fréchet Inception Distance (FID，越小越好) 在FID中我们用相同的inception network来提取中间层的特征。然后我们使用一个均值为 μμ 方差为 ΣΣ 的正态分布去模拟这些特征的分布。较低的FID意味着较高图片的质量和多样性。FID对模型坍塌更加敏感。

FID和IS都是基于特征提取，也就是依赖于某些特征的出现或者不出现。但是他们都无法描述这些特征的空间关系。

物体的数据在Imagenet数据库上比较，人脸的 progan 和stylegan 在CelebA-HQ和FFHQ上比较。上表列的为FFHQ指标。
## DCGAN

Deep Convolution Generative Adversarial Networks(深度卷积生成对抗网络)

Alec Radford & Luke Metz提出使用 CNN 结构来稳定 GAN 的训练，并使用了以下一些 trick：

Batch Normalization
使用 Transpose convlution 进行上采样
使用 Leaky ReLu 作为激活函数
上面这些 trick 对于稳定 GAN 的训练有许多帮助

这是CNN在unsupervised learning领域的一次重要尝试，这个架构能极大地稳定GAN的训练，以至于它在相当长的一段时间内都成为了GAN的标准架构，给后面的探索开启了重要的篇章。

![dcgan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/dcgang.jpg)


![dcganr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/dcganr.jpg)


## ImprovedDCGAN
 GANs的主要问题之一是收敛性不稳定，尽管DCGAN做了结构细化，训练过程仍可能难以收敛。GANs的训练常常是同时在两个目标上使用梯度下降，然而这并不能保证到达均衡点，毕竟目标未必是凸的。也就是说GANs可能永远达不到这样的均衡点，于是就会出现收敛性不稳定。

为了解决这一问题，ImprovedDCGAN针对DCGAN训练过程提出了不同的增强方法。
1 特征匹配(feature mapping)

为了不让生成器尽可能地去蒙骗鉴别器，ImprovedDCGAN希望以特征作为匹配标准，而不是图片作为匹配标准，于是提出了一种新的生成器的目标函数

2 批次判别(minibatch discrimination)

GAN的一个常见的失败就是收敛到同一个点，只要生成一个会被discriminator误认的内容，那么梯度方向就会不断朝那个方向前进。ImprovedDCGAN使用的方法是用minibatch discriminator。也就是说每次不是判别单张图片，而是判别一批图片。
3 历史平均(historical averaging)
4 单侧标签平滑(one-sided label smoothing)

## PGGAN(ProGAN)
Progressive Growing of GANs for Improved Quality, Stability, and Variation

Tero Karras, Timo Aila, Samuli Laine, Jaakko Lehtinen

首次实现了 1024 人脸生成的 Progressive Growing GANs，简称 PGGAN，来自 NVIDIA。

顾名思义，PGGAN 通过一种渐进式的结构，实现了从低分辨率到高分辨率的过渡，从而能平滑地训练出高清模型出来。论文还提出了自己对正则化、归一化的一些理解和技巧，值得思考。当然，由于是渐进式的，所以相当于要串联地训练很多个模型，所以 PGGAN 很慢。


![progan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/progan.gif)
论文地址：https://arxiv.org/pdf/1710.10196.pdf
代码实现地址：https://github.com/tkarras/progressive_growing_of_gans 


CelebA HQ 数据集


"随着 ResNet 在分类问题的日益深入，自然也就会考虑到 ResNet 结构在 GAN 的应用。事实上，目前 GAN 上主流的生成器和判别器架构确实已经变成了 ResNet：PGGAN、SNGAN、SAGAN 等知名 GAN 都已经用上了 ResNet

可以看到，其实基于 ResNet 的 GAN 在整体结构上与 DCGAN 并没有太大差别，主要的特点在于：
1) 不管在判别器还是生成器，均去除了反卷积，只保留了普通卷积层；
2) 通过 AvgPooling2D 和 UpSampling2D 来实现上/下采样，而 DCGAN 中则是通过 stride > 1 的卷积/反卷积实现的；其中 UpSampling2D 相当于将图像的长/宽放大若干倍；
3) 有些作者认为 BN 不适合 GAN，有时候会直接移除掉，或者用 LayerNorm 等代替。

然而，ResNet层数更多、层之间的连接更多，相比 DCGAN，ResNet比 DCGAN 要慢得多，所需要的显存要多得多。
                                           ---苏剑林

## SAGAN Ian Goodfellow
由于卷积的局部感受野的限制，如果要生成大范围相关（Long-range dependency）的区域会出现问题，用更深的卷积网络参数量太大，于是采用将 Self Attention 引入到了生成器（以及判别器）中，使用来自所有特征位置的信息生成图像细节，同时保证判别器能鉴别距离较远的两个特征之间的一致性，获取全局信息。
IS从36.8提到了52.52，并把FID（Fréchet Inception Distance）从27.62降到了18.65。
![sagan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/sagan.jpg)

![sagan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/sagan.png)

SAGAN 使用注意力机制，高亮部位为注意力机制关注的位置。

![saganr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/saganr.jpg)



论文地址：https://arxiv.org/pdf/1805.08318v2.pdf

https://github.com/taki0112/Self-Attention-GAN-Tensorflow

pytorch https://github.com/heykeetae/Self-Attention-GAN

## SELF-MOD

Self Modulated Generator，来自文章 On Self Modulation for Generative Adversarial Networks

SELF-MOD 考虑到 cGAN 训练的稳定性更好，但是一般情况下 GAN 并没有标签 c 可用，而以噪声 z 自身为标签好了，自己调节自己，不借助于外部标签，但能实现类似的效果。


## BigGAN

BigGAN — Brock et al. (2019) Large Scale GAN Training for High Fidelity Natural Image Synthesis”

https://arxiv.org/pdf/1809.11096.pdf

BigGAN模型是基于ImageNet生成图像质量最高的模型之一。BigGAN作为GAN发展史上的重要里程碑，将精度作出了跨越式提升。在ImageNet （128x128分辨率）训练下，将IS从52.52提升到166.3，FID从18.65降到9.6。
该模型很难在本地机器上实现，而且BigGAN有许多组件，如Self-Attention、 Spectral Normalization和带有投影鉴别器的cGAN，这些组件在各自的论文中都有更好的解释。不过，这篇论文对构成当前最先进技术水平的基础论文的思想提供了很好的概述，论文贡献包括，大batchsize，大channel数，截断技巧，训练平稳性控制等。（暴力出奇迹）

这篇文章提供了 128、256、512 的自然场景图片的生成结果。 自然场景图片的生成可是比 CelebA 的人脸生成要难上很多

![biggan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/biggan.png)

![bigganr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/bigganr.png)

Github：https://github.com/AaronLeong/BigGAN-pytorch


参考 https://mp.weixin.qq.com/s/9GeryvW5PI93FCmTpuFEPQ
此外 https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247495491&idx=1&sn=978f0afeb0b38affe54fc9e6d6086e3c&chksm=96ea30c3a19db9d52b735bdfee3f535ce68bcc6ace230b452b2ef8d389e66d32bba38e1574e3&scene=21#wechat_redirect


 Spectral Normalization 出自 《Spectral Norm Regularization for Improving the Generalizability of Deep Learning》 和 《Spectral Normalization for Generative Adversarial Networks》，是为了解决GAN训练不稳定的问题，从“层参数”的角度用spectral normalization 的方式施加regularization，从而使判别器D具备Lipschitz连续条件。


## StyleGAN  NVIDIA

A Style-Based Generator Architecture for Generative Adversarial Networks

被很多文章称之为 GAN 2.0，借鉴了风格迁移的模型，所以叫 Style-Based Generator

新数据集 FFHQ。

ProGAN是逐级直接生成图片，特征无法控制，相互关联，我们希望有一种更好的模型，能让我们控制生成图片过程中每一级的特征，要能够特定决定生成图片某些方面的表象，并且相互间的影响尽可能小。于是，在ProGAN的基础上，StyleGAN作出了进一步的改进与提升。

StyleGAN首先重点关注了ProGAN的生成器网络，它发现，渐进层的一个潜在的好处是，如果使用得当，它们能够控制图像的不同视觉特征。层和分辨率越低，它所影响的特征就越粗糙。简要将这些特征分为三种类型：
  1、粗糙的——分辨率不超过8^2，影响姿势、一般发型、面部形状等；
  2、中等的——分辨率为16^2至32^2，影响更精细的面部特征、发型、眼睛的睁开或是闭合等；
  3、高质的——分辨率为64^2到1024^2，影响颜色（眼睛、头发和皮肤）和微观特征；


![stylegan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/stylegan.png)

![stylegan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/stylegan.gif)
![styleganr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/styleganr.jpg)


![stylegan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/modelpic/gan/stylegan.png)


tf： https://github.com/NVlabs/stylegan

不存在系列：

人 ： https://thispersondoesnotexist.com/

动漫： https://www.thiswaifudoesnotexist.net/ 

https://www.obormot.net/demos/these-waifus-do-not-exist-alt

猫： http://thesecatsdonotexist.com/

换衣服： Generating High-Resolution Fashion Model Images Wearing Custom Outﬁts 1908.08847.pdf

other change clothers: https://arxiv.org/pdf/1710.07346.pdf


INSTAGAN: INSTANCE-AWARE IMAGE-TO-IMAGE TRANSLATION

加入实例 裤子换裙子  https://github.com/sangwoomo/instagan

论文链接：https://arxiv.org/pdf/1812.10889.pdf


Estimated StyleGAN wallclock training times for various resolutions & GPU-clusters (source: StyleGAN repo)

|GPUs |1024x2 |512x2| 256x2| [March 2019 AWS Costs19] |
|:---:|:---:|:---:|:---:|:---:|
|1	|41 days 4 hours [988 GPU-hours]	|24 days 21 hours [597 GPU-hours]|	14 days 22 hours [358 GPU-hours]|	[$320, $194, $115]|
|2	|21 days 22 hours [1,052]	|13 days 7 hours [638]	|9 days 5 hours [442]|	[NA]|
|4	|11 days 8 hours [1,088]	|7 days 0 hours [672] 	|4 days 21 hours [468]|	[NA]|
|8	|6 days 14 hours [1,264]	|4 days 10 hours [848]	|3 days 8 hours [640]|	[$2,730, $1,831, $1,382]|


## StyleGan2


## BigBiGAN DeepMind

https://arxiv.org/pdf/1907.02544.pdf

大规模对抗性表示学习
DeepMind基于最先进的BigGAN模型构建了BigBiGAN模型，通过添加编码器和修改鉴别器将其扩展到表示学习。

BigBiGAN表明，“图像生成质量的进步转化为了表示学习性能的显著提高”。

研究人员广泛评估了BigBiGAN模型的表示学习和生成性能，证明这些基于生成的模型在ImageNet上的无监督表示学习和无条件图像生成方面都达到了state of the art的水平。

[介绍](http://www.sohu.com/a/325681408_100024677)


PS:
O-GAN 可以加入其它的loss 将生成器 变为编码器。

通过简单地修改原来的GAN模型，就可以让判别器变成一个编码器，从而让GAN同时具备生成能力和编码能力，并且几乎不会增加训练成本。这个新模型被称为O-GAN（正交GAN，即Orthogonal Generative Adversarial Network），因为它是基于对判别器的正交分解操作来完成的，是对判别器自由度的最充分利用。

Arxiv链接：https://arxiv.org/abs/1903.01931

开源代码：https://github.com/bojone/o-gan

https://kexue.fm/archives/6409

TL-GAN ： 找到隐藏空间中的特征轴（如BigGAN PGGAN等，然后在特征轴上调节） [zhihu](https://zhuanlan.zhihu.com/p/48053933) [zhihu](https://zhuanlan.zhihu.com/p/47835790)

[日本的开源GAN插件，局部定制毫无压力 | Demo](https://zhuanlan.zhihu.com/p/62569491)



-------------------------------------------------------------
在现有的BigGan基础上，发展方向有了一个新的分支，就是减少标注数据，加入无监督学习。

## S³GAN（CompareGAN）


High-Fidelity Image Generation With Fewer Labels
https://arxiv.org/abs/1903.02271

Github ：https://github.com/google/compare_gan



-------------------------------------------------------

# Level 3: GANs Applications in CV

## 3.1 图像翻译 (Image Translation)

图像翻译，指从一副（源域）输入的图像到另一副（目标域）对应的输出图像的转换。它代表了图像处理的很多问题，比如灰度图、梯度图、彩色图之间的转换等。可以类比机器翻译，一种语言转换为另一种语言。翻译过程中会保持源域图像内容不变，但是风格或者一些其他属性变成目标域。

有标注数据的，被称为Paired Image-to-Image Translation，没有标注数据的，被称为 Unpaired Image-to-Image Translation。
一张图可以同时进行多领域转换的，称为Multiple Domain
- [ Paired two domain data](#1-Paired-Image-to-Image-Translation)
- [ Unpaired two domain data](#2-Unpaired-Image-to-Image-Translation)

![compare](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/compare.png)


|Title|	Co-authors|	Publication|Links|
|:---:|:---:|:---:|:---:|
|Pix2Pix |	Zhu & Park & et al.|CVPR 2017|[demo](https://affinelayer.com/pixsrv/) [code](https://phillipi.github.io/pix2pix/) [paper](https://arxiv.org/pdf/1611.07004v1.pdf)|
|Pix2Pix HD|NVIDIA UC Berkeley | CVPR 2018|[paper](https://arxiv.org/pdf/1711.11585v2.pdf) [code](https://github.com/NVIDIA/pix2pixHD)|
|SPADE|Nvidia|2019|[paper](https://arxiv.org/abs/1903.07291) [code](https://github.com/NVlabs/SPADE)|


|Title|	Co-authors|	Publication|Links|
|:---:|:---:|:---:|:---:|
|CoupledGan||2016|[paper](https://arxiv.org/abs/1606.07536) [code](https://github.com/mingyuliutw/CoGAN)|
|DTN||2017|[paper](https://arxiv.org/abs/1611.02200v1) [code](https://github.com/yunjey/domain-transfer-network)|
|CycleGan| |2017|[code](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix) [paper](https://arxiv.org/pdf/1703.10593.pdf)|
|DiscoGan ||2017|[paper](https://arxiv.org/abs/1703.05192)|
|DualGan||2017|[paper](https://arxiv.org/abs/1704.02510)|
|UNIT||2017||
|XGAN||2018||
|OST||2018||
|FUNIT||ICCV 2019|[paper](https://arxiv.org/pdf/1905.01723.pdf) [code](https://github.com/NVlabs/FUNIT) [demo](http://nvidia-research-mingyuliu.com/petswap)|

Multiple Domain

|Title|	Co-authors|	Publication|Links|
|:---:|:---:|:---:|:---:|
|Domain-Bank||2017||
|ComboGAN||2017||
|StarGan|| 2018||


## 1. Paired two domain data

成对图像翻译典型的例子就是 pix2pix，pix2pix 使用成对数据训练了一个条件 GAN，Loss 包括 GAN 的 loss 和逐像素差 loss。而 PAN 则使用特征图上的逐像素差作为感知损失替代图片上的逐像素差，以生成人眼感知上更加接近源域的图像。

## Pix2Pix

Image-to-Image Translation with Conditional Adversarial Networks

https://arxiv.org/pdf/1611.07004v1.pdf

传统的GAN也不是万能的，它有下面两个不足：

1. 没有用户控制（user control）能力
在传统的GAN里，输入一个随机噪声，就会输出一幅随机图像。但用户是有想法滴，我们想输出的图像是我们想要的那种图像，和我们的输入是对应的、有关联的。比如输入一只猫的草图，输出同一形态的猫的真实图片（这里对形态的要求就是一种用户控制）。

2. 低分辨率（Low resolution）和低质量（Low quality）问题
尽管生成的图片看起来很不错，但如果你放大看，就会发现细节相当模糊。
 ----------------朱俊彦（Jun-Yan Zhu） Games2018 Webinar 64期 ：[Siggraph 2018优秀博士论文报告](https://games-cn.org/games-webinar-20180913-64/)

Pix2Pix对传统的CGAN做了个小改动，它不再输入随机噪声，而是输入用户给的图片：

![pix2pix](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/pix2pix.png)


通过pix2pix来完成成对的图像转换(Labels to Street Scene, Aerial to Map,Day to Night等)，可以得到比较清晰的结果。

![pix2pixr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/pix2pixr.png)

代码：

官方project：https://phillipi.github.io/pix2pix/

官方torch代码：https://github.com/phillipi/pix2pix

官方pytorch代码（CycleGAN、pix2pix）：https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix

第三方的tensorflow版本：https://github.com/yenchenlin/pix2pix-tensorflow

https://github.com/affinelayer/pix2pix-tensorflow


demo: https://affinelayer.com/pixsrv/

Edges2cats： http://affinelayer.com/pixsrv/index.html

http://paintschainer.preferred.tech/index_zh.html

# Pix2Pix HD

High-Resolution Image Synthesis and Semantic Manipulation with Conditional GANs

https://arxiv.org/pdf/1711.11585v2.pdf

Ming-Yu Liu在介入过许多CV圈内耳熟能详的项目,包括vid2vid、pix2pixHD、CoupledGAN、FastPhotoStyle、MoCoGAN


1 模型结构
2 Loss设计
3 使用Instance-map的图像进行训练。

![pix2pixhd](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/pix2pixhd.png)

![pix2pixhd](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/pix2pixhd.gif)

官方代码 ：https://github.com/NVIDIA/pix2pixHD

pix2pix的核心是有了对应关系，这种网络的应用范围还是比较广泛的，如草稿变图片，自动上色，交互式上色等。

## 2. Unpaired two domain data

对于无成对训练数据的图像翻译问题，一个典型的例子是 CycleGAN。CycleGAN 使用两对 GAN，将源域数据通过一个 GAN 网络转换到目标域之后，再使用另一个 GAN 网络将目标域数据转换回源域，转换回来的数据和源域数据正好是成对的，构成监督信息。

## CoGAN (CoupledGAN)

CoGAN:Coupled Generative Adversarial Networks

CoGAN会训练两个GAN而不是一个单一的GAN。

当然，GAN研究人员不停止地将此与那些警察和伪造者的博弈理论进行类比。所以这就是CoGAN背后的想法，用作者自己的话说就是：

在游戏中，有两个团队，每个团队有两个成员。生成模型组成一个团队，在两个不同的域中合作共同合成一对图像，用以混淆判别模型。判别模型试图将从各个域中的训练数据分布中绘制的图像与从各个生成模型中绘制的图像区分开。同一团队中，参与者之间的协作是根据权重分配约束建立的。这样就有了一个GAN的多人局域网竞赛

CoupledGAN 通过部分权重共享学习到多个域图像的联合分布。生成器前半部分权重共享，目的在于编码两个域高层的，共有信息，后半部分没有进行共享，则是为了各自编码各自域的数据。判别器前半部分不共享，后半部分用于提取高层特征共享二者权重。对于训练好的网络，输入一个随机噪声，输出两张不同域的图片。

值得注意的是，上述模型学习的是联合分布 P(x,y)，如果使用两个单独的 GAN 分别取训练，那么学习到的就是边际分布 P(x) 和 P(y)。。


论文：

https://arxiv.org/abs/1606.07536

代码：

https://github.com/mingyuliutw/CoGAN

博客：

https://wiseodd.github.io/techblog/2017/02/18/coupled_gan/




## CycleGan /DiscoGan /DualGan

CycleGan: Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks

朱俊彦

https://arxiv.org/abs/1703.10593

同一时期还有两篇非常类似的文章，同样的idea，同样的结构，同样的功能：DualGAN( https://arxiv.org/abs/1704.02510 ) 和 DiscoGAN( Learning to Discover Cross-Domain Relations with Generative Adversarial Networks ： https://arxiv.org/abs/1703.05192)

CycleGan是让两个domain的图片互相转化。传统的GAN是单向生成，而CycleGAN是互相生成，一个A→B单向GAN加上一个B→A单向GAN，网络是个环形，所以命名为Cycle。理念就是，如果从A生成的B是对的，那么从B再生成A也应该是对的。CycleGAN输入的两张图片可以是任意的两张图片，也就是unpaired。

![CycleGan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/cyclegan.png)

![CycleGanr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/cyclegan.jpg)

官方pytorch代码（CycleGAN、pix2pix）：https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix

有趣的应用：
地图转换
风格转换
游戏画风转换：Chintan Trivedi的实现：用CycleGAN把《堡垒之夜》转成《绝地求生》写实风。


## FUNIT

Few-Shot Unsupervised Image-to-Image Translation

https://arxiv.org/pdf/1905.01723.pdf

小样本(few-shot)非监督图像到图像转换。

https://github.com/NVlabs/FUNIT

主要解决两个问题，小样本Few-shot和没见过的领域转换Unseen Domains。
人能针对一个新物种，看少量样本，也能进行想象和推算 。关键就是 一个大类型的物种中，信息可以相互转换。

![FUNIT](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/FUNIT.png)

![FUNITr](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/FUNITr.png)

demo http://nvidia-research-mingyuliu.com/petswap


## StarGan

StarGAN的引入是为了解决多领域间的转换问题的，之前的CycleGAN等只能解决两个领域之间的转换，那么对于含有C个领域转换而言，需要学习Cx(C-1)个模型，但StarGAN仅需要学习一个

![starGan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/stargan.png)



https://arxiv.org/pdf/1711.09020.pdf

pytorch 原版github地址：https://github.com/yunjey/StarGAN 
tf版github地址：https://github.com/taki0112/StarGAN-Tensorflow 



-----------------------------------------

## 3.2 超分辨率 (Super-Resolution)

超分辨率的问题研究由来已久，其目标是将低分辨率图像恢复或重建为高分辨率图像，随着GAN的发展，使得这个问题有了惊人的进展。这项技术得以广泛应用于卫星和航天图像分析、医疗图像处理、压缩图像/视频增强及手机摄像领域，有着明确商业用途。SR技术存在一个有趣的“悖论”，即还原或重建后的高分辨率图像与原图相似度越高，则肉眼观察清晰度越差；反之，若肉眼观察清晰度越好，则图像的失真度越高。导致这一现象的原因在于畸变（Distortion）参数和感知（Perception）参数之间侧重点选择的不同。

传统方法有Google发布的 RAISR: Rapid and Accurate Image Super Resolution(2016 [paper](https://arxiv.org/pdf/1606.01299.pdf) )
国内也同期都发布了自己的算法，如腾讯发布的TSR(Tencent Super Resolution），华为的HiSR等。

超分辨率的比赛 为 NTIRE

客观指标：Peak signal-to-noise ratio (PSNR)

主观指标：在纯的超分辨领域，评价性能的指标是 PSNR（和 MSE 直接挂钩），所以如果单纯看 PSNR 值可能还是 L2 要好。如果考虑主观感受的话估计 L1 要好。

Others’ Collection：其他人收集的相关工作：

https://github.com/icpm/super-resolution

https://github.com/YapengTian/Single-Image-Super-Resolution

https://github.com/huangzehao/Super-Resolution.Benckmark

https://github.com/ChaofWang/Awesome-Super-Resolution

SR可分为两类:从多张低分辨率图像重建出高分辨率图像和从单张低分辨率图像重建出高分辨率图像。基于深度学习的SR，主要是基于单张低分辨率的重建方法，即Single Image Super-Resolution (SISR)。

SISR是一个逆问题，对于一个低分辨率图像，可能存在许多不同的高分辨率图像与之对应，因此通常在求解高分辨率图像时会加一个先验信息进行规范化约束。在传统的方法中，这个先验信息可以通过若干成对出现的低-高分辨率图像的实例中学到。而基于深度学习的SR通过神经网络直接学习分辨率图像到高分辨率图像的端到端的映射函数。

较新的基于深度学习的SR方法，包括SRCNN，DRCN, ESPCN，VESPCN和SRGAN等。(SRCNN[1]、FSRCNN[2]、ESPCN[3]、VDSR[4]、EDSR[5]、SRGAN[6])


1. (SRCNN) Image super-resolution using deep convolutional networks

2. (DRCN) Deeply-recursive convolutional network for image super-resolution

3. (ESPCN) Real-time single image and video super-resolution using an efficient sub-pixel convolutional neural network

4. (VESPCN) Real-Time Video Super-Resolution with Spatio-Temporal Networks and Motion Compensation 

5. Spatial transformer networks 

6. Photo-realistic single image super-resolution using a generative adversarial network (SRGAN)


3.2.1 单张图像超分辨率（Single Image Super-Resolution)

Title	Co-authors	Publication	Links
|:---:|:---:|:---:|:---:|

## 

StackGAN: Text to Photo-realistic Image Synthesis with Stacked Generative Adversarial Networks	Zhang & et al.	ICCV 2017

GAN 对于高分辨率图像生成一直存在许多问题，层级结构的 GAN 通过逐层次，分阶段生成，一步步提生图像的分辨率。典型的使用多对 GAN 的模型有 StackGAN，GoGAN。使用单一 GAN，分阶段生成的有 ProgressiveGAN。



## SRGAN 
SRGAN，2017 年 CVPR 中备受瞩目的超分辨率论文，把超分辨率的效果带到了一个新的高度，而 2017 年超分大赛 NTIRE 的冠军 EDSR 也是基于 SRGAN 的变体。

SRGAN 是基于 GAN 方法进行训练的，有一个生成器和一个判别器，判别器的主体使用 VGG19，生成器是一连串的 Residual block 连接，同时在模型后部也加入了 subpixel 模块，借鉴了 Shi et al 的 Subpixel Network 思想，重点关注中间特征层的误差，而不是输出结果的逐像素误差。避免了生成的高分辨图像缺乏纹理细节信息问题。让图片在最后面的网络层才增加分辨率，提升分辨率的同时减少计算资源消耗。

胡志豪提出一个来自工业界的问题
在实际生产使用中，遇到的低分辨率图片并不一定都是 PNG 格式的（无损压缩的图片复原效果最好），而且会带有不同程度的失真（有损压缩导致的 artifacts）。很多算法包括SRGAN、EDSR、RAISR、Fast Neural Style 等等都没法在提高分辨率的同时消除失真。
Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network (https://arxiv.org/abs/1609.04802, 21 Nov, 2016)这篇文章将对抗学习用于基于单幅图像的高分辨重建。基于深度学习的高分辨率图像重建已经取得了很好的效果，其方法是通过一系列低分辨率图像和与之对应的高分辨率图像作为训练数据，学习一个从低分辨率图像到高分辨率图像的映射函数，这个函数通过卷积神经网络来表示。

得益于 GAN 在超分辨中的应用，针对小目标检测问题，可以通过 GAN 生成小目标的高分辨率图像从而提高目标检测精度

TensorFlow 版本：https://github.com/buriburisuri/SRGAN

Torch 版本：https://github.com/leehomyc/Photo-Realistic-Super-Resoluton

Keras 版本：https://github.com/titu1994/Super-Resolution-using-Generative-Adversarial-Networks


## ESRGAN 

ECCV 2018收录，赢得了PIRM2018-SR挑战赛的第一名。


其他应用 ：
Google 马赛克去除 ( Pixel Recursive Super Resolution https://arxiv.org/abs/1702.00783)


-------------------------
## 3.3 交互式图像生成
## iGAN
基于DCGAN，Adobe公司构建了一套图像编辑操作，能使得经过这些操作以后，图像依旧在“真实图像流形”上，因此编辑后的图像更接近真实图像。

具体来说，iGAN的流程包括以下几个步骤：

1 将原始图像投影到低维的隐向量空间

2 将隐向量作为输入，利用GAN重构图像

3 利用画笔工具对重构的图像进行修改（颜色、形状等）

4 将等量的结构、色彩等修改应用到原始图像上。


值得一提的是，作者提出G需为保距映射的限制，这使得整个过程的大部分操作可以转换为求解优化问题，整个修改过程近乎实时。

Theano 版本：https://github.com/junyanz/iGAN


[24] Jun-Yan Zhu, Philipp Krähenbühl, Eli Shechtman and Alexei A. Efros. “Generative Visual Manipulation on the Natural Image Manifold”, ECCV 2016.

## GANpaint

GAN dissection

MIT、香港中文大学、IBM等学校/机构的David Bau、朱俊彦、Joshua B.Tenenbaum、周博磊

http://gandissect.res.ibm.com/ganpaint.html?project=churchoutdoor&layer=layer4
 


### GauGAN（SPADE） Nvidia


你画一幅涂鸦，用颜色区分每一块对应着什么物体，它就能照着你的大作，合成以假乱真的真实世界效果图。
通过语义布局进行图像的生成 Segmentation mask，算法是源于Pix2PixHD，生成自然的图像。

数据来源是成对的，通过自然场景的图像进行分割，就可以得到分割图像的布局，组成了对应的图像对。
但是区别在于，之前的Pix2PixHD，场景都很规律，如室内，街景，可以使用BN，但是后来发现Pix2PixHD在COCO这些无限制的数据集训练结果很差。如果整张天空或者整张的草地，则计算通过BN后，结果很相似，这样合成会出现问题。于是修改了BN，这种方法称为空间自适应归一化合成法SPADE。将原来的label信息代入到原来BN公式中的γ和β

Semantic Image Synthesis with Spatially-Adaptive Normalization--CVPR 2019。

这篇论文的一作，照例还是实习生。另外几位作者来自英伟达和MIT，CycleGAN的创造者朱俊彦是四作。

在基于语义合成图像这个领域里，这可是目前效果最强的方法。

![gaugan](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/gaugan.jpg)


paper：https://arxiv.org/abs/1903.07291

GitHub：https://github.com/NVlabs/SPADE

项目地点：https://nvlabs.github.io/SPADE/

https://nvlabs.github.io/SPADE/demo.html
https://nvlabs.github.io/SPADE/

https://36kr.com/p/5187136

demo:

https://nvlabs.github.io/SPADE/demo.html
http://nvidia-research-mingyuliu.com/gaugan

https://www.nvidia.com/en-us/research/ai-playground/

---------------------------

## AutoDraw

AutoDraw能将机器学习与你信手涂鸦创建的图形配对，帮助你绘制完整而比较漂亮的图形。

--------------------------

## 3.4  Image Inpainting(图像修复)/Image Outpainting(图像拓展)/图像融合

图像修复传统算法：PatchMatch  PatchMatch: A Randomized Correspondence Algorithm for Structural Image Editing 、
 Space-Time completion of Image，据说Adobe ps cs5 中使用作为图像填充。不断迭代的用相似的非孔区域图片块来替换孔区域图片块。 优点是一张图片就行。缺点：慢，非语义


 Fast Marching (OpenCV)

深度学习有了不一样的发展:


| Paper | Source | Code/Project Link  |
| --- | --- | --- |
|Context-Encoders:Feature Learning by Inpainting|CVPR 2016|[code](https://github.com/pathak22/context-encoder​)|
|High-Resolution Image Inpainting using Multi-Scale Neural Patch Synthesis|CVPR 2017|[code](https://github.com/leehomyc/Faster-High-Res-Neural-Inpainting​) [paper](https://arxiv.org/pdf/1611.09969.pdf)|
|Semantic Image Inpainting with Perceptual and Contextual Losses| CVPR2017|[code](https://github.com/bamos/dcgan-completion.tensorflow)|
|On-Demand Learning for Deep Image Restoration| ICCV 2017|[code](https://github.com/rhgao/on-demand-learning​)|
|Globally and Locally Consistent Image Completion | SIGGRAPH 2017 |[code](https://github.com/satoshiiizuka/siggraph2017_inpainting) [code](https://github.com/shinseung428/GlobalLocalImageCompletion_TF)|
|Deep image prior |2017.12|[code](https://dmitryulyanov.github.io/deep_image_prior)|
|Image Inpainting for Irregular Holes Using Partial Convolutions​| ICLR 2018| [code](https://github.com/deeppomf/DeepCreamPy) [paper](https://arxiv.org/pdf/1804.07723.pdf)|
|Deepfill v1:Generative Image Inpainting with Contextual Attention|CVPR 2018| [code](https://github.com/JiahuiYu/generative_inpainting​)|
|Deepfill v2:Free-Form Image Inpainting with Gated Convolution||[paper](https://arxiv.org/abs/1806.03589)|
|Shift-Net: Image Inpainting via Deep Feature Rearrangement |||
|Contextual-based Image Inpainting|ECCV 2018|[paper](https://arxiv.org/abs/1711.08590v3)|
|Image Inpainting via Generative Multi-column Convolutional Neural Networks| NIPS 2018| [code](https://github.com/shepnerd/inpainting_gmcnn​)|
|PGN：Semantic Image Inpainting with Progressive Generative Networks| ACM MM 2018|[code](https://github.com/crashmoon/Progressive-Generative-Networks​)|
|EdgeConnect|2019|[code](https://github.com/knazeri/edge-connect)|
|MUSICAL: Multi-Scale Image Contextual Attention Learning for Inpainting|IJCAI 2019|[link](sigma.whu.edu.cn)|
|Coherent Semantic Attention for Image Inpainting|2019|[code](https://github.com/KumapowerLIU)|
|Foreground-aware Image Inpainting|CVPR 2019|[pdf](https://arxiv.org/abs/1901.05945v1)|
|Pluralistic Image Completion|CVPR2019|[code](https://github.com/lyndonzheng/Pluralistic-Inpainting) [paper](https://arxiv.org/abs/1903.04227​)|


## Deep image prior

项目主页：https://dmitryulyanov.github.io/deep_image_prior

github链接：https://github.com/DmitryUlyanov/deep-image-prior 

## Partial Conv：IMAGE INPAINTING Nvidia

Partial Convolution based Padding 
Guilin Liu, Kevin J. Shih, Ting-Chun Wang, Fitsum A. Reda, Karan Sapra, Zhiding Yu, Andrew Tao, Bryan Catanzaro 
NVIDIA Corporation 
Technical Report (Technical Report) 2018

Image Inpainting for Irregular Holes Using Partial Convolutions 
Guilin Liu, Fitsum A. Reda, Kevin J. Shih, Ting-Chun Wang, Andrew Tao, Bryan Catanzaro 
NVIDIA Corporation 
In The European Conference on Computer Vision (ECCV) 2018 

号称秒杀PS的AI图像修复神器，来自于Nvidia 研究团队。引入了局部卷积，只对部分区域做卷积，而破损区域置0,`能够修复任意非中心、不规则区域

在此之前基于深度学习的方法缺点： 
- 有孔区域需要设置初始值，深度学习网络会混淆那些初始值，以为是非孔区域数据。
- 需要后期要处理
- 只能处理规则孔

https://arxiv.org/pdf/1804.07723.pdf

https://www.nvidia.com/en-us/research/ai-playground/?ncid=so-twi-nz-92489DeepCreamPy

Partial Convolution based Padding.
https://github.com/NVIDIA/partialconv

deeppomf 开源了 Image Inpainting for Irregular Holes Using Partial Convolutions 的修复实现DeepCreamPy


预构建模型下载地址：https://github.com/deeppomf/DeepCreamPy/releases

预训练模型地址：https://drive.google.com/open?id=1byrmn6wp0r27lSXcT9MC4j-RQ2R04P1Z

https://github.com/deeppomf/DeepCreamPy



拓展：走红网络的一键生成裸照软件DeepNude，原站已关，延伸： https://github.com/yuanxiaosc/DeepNude-an-Image-to-Image-technology


## DeepFill
 
论文链接：https://arxiv.org/abs/1801.07892

github链接：https://github.com/JiahuiYu/generative_inpainting    

V2:《Free-Form Image Inpainting with Gated Convolution》

论文链接：https://arxiv.org/abs/1806.03589


## EdgeConnect：使用对抗边缘学习进行生成图像修复


https://github.com/knazeri/edge-connect

https://github.com/youyuge34/Anime-InPainting

## Foreground-aware Image Inpainting Adobe 

https://arxiv.org/abs/1901.05945v1

[Adobe放出P图新研究：就算丢了半个头，也能逼真复原](https://tech.sina.com.cn/csj/2019-01-22/doc-ihrfqziz9984559.shtml)


## Noise2Noise ：医学

2018年ICML 
将此项技术应用于含有大量噪声的图像，比如天体摄影、核磁共振成像（MRI）以及大脑扫描图像等。

使用来自IXI数据集近5000张图像来训练Noise2Noise的MRI图像去噪能力。在没有人工噪声的情况下，结果可能比原始图像稍微模糊一些，但仍然很好地还原了清晰度。


论文链接：https://arxiv.org/pdf/1803.04189.pdf

----------------------------------------------------------------------------------------

## Deep Flow-Guided 视频修复/视频内容消除


原文链接

https://nbei.github.io/video-inpainting.html

论文地址

https://arxiv.org/abs/1905.02884?context=cs

Github 开源地址

https://github.com/nbei/Deep-Flow-Guided-Video-Inpainting

[zhihu -量子位](https://zhuanlan.zhihu.com/p/73645545)


--------------------------------------------------------------------------------------------

## Painting Outside the Box: Image Outpainting 

https://cs230.stanford.edu/projects_spring_2018/posters/8265861.pdf

https://github.com/bendangnuksung/Image-OutPainting



---------------------------------------------------
## GP-GAN

GP-GAN，目标是将直接复制粘贴过来的图片，更好地融合进原始图片中，做一个 blending 的事情。

这个过程非常像 iGAN，也用到了类似 iGAN 中的一些约束，比如 color constraint。另一方面，这个工作也有点像 pix2pix，因为它是一种有监督训练模型，在 blending 的学习过程中，会有一个有监督目标和有监督的损失函数。

2017 https://arxiv.org/pdf/1703.07195.pdf
 
## Deep Painterly Harmonization
https://github.com/luanfujun/deep-painterly-harmonization
这个算法将你选择的外部物体添加到了任意一张图像上，并成功让它看上去好像本来就应该在那里一样。你不妨查看这个代码，然后尝试亲自到一系列不同的图像上去操作这项技术。


------------------------------------------------------------------------



## 3.5 图像上色  Colourful Image Colorization
在计算机图形学领域中，一直存在两种为图片上色的方向：数据驱动的自动上色和用户交互的引导上色。

- [Automatic Image Colorization](#1-automatic-image-colorization)
- [User Guided Image Colorization](#2-user-guided-image-colorization)
  - [Based on color strokes](#21-based-on-color-strokes)
  - [Based on reference color image](#22-based-on-reference-color-image)
  - [Based on color palette](#23-based-on-color-palette)
  - [Based on language(text)](#24-based-on-language-or-text)
- [Video Colorization](#3-video-colorization)
  - [Automatically](#31-automatically)
  - [Based on reference](#32-based-on-reference)



### 1. Automatic Image Colorization


| Paper | Source | Code/Project Link  |
| --- | --- | --- |
| [Learning Large-Scale Automatic Image Colorization](http://openaccess.thecvf.com/content_iccv_2015/papers/Deshpande_Learning_Large-Scale_Automatic_ICCV_2015_paper.pdf) | ICCV 2015 | [[project]](http://vision.cs.illinois.edu/projects/lscolor/) [[code]](https://github.com/aditya12agd5/iccv15_lscolorization) |
| [Deep Colorization](http://openaccess.thecvf.com/content_iccv_2015/papers/Cheng_Deep_Colorization_ICCV_2015_paper.pdf) | ICCV 2015 |  |
| [Learning Representations for Automatic Colorization](https://arxiv.org/pdf/1603.06668.pdf) | ECCV 2016 | [[project]](http://people.cs.uchicago.edu/~larsson/colorization/) [[code]](https://github.com/gustavla/autocolorize) |
| [Colorful Image Colorization](https://arxiv.org/pdf/1603.08511.pdf) | ECCV 2016 | [[project]](http://richzhang.github.io/colorization/) [[code]](https://github.com/richzhang/colorization) |
| [Let there be Color!: Joint End-to-end Learning of Global and Local Image Priors for Automatic Image Colorization with Simultaneous Classification](http://iizuka.cs.tsukuba.ac.jp/projects/colorization/data/colorization_sig2016.pdf) | SIGGRAPH 2016 | [[project]](http://iizuka.cs.tsukuba.ac.jp/projects/colorization/en/) [[code]](https://github.com/satoshiiizuka/siggraph2016_colorization) |
| [Unsupervised Diverse Colorization via Generative Adversarial Networks](https://arxiv.org/pdf/1702.06674.pdf) | ECML-PKDD 2017 | [[code]](https://github.com/ccyyatnet/COLORGAN) |
| [Learning Diverse Image Colorization](http://openaccess.thecvf.com/content_cvpr_2017/papers/Deshpande_Learning_Diverse_Image_CVPR_2017_paper.pdf) | CVPR 2017 | [[code]](https://github.com/aditya12agd5/divcolor) |
| [Structural Consistency and Controllability for Diverse Colorization](http://openaccess.thecvf.com/content_ECCV_2018/papers/Safa_Messaoud_Structural_Consistency_and_ECCV_2018_paper.pdf) | ECCV 2018 |  |
| [Pixelated Semantic Colorization](https://arxiv.org/abs/1901.10889) | 1901.10889 |  |
| [Coloring With Limited Data: Few-Shot Colorization via Memory Augmented Networks](http://davian.korea.ac.kr/filemanager/wl/?id=BPD0GpKupqUgHTxRMpaaLbCDrNoEjVfu) | CVPR 2019 |  |



#### Colourful Image Colourization 
2016 ECCV 里加州大学伯克利分校的一篇文章介绍的方法。这个方法与之前方法的不同之处在于，它把照片上色看成是一个分类问题——预测三百多种颜色在图片每一个像素点上的概率分布。这种方法tackle了这个任务本身的不确定性，例如，当你看到一个黑白的苹果时，你可能会觉得它是红色的，但如果这个苹果是青色的，其实也并没有多少违和感。大家也可以到作者的网站网站来试用他们的demo。 
https://richzhang.github.io/colorization/

#### Colornet

Github 地址：https://github.com/pavelgonchar/colornet

###  2. User Guided Image Colorization

这种方法是由 Levin 等人在 2004 年开创的，用户通过彩色画笔在灰度图像中进行引导性上色，随后优化算法会生成符合用户逻辑的上色结果。这种方法可以保留人工上色的部分性质，因而经常会有绝佳的表现，但往往需要密集的用户交互次数（有时超过五十次）。随着技术进步，现在的交互次数慢慢减少。

#### 2.1 Based on color strokes

| Image Type | Paper | Source | Code/Project Link  |
| --- | --- | --- |--- |
| Manga | [Manga colorization](https://dl.acm.org/citation.cfm?id=1142017) | SIGGRAPH 2006 |  |
| Line art / Sketch | [Outline Colorization through Tandem Adversarial Networks](https://arxiv.org/abs/1704.08834) | 1704.08834 | [[Demo]](http://color.kvfrans.com/) [[code]](https://github.com/kvfrans/deepcolor) |
| Line art / Sketch | [Auto-painter: Cartoon Image Generation from Sketch by Using Conditional Generative Adversarial Networks](https://arxiv.org/pdf/1705.01908.pdf) | 1705.01908 | [[code]](https://github.com/irfanICMLL/Auto_painter) |
| Natural Gray-Scale | [Real-Time User-Guided Image Colorization with Learned Deep Priors](https://arxiv.org/abs/1705.02999) | SIGGRAPH 2017 | [[project]](https://richzhang.github.io/ideepcolor/) [[code1]](https://github.com/junyanz/interactive-deep-colorization) [[code2]](https://github.com/richzhang/colorization-pytorch) |
| Sketch | [Scribbler: Controlling Deep Image Synthesis with Sketch and Color](http://openaccess.thecvf.com/content_cvpr_2017/papers/Sangkloy_Scribbler_Controlling_Deep_CVPR_2017_paper.pdf) | CVPR 2017 |  |
| Natural Gray-Scale | [Interactive Deep Colorization Using Simultaneous Global and Local Inputs](https://ieeexplore.ieee.org/abstract/document/8683686) (also palette based) | ICASSP 2019 |  |


#### Real-Time User-Guided Image Colorization with Learned Deep Priors

UC Berkeley  SIGGRAPH 2017 
论文链接：https://arxiv.org/abs/1705.02999
Demo 和代码链接：https://richzhang.github.io/ideepcolor/


#### 2.2 Based on reference color image

| Image Type | Paper | Source | Code/Project Link  |
| --- | --- | --- |--- |
| Line art | Style2paints V1 : [Style Transfer for Anime Sketches with Enhanced Residual U-net and Auxiliary Classifier GAN](https://arxiv.org/abs/1706.03319) | ACPR 2017 | [[Code]](https://github.com/lllyasviel/style2paints#style2paints-v1) |
| Manga | [Comicolorization: Semi-Automatic Manga Colorization](https://arxiv.org/pdf/1706.06759.pdf) (also palette based) | SIGGRAPH Asia 2017 | [[code]](https://github.com/DwangoMediaVillage/Comicolorization) |
| Sketch | [TextureGAN: Controlling Deep Image Synthesis with Texture Patches](http://openaccess.thecvf.com/content_cvpr_2018/papers/Xian_TextureGAN_Controlling_Deep_CVPR_2018_paper.pdf) | CVPR 2018 | [[code]](https://github.com/janesjanes/Pytorch-TextureGAN) |
| Natural Gray-Scale | [Deep Exemplar-based Colorization](https://arxiv.org/pdf/1807.06587.pdf) | SIGGRAPH 2018 | [[code]](https://github.com/msracver/Deep-Exemplar-based-Colorization) |
| Natural Gray-Scale | [Example-Based Colourization Via Dense Encoding Pyramids](http://www.shengfenghe.com/uploads/1/5/1/3/15132160/cgf_13659_rev_ev.pdf) (also palette based) | Pacific Graphics 2018 | [[code]](https://github.com/chufengxiao/Example-based-Colorization-via-Dense-Encoding-pyramids) |
| Natural Gray-Scale | [A Superpixel-based Variational Model for Image Colorization](https://ieeexplore.ieee.org/abstract/document/8676327) | TVCG 2019 |  |
| Natural Gray-Scale | [Automatic Example-based Image Colourisation using Location-Aware Cross-Scale Matching](https://ieeexplore.ieee.org/abstract/document/8699109) | TIP 2019 |  |

#### 2.3 Based on color palette

| Image Type | Paper | Source | Code/Project Link  |
| --- | --- | --- |--- |
| Natural Image | [Palette-based Photo Recoloring](https://gfx.cs.princeton.edu/pubs/Chang_2015_PPR/chang2015-palette_small.pdf) | SIGGRAPH 2015 | [[project]](https://gfx.cs.princeton.edu/pubs/Chang_2015_PPR/index.php) |
| Manga | [Comicolorization: Semi-Automatic Manga Colorization](https://arxiv.org/pdf/1706.06759.pdf) (also reference based) | SIGGRAPH Asia 2017 | [[code]](https://github.com/DwangoMediaVillage/Comicolorization) |
| Natural Gray-Scale | [Coloring with Words: Guiding Image Colorization Through Text-based Palette Generation](https://arxiv.org/pdf/1804.04128.pdf) (also text based) | ECCV 2018 | [[code]](https://github.com/awesome-davian/Text2Colors/) |
| Natural Gray-Scale | [Example-Based Colourization Via Dense Encoding Pyramids](http://www.shengfenghe.com/uploads/1/5/1/3/15132160/cgf_13659_rev_ev.pdf) (also reference based) | Pacific Graphics 2018 | [[code]](https://github.com/chufengxiao/Example-based-Colorization-via-Dense-Encoding-pyramids) |
| Natural Gray-Scale | [Interactive Deep Colorization Using Simultaneous Global and Local Inputs](https://ieeexplore.ieee.org/abstract/document/8683686) (also strokes based) | ICASSP 2019 |  |

#### 2.4 Based on language or text

| Image Type | Paper | Source | Code/Project Link  |
| --- | --- | --- |--- |
| Natural Gray-Scale / Sketch | [Language-Based Image Editing with Recurrent Attentive Models](https://arxiv.org/pdf/1711.06288.pdf) | CVPR 2018 | [[code]](https://github.com/Jianbo-Lab/LBIE) |
| Natural Gray-Scale | [Coloring with Words: Guiding Image Colorization Through Text-based Palette Generation](https://arxiv.org/pdf/1804.04128.pdf) (also palette based) | ECCV 2018 | [[code]](https://github.com/awesome-davian/Text2Colors/) |
| Scene Sketch | [LUCSS: Language-based User-customized Colorization of Scene Sketches](https://arxiv.org/pdf/1808.10544.pdf) | 1808.10544 | [[code]](https://github.com/SketchyScene/LUCSS) |


### 3. Video Colorization

#### 3.1 Automatically

| Paper | Source | Code/Project Link  |
| --- | --- |--- |
| [Fully Automatic Video Colorization with Self-Regularization and Diversity](https://cqf.io/papers/Fully_Automatic_Video_Colorization_CVPR2019.pdf) | CVPR 2019 |  |


#### 3.2 Based on reference

| Paper | Source | Code/Project Link  |
| --- | --- |--- |
| [Switchable Temporal Propagation Network](http://openaccess.thecvf.com/content_ECCV_2018/papers/Sifei_Liu_Switchable_Temporal_Propagation_ECCV_2018_paper.pdf) | ECCV 2018 |  |
| [Tracking Emerges by Colorizing Videos](http://openaccess.thecvf.com/content_ECCV_2018/papers/Carl_Vondrick_Self-supervised_Tracking_by_ECCV_2018_paper.pdf) | ECCV 2018 | [[code]](https://github.com/wbaek/tracking_via_colorization) |
| [Deep Exemplar-based Video Colorization]() | CVPR 2019 |  |



DeOldify: Colorizing and Restoring Old Images and Videos with Deep Learning

老照片上色, 人脸处理，港星老照片：“你我当年”




## 3.6 人脸相关

### 3.6.1 侧脸转正脸
为了提高人脸识别准确率，侧脸转正脸一直是很多人的研究方向：TP-GAN

### 3.6.2 年龄，表情
之前有应用就是生成年老的照片等，实际年龄判断一直不太准，是因为数据库在10-15岁年龄段的照片有断层。所以年龄预测一直准确度也不够高。随着stylegan等的成长，之前年龄这个部分的研究并没有太多可追随性。研究paper有 agengan，Face Aging With Conditional Generative Adversarial Networks 。作者使用在 IMDB 数据集上预训练模型而获得年龄的预测方法，然后研究者基于条件 GAN 修改生成图像的面部年龄。

论文：https://arxiv.org/pdf/1711.10352.pdf

http://k.sina.com.cn/article_6462307252_1812efbb4001009quq.html

表情也和年龄类似，可用模型有 GANimation 它将自己定义为“从一张图像中提取具有解剖学意义的面部动画”。

https://github.com/albertpumarola/GANimation

https://www.albertpumarola.com/research/GANimation/
http://tech.ifeng.com/a/20180729/45089543_0.shtml

Glow: Generative Flow with Invertible 1x1 Convolutions


基于流的生成模型在 2014 年已经被提出，但是一直被忽视。由 OpenAI 带来的 Glow 展示了流生成模型强大的图像生成能力。文章使用可逆 1 x 1 卷积在已有的流模型 NICE 和 RealNVP 基础上进行扩展，精确的潜变量推断在人脸属性上展示了惊艳的实验效果。

 https://www.paperweekly.site/papers/2101

 https://github.com/openai/glow

### 3.6.3 换脸
　
　　DAPAR的担忧并非空穴来风，如今的变脸技术已经达到威胁安全的地步。最先，可能是把特朗普和普京弄来表达政治观点；但后来，出现了比如DeepFake，令普通人也可以利用这样的技术制造虚假色情视频和假新闻。技术越来越先进，让AI安全也产生隐患。

　　1、Deepfake
https://github.com/deepfakes/faceswap
　　我们先看看最大名鼎鼎的Deepfake是何方神圣。

　　Deepfake即“deep learning”和“fake”的组合词，是一种基于深度学习的人物图像合成技术。它可以将任意的现有图像和视频组合并叠加到源图像和视频上。

　　Deepfake允许人们用简单的视频和开源代码制作虚假的色情视频、假新闻、恶意内容等。后来，deepfakes还推出一款名为Fake APP的桌面应用程序，允许用户轻松创建和分享换脸的视频，进一步把技术门槛降低到C端。

特朗普的脸被换到希拉里身上特朗普的脸被换到希拉里身上
　　由于其恶意使用引起大量批评，Deepfake已经被Reddit、Twitter等网站封杀。

　　2、Face2Face

　　Face2Face同样是一项引起巨大争议的“换脸”技术。它比Deepfake更早出现，由德国纽伦堡大学科学家Justus Thies的团队在CVPR 2016发布。这项技术可以非常逼真的将一个人的面部表情、说话时面部肌肉的变化、嘴型等完美地实时复制到另一个人脸上。它的效果如下：


　　Face2Face被认为是第一个能实时进行面部转换的模型，而且其准确率和真实度比以前的模型高得多。

　　3、HeadOn

　　HeadOn可以说是Face2Face的升级版，由原来Face2Face的团队创造。研究团队在Face2Face上所做的工作为HeadOn的大部分能力提供了框架，但Face2Face只能实现面部表情的转换，HeadOn增加了身体运动和头部运动的迁移。

　　也就是说，HeadOn不仅可以“变脸”，它还可以“变人”——根据输入人物的动作，实时地改变视频中人物的面部表情、眼球运动和身体动作，使得图像中的人看起来像是真的在说话和移动一样。

HeadOn技术的图示HeadOn技术的图示
　　研究人员在论文里将这个系统称为“首个人体肖像视频的实时的源到目标（source-to-target）重演方法，实现了躯干运动、头部运动、面部表情和视线注视的迁移”。

　　4、Deep Video Portraits

　　Deep Video Portraits 是斯坦福大学、慕尼黑技术大学等的研究人员提交给今年 8 月SIGGRAPH 大会的一篇论文，描述了一种经过改进的 “换脸” 技术，可以在视频中用一个人的脸再现另一人脸部的动作、面部表情和说话口型。


　　例如，将普通人的脸换成奥巴马的脸。Deep Video Portraits 可以通过一段目标人物的视频（在这里就是奥巴马），来学习构成脸部、眉毛、嘴角和背景等的要素以及它们的运动形式。 

  5、 ZAO


工具 ：

https://github.com/iperov/DeepFaceLab




## 3.8 字体合成


### 英文手写体 Handwriting Synthesis

### Generating Sequences with Recurrent Neural Networks
这个项目来自亚历克斯 · 格雷夫斯（Alex Graves）撰写的论文（Generating Sequences with Recurrent Neural Networks）《用 RNN 生成序列》，正如存储库的名称所示，您可以生成不同风格的手写，是其中手写体合成实验的实现，它可以生成不同风格的手写字迹。模型包括初始化和偏置两个部分，其中初始化控制样例的风格，偏置控制样例的整洁度。
作者在 GitHub 页面上呈现的样本的多样性真的很吸引人。他正在寻找贡献者来加强存储库，所以如果您有兴趣，可以研究去看看。

https://github.com/sherjilozair/char-rnn-tensorflow

https://github.com/karpathy/char-rnn

https://github.com/hardmaru/write-rnn-tensorflow

### 艺术字体 

### Multi-Content GAN for Few-Shot Font Style Transfer

cvpr2018，伯克利的BAIR实验室和adobe合作的论文

本文首次提出用端到端的方案来解决从少量相同风格的字体中合成其他艺术字体，例如 A-Z 26 个相同风格的艺术字母，已知其中 A-D 的艺术字母，生成剩余 E-Z 的艺术字母。

本文研究的问题看上去没啥亮点，但在实际应用中，很多设计师在设计海报或者电影标题的字体时，只会创作用到的字母，但想将风格迁移到其他设计上时，其他的一些没设计字母需要自己转化，造成了不必要的麻烦。

如何从少量（5 个左右）的任意类型的艺术字中泛化至全部 26 个字母是本文的难点。本文通过对传统 Condition GAN 做扩展，提出了 Stack GAN 的两段式架构，首先通过 Conditional GAN #1 根据已知的字体生成出所有 A-Z 的字体，之后通过 Conditional GAN #2 加上颜色和艺术点缀。

作者：黄亢，卡耐基梅隆大学硕士，研究方向为信息抽取和手写识别，现为波音公司数据科学家。

论文链接： https://arxiv.org/abs/1712.00516  https://www.paperweekly.site/papers/1781

GitHub 链接：https://github.com/azadis/MC-GAN

原文链接：http://bair.berkeley.edu/blog/2018/03/13/mcgan/




### 汉字

Github 用户 kaonashi-tyc 将 字体设计 的过程转化为一个“风格迁移”（style transfer）的问题，使用条件 GAN 自动将输入的汉字转化为另一种字体（风格）的汉字，效果相当不错。


动机

创造字体一直是一件难事，中文字体更难，毕竟汉字有26000多个，要完成一整套设计需要很长的时间。

而 Github 用户 kaonashi-tyc 想到的解决方案是只手工设计一部分字体，然后通过深度学习算法训练出剩下的字体，毕竟汉字也是各种「零件」组成的。

于是，作者将字体设计的过程转化为一个“风格迁移”（style transfer）的问题。他用两种不同字体作为训练数据，训练了一个神经网络，训练好的神经网络自动将输入的汉字转化为另一种字体（风格）的汉字。

作者使用风格迁移解决中文字体生成的问题，同时加上了条件生成对抗网络（GAN）的力量。

项目地址：https://github.com/kaonashi-tyc/zi2zi


----------------

## 3.9 视频生成

通常来说，视频有相对静止的背景和运动的前景组成。VideoGAN 使用一个两阶段的生成器，3D CNN 生成器生成运动前景，2D CNN 生成器生成静止的背景。Pose GAN 则使用 VAE 和 GAN 生成视频，首先，VAE 结合当前帧的姿态和过去的姿态特征预测下一帧的运动信息，然后 3D CNN 使用运动信息生成后续视频帧。Motion and Content GAN(MoCoGAN) 则提出在隐空间对运动部分和内容部分进行分离，使用 RNN 去建模运动部分。

### vid2vid

视频到视频的合成 ： Video-to-Video Synthesis
作者：Ting-Chun Wang, Ming-Yu Liu, Jun-Yan Zhu, Guilin Liu, Andrew Tao, Jan Kautz, Bryan Catanzaro

https://arxiv.org/abs/1808.06601

英伟达的研究人员引入了一种新的视频合成方法。该框架基于生成对抗学习框架。实验表明，所提出的vid2vid方法可以在不同的输入格式(包括分割掩码、草图和姿势)上合成高分辨率、逼真、时间相干的视频。它还可以预测下一帧，其结果远远优于基线模型。

https://github.com/NVIDIA/vid2vid



### 人人来跳舞

作者：Caroline Chan, Shiry Ginosar, Tinghui Zhou, Alexei A. Efros

https://youtu.be/PCBTZh41Ris

加州大学伯克利分校的研究人员提出了一种简单的方法，可以让业余舞蹈演员像专业舞蹈演员一样表演，从而生成视频。如果你想参加这个实验，你所需要做的就是录下你自己表演一些标准动作的几分钟的视频，然后拿起你想要重复的舞蹈的视频。

神经网络将完成主要工作：它将问题解决为具有时空平滑的每帧图像到图像的转换。利用位姿检测作为源和目标之间的中间表示,通过将每帧上的预测调整为前一时间步长的预测以获得时间平滑度并应用专门的GAN进行逼真的面部合成，该方法实现了非常惊人的结果。

https://github.com/nyoki-mtl/pytorch-EverybodyDanceNow

http://www.sohu.com/a/294911565_100024677

https://ceit.aut.ac.ir/~khalooei/tutorials/gan/


### Recycle-GAN

-------------------

### 3.10 3D
《Visual Object Networks: Image Generation with Disentangled 3D Representation》，描述了一种用GAN生成3D图片的方法。

这篇文章被近期在蒙特利尔举办的NeurIPS 2018

--------------------------

# Level 4: GANs Applications in Others

## 4.1 NLP 自然语言处理领域
相比于 GAN 在图像领域的应用，GAN 在文本，语音领域的应用要少很多。主要原因有两个：

GAN 在优化的时候使用 BP 算法，对于文本，语音这种离散数据，GAN 没法直接跳到目标值，只能根据梯度一步步靠近。
对于序列生成问题，每生成一个单词，我们就需要判断这个序列是否合理，可是 GAN 里面的判别器是没法做到的。除非我们针对每一个 step 都设置一个判别器，这显然不合理。为了解决上述问题，强化学习中的策略梯度下降（Policy gredient descent）被引入到 GAN 中的序列生成问题。

GAN在自然语言处理上的应用可以分为两类：生成文本、根据文本生成图像。其中，生成文本包括两种：根据隐向量（噪声）生成一段文本；对话生成。

 
BERT和自然语言处理（NLP）

### 4.2.1 对话生成
 Li J等2017年发表的Adversarial Learning for Neural Dialogue Generation[16]显示了GAN在对话生成领域的应用。实验效果如图11。可以看出，生成的对话具有一定的相关性，但是效果并不是很好，而且这只能做单轮对话。



如图11 Li J对话生成效果

文本到图像的翻译（text to image）

文本到图像的翻译指GAN的输入是一个描述图像内容的一句话，比如“一只有着粉色的胸和冠的小鸟”，那么所生成的图像内容要和这句话所描述的内容相匹配。



在ICML 2016会议上，Scott Reed等[17]人提出了基于CGAN的一种解决方案将文本编码作为generator的condition输入；对于discriminator，文本编码在特定层作为condition信息引入，以辅助判断输入图像是否满足文本描述。作者提出了两种基于GAN的算法，GAN-CLS和GAN-INT。



### 4.2.2 Text2image

Torch 版本：https://github.com/reedscot/icml2016

TensorFlow+Theano 版本：https://github.com/paarthneekhara/text-to-image


 Jun-Yan Zhu, Philipp Krähenbühl, Eli Shechtman and Alexei A. Efros. “Generative Visual Manipulation on the Natural Image Manifold”, ECCV 2016.

从 Text 生成 Image，比如从图片标题生成一个具体的图片。这个过程需要不仅要考虑生成的图片是否真实，还应该考虑生成的图片是否符合标题里的描述。比如要标题形容了一个黄色的鸟，那么就算生成的蓝色鸟再真实，也是不符合任务需求的。为了捕捉或者约束这种条件，他们提出了 matching-aware discriminator 的思想，让本来的 D 的目标函数中的两项，扩大到了三项：


StackGAN


Han Zhang, Tao Xu, Hongsheng Li, Shaoting Zhang, Xiaolei Huang, Xiaogang Wang, Dimitris Metaxas. “StackGAN: Text to Photo-realistic Image Synthesis with Stacked Generative Adversarial Networks”. arXiv preprint 2016.
第三篇这方面的工作[20]可以粗略认为是 LAPGAN[16] 和 matching-aware[18] 的结合。他们提出的 StackGAN[20] 做的事情从标题生成鸟类，但是生成的过程则是像 LAPGAN 一样层次化的，从而实现了 256X256 分辨率的图片生成过程。StackGAN 将图片生成分成两个阶段，阶段一去捕捉大体的轮廓和色调，阶段二加入一些细节上的限制从而实现精修。这个过程效果很好，甚至在某些数据集上以及可以做到以假乱真：

ObjGAN，可以通过关注文本描述中最相关的单词和预先生成的语义布局(semantic layout)来合成显著对象。

https://www.microsoft.com/en-us/research/uploads/prod/2019/06/1902.10740.pdf


其他：
textGAN MailGAN

MaskGAN


## 4.2 语音方向

### 4.2.1 音乐生成

RNN-GAN 使用 LSTM 作为生成器和判别器，直接生成整个音频序列。然而，正如上面提到的，音乐当做包括歌词和音符，对于这种离散数据生成问题直接使用 GAN 存在很多问题，特别是生成的数据缺乏局部一致性。

相比之下，SeqGAN 把生成器的输出作为一个智能体 (agent) 的策略，而判别器的输出作为奖励 (reward)，使用策略梯度下降来训练模型。ORGAN 则在 SeqGAN 的基础上，针对具体的目标设定了一个特定目标函数。

RNN-GAN 使用 LSTM 作为生成器和判别器，直接生成整个音频序列。然而，正如上面提到的，音乐当做包括歌词和音符，对于这种离散数据生成问题直接使用 GAN 存在很多问题，特别是生成的数据缺乏局部一致性。

相比之下，SeqGAN 把生成器的输出作为一个智能体 (agent) 的策略，而判别器的输出作为奖励 (reward)，使用策略梯度下降来训练模型。ORGAN 则在 SeqGAN 的基础上，针对具体的目标设定了一个特定目标函数。


wavenet

GANSynth是一种快速生成高保真音频的新方法
http://www.elecfans.com/d/877752.html



audio (currently no meta)   

https://github.com/CorentinJ/Real-Time-Voice-Cloning

https://github.com/andabi/deep-voice-conversion

https://github.com/r9y9/wavenet_vocoder

https://github.com/kuleshov/audio-super-res

https://github.com/francoisgermain/SpeechDenoisingWithDeepFeatureLosses

https://github.com/drethage/speech-denoising-wavenet

### 4.2.2 语言和语音


VAW-GAN(Variational autoencoding Wasserstein GAN) 结合 VAE 和 WGAN 实现了一个语音转换系统。编码器编码语音信号的内容，解码器则用于重建音色。由于 VAE 容易导致生成结果过于平滑，所以此处使用 WGAN 来生成更加清晰的语音信号。

Generative Adversarial Text to Image Synthesis	Reed & et al.	ICML 2016
Conditional Generative Adversarial Networks for Speech Enhancement and Noise-Robust Speaker Verification	Michelsanti & Tan	Interspeech 2017	
Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network	Ledig & et al.	CVPR 2017	
SalGAN: Visual Saliency Prediction with Generative Adversarial Networks	Pan & et al.	CVPR 2017	
SAGAN: Self-Attention Generative Adversarial Networks	Zhang & et al.	NIPS 2018	
Speaker Adaptation for High Fidelity WaveNet Vocoder with GAN	Tian & et al.	arXiv Nov 2018	
MTGAN: Speaker Verification through Multitasking Triplet Generative Adversarial Networks	Ding & et al.	arXiv Mar 2018	
Adversarial Learning and Augmentation for Speaker Recognition	Zhang & et al.	Speaker Odyssey 2018 / ISCA 2018	
Investigating Generative Adversarial Networks based Speech Dereverberation for Robust Speech Recognition	Wang & et al.	Interspeech 2018	
On Enhancing Speech Emotion Recognition using Generative Adversarial Networks	Sahu & et al.	Interspeech 2018	
Robust Speech Recognition Using Generative Adversarial Networks	Sriram & et al.	ICASSP 2018	
Adversarially Learned One-Class Classifier for Novelty Detection	Sabokrou & khalooei & et al.	CVPR 2018	
Generalizing to Unseen Domains via Adversarial Data Augmentation	Volpi & et al.	NeurIPS (NIPS) 2018	
Generative Adversarial Networks for Unpaired Voice Transformation on Impaired Speech	Chen & lee & et al.	Submitted on ICASSP 2019	
Generative Adversarial Speaker Embedding Networks for Domain Robust End-to-End Speaker Verification	Bhattacharya & et al.	Submitted on ICASSP 2019	


### 4.2.3 唇读

唇读（lipreading）是指根据说话人的嘴唇运动解码出文本的任务。传统的方法是将该问题分成两步解决：设计或学习视觉特征、以及预测。最近的深度唇读方法是可以端到端训练的（Wand et al., 2016; Chung & Zisserman, 2016a）。目前唇读的准确度已经超过了人类。

Google DeepMind 与牛津大学合作的一篇论文《Lip Reading Sentences in the Wild》介绍了他们的模型经过电视数据集的训练后，性能超越 BBC 的专业唇读者。

该数据集包含 10 万个音频、视频语句。音频模型：LSTM，视频模型：CNN + LSTM。这两个状态向量被馈送至最后的 LSTM，然后生成结果（字符）。

 人工合成奥巴马：嘴唇动作和音频的同步



华盛顿大学进行了一项研究，生成美国前总统奥巴马的嘴唇动作。选择奥巴马的原因在于网络上有他大量的视频（17 小时高清视频）。



------------------------------------
## Neural Style 风格迁移

Neural Style Transfer: A Review 

https://github.com/ycjing/Neural-Style-Transfer-Papers 

包含图像风格化综述论文对应论文、源码和预训练模型。 [中文](https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247489172&idx=1&sn=42f567fb57d2886da71a07dd16388022&chksm=96e9c914a19e40025bf88e89514d5c6f575ee94545bd5d854c01de2ca333d4738b433d37d1f5#rd)

![neuralstyle](https://github.com/weslynn/graphic-deep-neural-network/blob/master/pic/ganpic/overview.jpg)

### 风格迁移 Neural Style

它将待风格化图片和风格化样本图放入VGG中进行前向运算。其中待风格化图像提取relu4特征图，风格化样本图提取relu1,relu2,relu3,relu4,relu5的特征图。我们要把一个随机噪声初始化的图像变成目标风格化图像，将其放到VGG中计算得到特征图，然后分别计算内容损失和风格损失。

用这个训练好的 VGG 提取风格图片代表风格的高层语义信息，具体为，把风格图片作为 VGG 的输入，然后提取在风格语义选取层激活值的格拉姆矩阵（Gramian Matrix）。值得一提的是，格拉姆矩阵的数学意义使得其可以很好地捕捉激活值之间的相关性，所以能很好地表现图片的风格特征；
用 VGG 提取被风格化图片代表内容的高层语义信息，具体为，把该图片作为 VGG 的输入，然后提取内容语义提取层的激活值。这个方法很好地利用了卷积神经网络的性质，既捕捉了图片元素的结构信息，又对细节有一定的容错度；
随机初始化一张图片，然后用2，3介绍的方法提取其风格，内容特征，然后将它们分别与风格图片的风格特征，内容图片的内容特征相减，再按一定的权重相加，作为优化的目标函数。
保持 VGG 的权重不不变，直接对初始化的图⽚做梯度下降，直至目标函数降至一个比较小的值。
这个方法的风格化效果震惊了学术界，但它的缺点也是显而易见的，由于这种风格化方式本质上是一个利用梯度下降迭代优化的过程，所以尽管其效果不不错，但是风格化的速度较慢，处理一张图片在GPU上大概需要十几秒。deepart.io这个网站就是运用这个技术来进行图片纹理转换的。 




Ulyanov的Texture Networks: Feed-forward Synthesis of Textures and Stylized Images
李飞飞老师的Perceptual Losses for Real-Time Style Transfer and Super-Resolution。 

后面两篇都是将原来的求解全局最优解问题转换成用前向网络逼近最优解
原版的方法每次要将一幅内容图进行风格转换，就要进行不断的迭代，而后两篇的方法是先将其进行训练，训练得到前向生成网络，以后再来一张内容图，直接输入到生成网络中，即可得到具有预先训练的风格的内容图。 


### 多风格及任意风格转换

A Learned Representation for Artistic Style

condition instance normalization。

该论文在IN的基础上做了改进，加入了类似BN的γ和β缩放和平移因子，也就是风格特征的方差和均值，称为CIN（条件IN）。这样一来，网络只要在学习风格化的同时学习多种不同风格的γ和β，并保存起来。在要风格化某一风格的时候，只要将网络的所有γ和β（网络中所有有用到CIN层的地方）替换成对应风格的γ和β。 该方法还能实现同一内容图像风格化成多种风格的融合，这只要将多种风格特征的γ和β进行相应的线性融合便可，具体参考论文的实验部分。 该论文只能同时风格化有限的风格种类（论文中为32种），因为其需要保存所有风格种类的γ和β参数。



Diversified Texture Synthesis with Feed-forward Networks
是通过加入不同的风格图片ID，并加入嵌入层，来达到实现多种风格的目的。有点类似语音合成中的基于说话人ID搞成词向量作为网络的输入信息之一。

Fast Patch-based Style Transfer of Arbitrary Style
      这篇论文实现了图像的任意风格转换，不在局限于单个风格的训练。同时支持优化和前向网络的方法。生成时间：少于 10 秒。网络核心部分是一个style swap layer，即在这一层，对content的feature maps的每一块使用最接近的style feature 来替换。


style swap
论文分为2个部分，第一部分就是常规的迭代方式，第二个是将常规的改成一次前向的方法。


Arbitrary Style Transfer in Real-time with Adaptive Instance Normalization
https://arxiv.org/pdf/1703.06868.pdf
https://github.com/xunhuang1995/AdaIN-style
http://www.ctolib.com/AdaIN-style.html
支持使用一个前向网络来实现任意的风格转换，同时还保证的效率，能达到实时的效果。运行时间少于1s, 该论文在CIN的基础上做了一个改进，提出了AdaIN（自适应IN层）。顾名思义，就是自己根据风格图像调整缩放和平移参数，不在需要像CIN一样保存风格特征的均值和方差，而是在将风格图像经过卷积网络后计算出均值和方差。



### 语义合成图像：涂鸦拓展

### Neural Doodle 
纹理转换的另外一个非常有意思的应用是Neural Doodle，运用这个技术，我们可以让三岁的小孩子都轻易地像莫奈一样成为绘画大师。这个技术本质上其实就是先对一幅世界名画（比如皮埃尔-奥古斯特·雷诺阿的Bank of a River）做一个像素分割，得出它的语义图，让神经网络学习每个区域的风格。 
然后，我们只需要像小孩子一样在这个语义图上面涂鸦（比如，我们想要在图片的中间画一条河，在右上方画一棵树），神经网络就能根据语义图上的区域渲染它，最后得出一幅印象派的大作。

Champandard（2016） “Semantic Style Transfer and Turning Two-Bit Doodles into Fine Artworks”

基于 Chuan Li 和 Michael Wand（2016）在论文“Combining Markov Random Fields and Convolutional Neural Networks for Image Synthesis”中提出的 Neural Patches 算法。这篇文章中深入解释了这个项目的动机和灵感来源：https://nucl.ai/blog/neural-doodles/

doodle.py 脚本通过使用1个，2个，3个或4个图像作为输入来生成新的图像，输入的图像数量取决于你希望生成怎样的图像：原始风格及它的注释（annotation），以及带有注释（即你的涂鸦）的目标内容图像（可选）。该算法从带风格图像中提取 annotated patches，然后根据它们匹配的紧密程度用这些 annotated patches 渐进地改变目标图像的风格。

Github 地址：https://github.com/alexjc/neural-doodle

Faster
https://github.com/DmitryUlyanov/fast-neural-doodle
实时
https://github.com/DmitryUlyanov/online-neural-doodle





###  Controlling Perceptual Factors in Neural Style Transfer
颜色控制颜色控制
在以前的风格转换中，生成图的颜色都会最终变成style图的颜色，但是很多时候我们并不希望这样。其中一种方法是，将RGB转换成YIQ，只在Y上进行风格转换，因为I和Q通道主要是保存了颜色信息。 




###  Deep Photo Style Transfer
本文在 Neural Style algorithm [5] 的基础上进行改进，主要是在目标函数进行了修改，加了一项 Photorealism regularization，修改了一项损失函数引入 semantic segmentation 信息使其在转换风格时 preserve the image structure
贡献是将从输入图像到输出图像的变换约束在色彩空间的局部仿射变换中，将这个约束表示成一个完全可微的参数项。我们发现这种方法成功地抑制了图像扭曲，在各种各样的场景中生成了满意的真实图像风格变换，包括一天中时间变换，天气，季节和艺术编辑风格变换。


以前都是整幅图stransfer的，然后他们想只对一幅图的单个物体进行stransfer，比如下面这幅图是电视剧Son of Zorn的剧照，设定是一个卡通人物生活在真实世界。他们还说这种技术可能在增强现实起作用，比如Pokemon go. 




### Visual Attribute Transfer through Deep Image Analogy SIGGRAPH 2017 paper
https://github.com/msracver/Deep-Image-Analogy




--------------------------------

其他 

 SketchRNN：教机器画画



你可能看过谷歌的 Quick, Draw! 数据集，其目标是 20 秒内绘制不同物体的简笔画。谷歌收集该数据集的目的是教神经网络画画。

专业摄影作品



谷歌已经开发了另一个非常有意思的 GAN 应用，即摄影作品的选择和改进。开发者在专业摄影作品数据集上训练 GAN，其中生成器试图改进照片的表现力（如更好的拍摄参数和减少对滤镜的依赖等），判别器用于区分「改进」的照片和真实的作品。
训练后的算法会通过 Google Street View 搜索最佳构图，获得了一些专业级的和半专业级的作品评分。

：Creatism: A deep-learning photographer capable of creating professional work（https://arxiv.org/abs/1707.03491）。
Showcase：https://google.github.io/creatism/

https://www.sohu.com/a/157091073_473283



　DeepMasterPrint 万能指纹







---------------------------

其他：

Deep Learning: State of the Art*
(Breakthrough Developments in 2017 & 2018)

• AdaNet: AutoML with Ensembles
• AutoAugment: Deep RL Data Augmentation
• Training Deep Networks with Synthetic Data
• Segmentation Annotation with Polygon-RNN++
• DAWNBench: Training Fast and Cheap
• Video-to-Video Synthesis
• Semantic Segmentation
• AlphaZero & OpenAI Five
• Deep Learning Frameworks


AdaNet：可集成学习的AutoML

AutoAugment：用强化学习做数据增强

用合成数据训练深度神经网络

用Polygon-RNN++做图像分割自动标注

DAWNBench：寻找快速便宜的训练方法



语义分割

AlphaZero和OpenAI Five

深度学习框架
https://github.com/lexfridman/mit-deep-learning



SAC-X






-----------------------------------------------------------------------------

https://github.com/wiseodd/generative-models


PPGAN - Anh Nguyen, arXiv:1612.00005v1



SeqGAN - Lantao Yu, arxiv: 1609.05473

EBGAN - Junbo Zhao, arXiv:1609.03126v2

VAEGAN - Anders Boesen Lindbo Larsen, arxiv: 1512.09300

......

特定任务中提出来的模型，如GAN-CLS、GAN-INT、SRGAN、iGAN、IAN 等


IAN

Theano 版本：https://github.com/ajbrock/Neural-Photo-Editor


TensorFlow 版本：https://github.com/yenchenlin/pix2pix-tensorflow

GAN for Neural dialogue generation

Torch 版本：https://github.com/jiweil/Neural-Dialogue-Generation


GAN for Imitation Learning

Theano 版本：https://github.com/openai/imitation

SeqGAN

TensorFlow 版本：https://github.com/LantaoYu/SeqGAN


Qi G J. Loss-Sensitive Generative Adversarial Networks onLipschitz Densities[J]. arXiv preprint arXiv:1701.06264, 2017.

Li J, Monroe W, Shi T, et al. Adversarial Learning for NeuralDialogue Generation[J]. arXiv preprint arXiv:1701.06547, 2017.

Sønderby C K, Caballero J, Theis L, et al. Amortised MAPInference for Image Super-resolution[J]. arXiv preprint arXiv:1610.04490, 2016.

Ravanbakhsh S, Lanusse F, Mandelbaum R, et al. Enabling DarkEnergy Science with Deep Generative Models of Galaxy Images[J]. arXiv preprintarXiv:1609.05796, 2016.

Ho J, Ermon S. Generative adversarial imitationlearning[C]//Advances in Neural Information Processing Systems. 2016:4565-4573.

Zhu J Y, Krähenbühl P, Shechtman E, et al. Generative visualmanipulation on the natural image manifold[C]//European Conference on ComputerVision. Springer International Publishing, 2016: 597-613.

Isola P, Zhu J Y, Zhou T, et al. Image-to-image translationwith conditional adversarial networks[J]. arXiv preprint arXiv:1611.07004,2016.

Shrivastava A, Pfister T, Tuzel O, et al. Learning fromSimulated and Unsupervised Images through Adversarial Training[J]. arXivpreprint arXiv:1612.07828, 2016.

Ledig C, Theis L, Huszár F, et al. Photo-realistic singleimage super-resolution using a generative adversarial network[J]. arXivpreprint arXiv:1609.04802, 2016.

Nguyen A, Yosinski J, Bengio Y, et al. Plug & playgenerative networks: Conditional iterative generation of images in latentspace[J]. arXiv preprint arXiv:1612.00005, 2016.

Yu L, Zhang W, Wang J, et al. Seqgan: sequence generativeadversarial nets with policy gradient[J]. arXiv preprint arXiv:1609.05473,2016.

Lotter W, Kreiman G, Cox D. Unsupervised learning of visualstructure using predictive generative networks[J]. arXiv preprintarXiv:1511.06380, 2015.

Reed S, Akata Z, Yan X, et al. Generative adversarial textto image synthesis[C]//Proceedings of The 33rd International Conference onMachine Learning. 2016, 3.

Brock A, Lim T, Ritchie J M, et al. Neural photo editingwith introspective adversarial networks[J]. arXiv preprint arXiv:1609.07093,2016.

Pfau D, Vinyals O. Connecting generative adversarialnetworks and actor-critic methods[J]. arXiv preprint arXiv:1610.01945, 2016.



Neural Dialogue Generation
https://github.com/jiweil/Neural-Dialogue-Generation








