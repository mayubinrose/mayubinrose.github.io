---
layout: title
title: Linux
date: 2021-09-17 13:45:57
tags: Linux
top: 97
categories: 开发岗
---
## Linux 命令
<!--more-->
```python
# 添加用户赋予权限
sudo useradd myb -m
sudo passwd myb
sudo adduser myb sudo 
su myb
sudo userdel -r myb

# 安装anoconda
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.3.1-Linux-x86_64.sh
bash Anaconda3-5.3.1-Linux-x86_64.sh

# 添加仓库
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
# 创建虚拟环境
conda create -n 虚拟环境名 python=3.8
# 激活虚拟环境
source activate your_env_nameco(虚拟环境名称)
conda activate your_env_name
# 删除虚拟环境
conda remove -n your_env_name(虚拟环境名称) --all
# 查看有哪些虚拟环境
conda env list
# 退回到base
conda deactivate
# 台式
conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=11.1  
conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=11.1  -c nvidia
# 新服务器
conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=11.1 -c nvidia

# 镜像下载cv2
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple opencv-python
python -m pip install --upgrade pip -i https://pypi.douban.com/simple
# 查看进程
ps -aux | grep xxx
# 后台运行
nohup python ... > ....log 2>&1 &
# 无提示的强制递归删除
rm -rf 
# 是将标准错误输出重定向到名为"1"的文件里去了
2>&1 &
# 根据文件类型  -d 目录 -f 文件 -l 符号链接 -s 套接字 -b 块设备 -c 字符设备 -p 管道文件
# wc会打印每个文件或来自标准输入的换行符 n   、单词和字节计数
# -l 打印换行符的总数 也就是由find命令输出的绝对文件路径个数
find ..... -type  f|wc -l
#将一个文件夹下所有内容复制到另一个文件夹下面：
cp -r /home/packageA/* /home/cp/packageB/
# 后台运行
CUDA_CACHE = /tmp
# "0,1" 也可以带引号
CUDA_VISIBLE_DEVICES = 0,1
nohup python -u "/home/ding20/GenerateSample/GenerateSample/generate_samples3.py" > "/home/ding20/GenerateSample/GenerateSample/generate_samples3.log" 2>&1 &
# 赋予文件权限
chmod 777 /xx/xxx
# 一个文件夹移动到另一个
mv wenjianjia wenjianjia 
# 一个服务器移动到另一个
scp -r 文件 hit@xxx.xxxx.xxx.xx: 路径
df -hl
du --max-depth=1 -h
pip install 包名 -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
# 删除所有的channels 只有默认
conda config --remove-key channels
# linux w3m操作网页
w3m www.baidu.com
# Tensorboard
(myb) E:\PyFile\Graduation\Bili>tensorboard --logdir=log --port=6007
```
## 链接远程jupyter notebook
```python
### 服务器
pip install jupyter
conda install nb_conda
jupyter notebook --generate-config
ipython
from notebook.auth import passwd
passwd()
c.NotebookApp.notebook_dir = '/mnt/Data/myb/Grad/Jupyter/'
c.NotebookApp.allow_remote_access = True    # 可能没有需要自己添加
c.NotebookApp.ip = '10.249.144.163'
c.NotebookApp.open_browser = False
c.NotebookApp.iopub_data_rate_limit = 10000000
c.NotebookApp.password = 'argon2:$argon2id$v=19$m=10240,t=10,p=8$E3mf93bOAL0wCuTOgHUS2A$hx5AvtlqGgfyBsA5v04dfw' # 填写刚才在第二步Out[2]生成的密钥
c.NotebookApp.port = 2020   # 自己设置一个就行
## 本地 -N不运行远程指令 配合-f一块使用 -f后台运行ssh隧道 -L本地转发
ssh -N -f -L localhost:2020:10.249.144.163:2020 myb@10.249.144.163
# 任务管理器删除后台隧道
# 后台运行jupyter
nohup jupyter notebook > /mnt/Data/myb/Grad/Jupyter/log/jupyter.log 2>&1 &
# 根据端口找到进程
netstat -anp |grep 端口
```
## 解压缩文件
```python
# 解压tar.gz文件
tar -zxvf wenjian -C jieyalujing
# 解压tar.bz2文件
tar -jxvf ×××.tar.bz2 -C
# 解压gz文件
gzip -d wenjian.gz
zcat  pythontab.gz > /home/test/aa/pythontab.py
# tar -c：建立压缩档案 -x：解压 -t：查看内容 -r：向压缩归档文件末尾追加文件 -u：更新原压缩包中的文件（这五个是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但是只能用一个）-z:有gzip属性的 -j：有bz2属性的 -Z：有compress属性的 -v：显示所有过程 -O：将文件解开到标准输出 -f：使用档案名字，最后一个参数，后面只可以接档案名
```

