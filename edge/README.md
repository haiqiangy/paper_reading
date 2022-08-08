# edge model
## 2022
- [MobileViT] MobileViT: Light-weight, General-purpose, and Mobile-friendly Vision Transformer [[paper]](https://arxiv.org/pdf/2110.02178.pdf) [[code]](https://github.com/apple/ml-cvnets)
</br>
 Transformer相比于CNN结构没有空间归纳偏置，能够取得更sota的精度，但是需要更大的参数量和训练数据才能取得有效的结果，因此在端侧模型上CNN还是占据了主导地位。MobileViT由CNN和Transformer结合，CNN和patch based transformer block交替组成。虽然attention module只在patch上进行操作，但是由于经过CNN，感受野有全图的大小。另外由于只在patch上进行attention操作，因此输出特征reshape回H*W后仍然保留了空间位置信息，有空间归纳偏置的特性，因此能在较小的模型量上取得有效的结果。