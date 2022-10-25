# Pytorch使用中遇到的一些性能问题的坑  

使用GPU训练模型时出现性能达不到90%以上，当并不是网络太简单或数据太少时要分析其不能跑满的原因。网上一般能找到的都是增加DataLoader的缓存、使用内存缓存数据等提升pytorch的效率。但经实际使用发现一些坑。  

## Nvidia Torch Docker包的坑  

由于相信Nvidia在CUDA加速上应该更加权威，使用Nvidia提供的Pytorch Docker包，版本有`nvcr.io/nvidia/pytorch:22.08-py3`、`nvcr.io/nvidia/pytorch:22.09-py3`（注：其他版本未做测试），都在将GPU的数据print时有CUDA使用率大幅度下降的问题。  

注：使用的3060显卡。测试发现无法开启TF32加速。torch.backends.cuda.matmul.allow_tf32, torch.backends.cudnn.allow_tf32参数无论是True还是False训练时的性能相同。  

### GPU中的评价指标导致性能下降  

在训练过程中使用item()、detach()命令时会导致GPU同步，破坏了其异步处理机制，从而导致性能下降严重。经测试可以导致GPU的CUDA使用率从100%降到70%~90%。  
![alt 性能图2](../images/performance2.jpg)  

当在训练时计算了loss以及accuracy等指标时，如果使用item()会导致GPU同步处理，GPU的处理效率下降10%~30% 。可在训练时记录其tensor，在需要打印时再使用item()。  

因此，如果一定要打印指标进行查看，建议只在每个epoch结束时打印一次。这样可以有效的降低GPU同步次数，尽可能让GPU异步快速处理。  
![alt 性能图1](../images/performance1.jpg)  

### 解决方案  

改用Docker Hub上的pytorch/pytorch包，如：pytorch/pytorch:1.12.1-cuda11.3-cudnn8-runtime。  
测试相同训练代码开TF32后性能翻倍。  

如下图所示，相同的代码Cuda利用率基本上可以跑满。  
![alt 性能图3](../images/performance3.jpg)  
