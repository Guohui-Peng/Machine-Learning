# Windows 11上安装Pytorch  

## 1. 安装Docker  

建议先在windows 11上启用WSL2并安装docker，选择支持WSL集成。可参考[官方文档](https://docs.docker.com/desktop/windows/wsl/)  

## 2. 安装Nvidia官方驱动  

建议安装最新版本的Nvidia驱动，官方网站<https://www.nvidia.cn/Download/index.aspx?lang=cn>。使用docker方式无需在主机上安装CUDA等其它软件，只需要安装显卡驱动即可。  

## 3. 安装Nvidia的pytorch docker镜像  

Windows 11下打开“终端”，如未安装可以在微软商店中免费安装。然后运行下面的命令就可以创建需要的pytorch docker。

```linux
docker run --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -it --name docker-name -v E:\git-home\wsl\xxx:/workspace nvcr.io/nvidia/pytorch:22.09-py3
```  

22.09-py3中的22.09可以更换为需要的版本号，官方版本查询<https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch/tags>  

## 4. 使用VS Code连接docker  

VS Code可以通过远程资源连接功能，连接WSL以及下面的docker。连接后可以在docker镜像上进行编程，建议将工作目录映射到主机目录，并设置GIT或其它版本控制软件进行代码同步。Docker的镜像更新需删除原来的镜像重新创建，不做好备份可能丢失数据！！！  
