---
title: 通用大语言模型介绍、本地部署及微调
date: 2023-04-24 11:25:00
author: okeeper
top: false
toc: true
cover: true
mathjax: false
summary: 通用大语言模型介绍、本地部署及微调
categories: 人工智能
tags:
  - AI
  - LLama
  - ChatGLM
---


## wget 后台下载

```
wget -b url

# 查看wget log
cat wget-log
tail -f wget-log

# wget指定文件名称
wget url -o filename
```



## sftp文件上传

```
sftp root@8.134.128.130 <<EOF
put ./source/openai-demo*.jar /opt/openai-demo/
bye
EOF

#查看本地的文件列表
lls
lcd 

ls
cd
```





## 查看文件摘要

```
sha256sum example.txt
```



## Git lfs使用

```
安装 homebrew
brew install git-lfs
git lfs install

下载安装 windows installer
运行 windows installer
git lfs install


sudo yum install git-lfs
sudo apt-get install git-lfs

# 切换到lfs
git lfs install

# 列出git lfs管理的大文件
git lfs list-files
```



## Conda使用

```
conda create -n env_name
conda activate env_name
conda info --envs

# 安装软件
conda install xxx

```



## 查看显卡运行情况命令

```
watch -n 0.5 nvidia-smi
```





## 安装环境依赖

```
pip install -r requirement.txt
```



## Clash服务器端代理：

使用Xflash作为梯子

https://i.jakeyu.top/2021/11/27/centos-%E4%BD%BF%E7%94%A8-Clash-%E6%A2%AF%E5%AD%90/

https://www.jianshu.com/p/1702a352797d



```
nohup ~/clash > /dev/null 2>&1 &
nohup ~/clash > ~/clash_out.log &


vim /etc/profile
source /etc/profile

export ALL_PROXY=socks5://127.0.0.1:7891
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

#测试
curl www.google.com
```



## 报错集锦





## 大语言模型大爆发

开源大语言模型(LLM)汇总： http://www.dtmao.cc/NodeJs/75351.html

 开源ChatGPT替代模型项目整理： https://zhuanlan.zhihu.com/p/618790279



![img](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/1deb2c972163d97bf177aa20222ac42a.png)





## LLMA（大语言模型，Large Language Model, LLM）

大语言模型llma-13b:https://huggingface.co/decapoda-research/llama-13b-hf/tree/main

LMFlow大语言模型微调：https://github.com/OptimalScale/LMFlow/blob/main/readme/README_zh-hans.md

LLMA-Alpaca中文版：https://github.com/ymcui/Chinese-LLaMA-Alpaca

LLMA-Alpaca中文版与原始llma模型合并：https://github.com/ymcui/Chinese-LLaMA-Alpaca/wiki/%E6%A8%A1%E5%9E%8B%E5%90%88%E5%B9%B6%E4%B8%8E%E8%BD%AC%E6%8D%A2

LLMA-Alpaca中文版模型训练过程：https://github.com/ymcui/Chinese-LLaMA-Alpaca/wiki/%E8%AE%AD%E7%BB%83%E7%BB%86%E8%8A%82

大语言模型的下载和安装：https://ivonblog.com/posts/dalai-llama-installation/



## ChatGLM

https://github.com/THUDM/ChatGLM-6B

官方chatglm微调：https://github.com/THUDM/ChatGLM-6B/blob/main/ptuning/README.md

第三方微调：https://github.com/ssbuild/chatglm_finetuning

PTuning与LoRA微调方式：https://github.com/liucongg/ChatGLM-Finetuning

制作数据集方案：https://github.com/hikariming/alpaca_chinese_dataset/blob/main/%E5%BE%AE%E8%B0%83%E4%BD%BF%E7%94%A8%E8%87%AA%E5%B7%B1%E6%95%B0%E6%8D%AE%E9%9B%86%E6%88%90%E5%8A%9F%E6%96%B9%E6%A1%88.ipynb

用Blog和聊天记录微调自己的ChatGLM模型：https://github.com/wdkwdkwdk/CLONE_DK

使用langchat进行训练：https://www.heywhale.com/mw/project/643977aa446c45f4592a1e59

港科大开源的训练微调脚手架：https://github.com/OptimalScale/LMFlow

## 微调

ChatGPT等大模型高效调参大法——PEFT：https://zhuanlan.zhihu.com/p/613863520

（1）对于动则百亿级别的参数,如何更高效,低资源的微调大模型呢

（2）当样本量很小的时候，如何微调大模型能得到较好的效果呢

- LORA ：[LORA](https://link.zhihu.com/?target=https%3A//arxiv.org/pdf/2106.09685.pdf) 算法是在 每层 transfomer block 旁边引入一个并行低秩的支路，支路的输入是transfomer block 的输入，
- PREFIX_TUNING 
- P_TUNING/P_TUNING 2
- PROMPT_TUNING



## 基于ChatGLM的P-Tuning2微调

官方chatglm微调：https://github.com/THUDM/ChatGLM-6B/blob/main/ptuning/README.md

### 基于ChatGLM的Lora微调

![image-20230423203743669](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20230423203743669.png)

![image-20230423205550920](https://okeeper-blog-images.oss-cn-hangzhou.aliyuncs.com/images/image-20230423205550920.png)



## Langchain

人工智能应用搭建的脚手架，提供了诸如在特定文档上进行问答、聊天机器人、智能代理等各类应用场景的快速搭建

https://github.com/hwchase17/langchain

https://github.com/arc53/docsgpt

https://github.com/liaokongVFX/LangChain-Chinese-Getting-Started-Guide

实现基于上下文的QA机器人：

https://python.langchain.com/en/latest/modules/chains/index_examples/vector_db_qa.html

https://python.langchain.com/en/latest/use_cases/question_answering.html


# 人工智能领域软件汇总

- transflow 神经网络框架
- PyTorch 新兴神经网络框架
- transformer  huggingface出品，类似于人工智能的运行框架和平台, 底层进一步对PyTorch/Transflow等进行封装,是人工智能模型标准定义
- huggingface：https://huggingface.co/  是一个AI领域的GITHUB，上面管理的市训练好的模型(Model)、数据集（Dataset）、社区(Space)
- Conda python环境隔离管理软件，因为各个python应用所需的依赖版本有时各有不同，为了防止冲突，可以通过conda进行依赖环境隔离
- Cuda Nvidia的指令集，供给应用层使用GPU计算能力，也就是给ai的训练和推演提供算力支持
- Git lfs 大文件git管理指令集，用于从：https://huggingface.co/ clone下载模型文件
- Langchain 人工智能应用搭建的脚手架，提供了诸如在特定文档上进行问答、聊天机器人、智能代理等各类应用场景的快速搭建

