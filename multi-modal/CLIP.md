# CLIP Brief Introduction
## Abstract
[[paper]](https://arxiv.org/abs/2103.00020) [[官方code]](https://github.com/openai/CLIP) [[训练code]](https://github.com/mlfoundations/open_clip)<br/>
&emsp;&emsp;本文使用文图之间对比学习的方式（CLIP，Contrastive Language-Image Pre-training），在4亿网络上爬取的文图对上进行预训练，并通过实验证明了1、CLIP模型在各种CV的下游任务中都具有zero-shot transformer能力。2、通过linear prob的方式证明CLIP的representation learn能力3、以及对distribution shift的鲁棒性
## Structure and training setting
![sturct](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/clip_structure.png?raw=true)<br/>
&emsp;&emsp;CLIP从结构和实现上来看都非常简单，包括一个Text Encoder和Image Encoder。训练过程也很简单，伪代码如下：<br/>