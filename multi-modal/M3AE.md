# M3AE Brief Introduction
## Abstract
[[paper]](https://arxiv.org/abs/2205.14204)<br/>
&emsp;&emsp;本文提出了Multimodal Masked Autoencoder (M3AE)，多模态版的MAE，通过统一的一个编码器以masked token prediction方式来学习图像和文本的统一表征。1.在这种范式下，作者发现对文本采用（50%-90%）的掩码率能取得具有更好表征效果的模型。这点和Bert的15%有所不同。2.M3AE有和MAE类似的性质在扩展模型大小和训练时间的时候，能取得有效的精度提升。3.由于使用了统一的解码器，在进行预训练的时候即能够利用image-text对数据，也能够利用不成对的数据进行训练。
## Architecture and Pipeline
![architecture](https://raw.githubusercontent.com/haiqiangy/paper_reading/main/multi-modal/figs/m3ae-architecture.png)<br/>
&emsp;&emsp;M3AE做预训练的模型结构和MAE基本一致，包括一个编码器将文本和图像映射到一个共同的表征空间和一个轻量级的解码器用于重建文本和图像中被mask掉的部分。简单说明一下M3AE预训练时候的流程。<br/>
1. 分别将文本和图像做tokenizer。这部分图像和text的做法分别是参照vit和bert。然后直接将这两部分concat成同一个序列的tokens。
2. 对tokens按均匀采样的方式mask掉一个较高的比例。
3. 分别对text部分的token加上1D的position emdeding，对image部分加上2的position embeding用于区分token的位置信息。除此之外，还加上了可学习的modality emdeding用于区分token的模态类型。除此之外还在token序列的头部加上了一个cls token。
4. 仅将未被mask掉的token部分输入到encoder中学习表征。
5. 将encoder后的embeing加上可学习权重共享的mask token组成一个完整的序列通过decoder去重建mask掉的图像和文本部分。（输入decoder前token加上了position embeding和modality emdeding让mask token可以区分位置和模态类型）
## Experiments
### M3AE scale well with model size and training time
![experiment1](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment1.png?raw=true)<br/>
在扩展模型大小和训练时间的时候M3AE方法的精度都能获得有效的提升。
### M3AE在不同文图对比例下的精度
![experiment2](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment2.png?raw=true)</br>
M3AE方法可以在文图对和只有图片的两种数据上共同进行训练，相比于CLIP这种对比学习方法限制更少，而相比于MAE这种单模态的方法，能够有效的利用text中的监督信息取得更好的效果
### Out-of-distribution detection
![experiment3](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment3.png?raw=true)
### text mask ratio
![experiment4](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment4.png?raw=true)<br/>
和bert中mask掉15%的情况不同，M3AE这种多模态的预训练范式，在text的mask掉75%的时候能取得较好的精度。
## 可视化解释
![vis1](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment5.png?raw=true)<br/>
![vis2](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment6.png?raw=true)<br/>
在给定文本token和图像patch的时候，M3AE都能在图像和文本上给出较合理的attention解释。
![vis3](https://github.com/haiqiangy/paper_reading/blob/main/multi-modal/figs/m3ae-experiment7.png?raw=true)<br/>
通过 t-SNE方式分别对MAE和M3AE在imagenet10个类别上的表征进行可视化。证明M3AE具有更强的区分能力
