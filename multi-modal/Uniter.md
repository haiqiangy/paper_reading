# UNITER Brief Introduction
## Abstract
[[paper]](https://arxiv.org/abs/1909.11740)<br/>
[[code]](https://github.com/ChenRocks/UNITER)<br/>
&emsp;&emsp;本文提出了一种通用图文表征预训练模型（UNITER）。设计了四种预训练任务：Masked Language Modeling
(MLM), Masked Region Modeling (MRM, with three variants), ImageText Matching (ITM), and Word-Region Alignment (WRA)。和之前工作同时随机mask掉图文token进行学习的方式不同，UNITER是保留完整图/文的同时mask掉另一种模态进行学习。ITM任务用于全局的图文匹配。WRA任务则用于图片区域和文字这种更细粒度的匹配。
## Architecture and Pipeline
