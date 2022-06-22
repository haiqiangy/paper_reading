#  Mask2Former Brief Introduction
## Contribution
    提出了一个统一的分割架构，能够在语义分割、实例分割、全景分割上取得sota的结果，在coco数据集上，全景分割57.8PQ、实例分割50.1AP、语义分割在ADE20K达57.7miou。
![结果对比](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/mask2former_result.png?raw=true)
## Structure
![mask2former结构](https://github.com/haiqiangy/paper_reading/blob/main/segmentation/figs/mask2former_structure.png?raw=true)
和mask-rcnn一样，mask2former采用mask classification的方式来进行分割。mask-rcnn和mask2former在如何生产二值mask上的做法不同。mask-rcnn是通过bounding boxes的方式来表示，使得mask-rcnn无法进行semantic segmentaion任务。而Mask2Former参考了Detr的做法，将这些二值mask用一组C维的特征向量来表示(object query)，这样就可以用transformer decoder，通过一组固定的query来进行训练。
1. Backbone
    从图片中抽取低分辨率的特征
2. Pixel Decoder
    从低分辨率的特征中逐步上采样得到高分辨率的特征
3. Transformer Decoder
    通过图像特征产生对应的目标queries