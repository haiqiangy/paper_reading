# UNITER Brief Introduction
## Abstract
[[paper]](https://arxiv.org/abs/1909.11740)<br/>
[[code]](https://github.com/ChenRocks/UNITER)<br/>
&emsp;&emsp;本文提出了一种通用图文表征预训练模型（UNITER）。设计了四种预训练任务：Masked Language Modeling
(MLM), Masked Region Modeling (MRM, with three variants), ImageText Matching (ITM), and Word-Region Alignment (WRA)。其中，WRA还包括三个变种：(i) Masked Region
Classification (MRC); (ii) Masked Region Feature Regression (MRFR);(iii)
Masked Region Classification with KL-divergence (MRC-kl)和之前工作同时随机mask掉图文token进行学习的方式不同，UNITER是保留完整图/文的同时mask掉另一种模态进行学习(这个操作称为conditional masking)。ITM任务用于全局的图文匹配。WRA任务则用于图片区域和文字这种更细粒度的匹配。UNITER相比于之前的工作提出了conditional masking、WRA都能有效的作用提升预训练模型在下游任务上的表现。
## Architecture and Pipeline
![struct](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/uniter_structure.png?raw=true)<br/>
&emsp;&emsp;UNITER的模型结构由三部分组成，Image Embedder、Text Embedder、Transformer。下面简单说明一下预训练的流程：<br/>
1. Image Embeding：给定一个图像文本对,对图片用Faster R-CNN抽取pooled ROI features。再加上位置编码[x1; y1; x2; y2; w; h; w ∗ h] (normalized top/left/bottom/right coordinates, width,
height, and area.)。最后通过两个FC层+LN映射到emdeding所需的维度。
2. Text embeding：首先对文本通过WordPieces进行分词，转换成word embeding，加上position embeding所谓transformer的输入。
3. 通过transformer学习cross-modality embeding。这个步骤对于不同的预训练任务有不同的做法。对于
3. 通过transformer学习cross-modality embeding，在训练过程中对于每个batch会随机采用其中一种任务进行学习。这个步骤对于不同的预训练任务有不同的做法。对于MRM和MLM任务会随机mask掉部分token，文本的token用[MASK] token代替，图像token用0值填充。对于ITM任务会同事采样正样本对和负样本对进行学习。对于WRA任务，计算从image embeding和text embeding互相转换的cost，作者认为这样模型可以学习到细粒度的匹配。
## Pre-training Tasks
### Masked Language Modeling (MLM)
&emsp;&emsp;参照BERT采用15%的mask ratio,其中，10%采用随机的词。10%不变，80%替换为[MASK]
### Image-Text Matching (ITM)
&emsp;&emsp;加入一个[CLS] token用于学习是否图文对是否匹配
### Word-Region Alignment (WRA)
&emsp;&emsp;采用Optimal Transport (OT)
### Masked Region Modeling (MRM) 
&emsp;&emsp;采用15%的mask ratio，mask掉的部分用0代替。这个任务有三种变体：MRFR去拟合pooled ROI features、MRC去学习这个部分的标签类别，将Faster R-CNN预测的最大概率作为真值。MRC-kl去拟合Faster R-CNN输出logit之间的kl散度。
## Experiment

