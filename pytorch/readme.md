# Pytorch学习记录  

## 安装  

建议使用docker进行安装，方便升级以及系统隔离。[安装说明](../pytorch/install.md)  

## 常用功能记录  

Tensorboard是TF全家桶之一，能很方便的画出各种指标的曲线以及显示一些其它信息。Pytorch也提供了库可以将数据记录为Tensorboard支持的格式。Pytorch下tensorboard记录[参考代码](../pytorch/tensorboard.ipynb)  

## 采坑记  

学习都是在摸索中过河，难免踩坑。在此记录一些自己学习中遇到的坑。  

1. [GPU上的性能问题](./performance.md)  
1. `os.path.join`使用时，注意任何带“/”开头的参数会导致路径从此开始从根目录计算路径，前面的所有路径都会被忽略。
如：`os.path.join(log_path, '/train')`会返回路径`/train`，并不会返回预想中的`log_path + /train`路径！  
