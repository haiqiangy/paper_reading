# M3AE Brief Introduction
## Abstract
&emsp;&emsp;本文提出了Multimodal Masked Autoencoder (M3AE)，通过统一的一个编码器以masked token prediction方式来学习图像和文本的表征。1.在这种范式下，作者发现对文本采用（50%-90%）的掩码率能取得具有更好表征效果的模型。这点和Bert的15%有所不同。2.M3AE在扩展模型大小和数据集大小的时候，能取得有效的精度提升。3.由于使用了统一的解码器，在进行预训练的时候即能够利用image-text对数据，也能够利用不成对的数据进行训练。
## Architecture an Pipeline
&emsp;&emsp;