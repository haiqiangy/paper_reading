# M3AE Brief Introduction
## Abstract
&emsp;&emsp;本文提出了Multimodal Masked Autoencoder (M3AE)，通过统一的一个编码器以masked token prediction方式来学习图像和文本的表征。1.在这种范式下，作者发现对文本采用（50%-90%）的掩码率能取得具有更好表征效果的模型。这点和Bert的15%有所不同。2.M3AE在扩展模型大小和数据集大小的时候，能取得有效的精度提升。3.由于使用了统一的解码器，在进行预训练的时候即能够利用image-text对数据，也能够利用不成对的数据进行训练。
## Architecture an Pipeline
![architecture](https://raw.githubusercontent.com/haiqiangy/paper_reading/main/multi-modal/figs/m3ae-architecture.png)<br/>
&emsp;&emsp;M3AE做预训练的模型结构和MAE基本一致，包括一个编码器将文本和图像映射到一个共同的表征空间和一个轻量级的解码器用于重建文本和图像中被mask掉的部分。简单说明一下M3AE预训练时候的流程。<br/>
1. 分别将文本和图像做tokenizer。这部分图像和text的做法分别是参照vit和bert。然后直接将这两部分concat成同一个序列的tokens。
2. 对tokens按均匀采样的方式mask掉一个较高的比例。
3. 分别对text部分的token加上1D的position emdeding，对image部分加上2的position embeding用于区分token的位置信息。除此之外，还加上了可学习的modality emdeding用于区分token的模态类型。除此之外还在token序列的头部加上了一个cls token。
4. 仅将未被mask掉的token部分输入到encoder中学习表征。
5. 将encoder后的embeing加上可学习权重共享的mask token组成一个完整的序列通过decoder去重建mask掉的图像和文本部分。（输入decoder前token加上了position embeding和modality emdeding让mask token可以区分位置和模态类型）
