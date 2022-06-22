#  Mask2Former Brief Introduction
## Contribution
    提出了一个统一的分割架构，能够在语义分割、实例分割、全景分割上取得sota的结果，在coco数据集上，全景分割57.8PQ、实例分割50.1AP、语义分割在ADE20K达57.7miou。
![结果对比](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/mask2former_result.png?raw=true)
## Structure
![mask2former结构](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/mask2former_structure.png?raw=true)
和mask-rcnn一样，mask2former采用mask classification的方式来进行分割。mask-rcnn和mask2former在如何生产二值mask上的做法不同。mask-rcnn是通过bounding boxes的方式来表示，使得mask-rcnn无法进行semantic segmentaion任务。而Mask2Former参考了Detr的做法，将这些二值mask用一组C维的特征向量来表示(object query)，这样就可以用transformer decoder，通过一组固定的query来进行训练。
1. Backbone(resnet、swin)
    从图片中抽取低分辨率的特征
2. Pixel Decoder
    从低分辨率的特征中逐步上采样得到高分辨率的特征
3. Transformer Decoder
    通过图像特征产生对应的目标queries
## Transformer decoder with masked attention
1. 和一般transformer decoder不同，mask2former在transformer deocder中加入了masked attention，将cross attention限制在每个query关注的前景区域中
2. 采用了多尺度策略，将pixel decoder中的特征金字塔依次输入到transformer decoder中
### masked attention
- 目的
    上下文特征被证明对分割任务有重要的作用，但是在transformer based的方法中用cross attention存在收敛慢的问题。本文使用mask attention来解决这个问题。
- cross attention函数
![cross attention](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/cross_attention_function.png?raw=true)
- masked cross attention函数
![masked cross attention](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/masked_cross_attention_1.png?raw=true)
![masked cross attention2](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/masked_cross_attention_2.png?raw=true)
    ps:mask是前一层transformer decoder block输出的feature做二值化之后的结果
### efficient multi-scale strategy
    具体的讲就是将pixel decoder中尺度为原图1/32、1/16、1/8的特征依次作为对应transformer decoder block的K、V的输入。参照deformable detr的做法，对每个输入都加上了sinusoidal positional embedding和learnable scale-level embedding。按分辨率从低到高的循序依次输入，并循环L次。
### Optimization improvements
1. 更换了self-attention和cross-attention的顺序。self-attention->cross-attention变成cross-attention->self-attention。让计算更加有效。
2. 让query变成可学习的参数。让query进行监督学习可以起到类似region proposal的作用。通过实验可以证明可学习的query可以产生mask proposal。
3. 去掉了transformer deocder中的dropout操作。通过实验发现这个操作会降低精度。
### PointRend
    参考PointRend的操作。K取12544时，可以节省3倍显存。
1. 在bipartite matching的时候，通过均匀采样的方式在K个点集上计算match loss
2. 在计算最终损失时按importance sampling来采样K个点集。
## Ablation studies

