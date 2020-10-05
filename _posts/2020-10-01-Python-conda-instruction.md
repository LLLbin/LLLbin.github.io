---
layout:     post                    
title:      Python - conda - instruction
subtitle:   那些conda的常用指令👻
date:       2020-10-01            
author:     Lebin                     
header-img: img/post-bg-conda.jpg
catalog: true                       
tags:             
    - python                  
    - conda
---

# 那些conda的常用命令

## 0.获取版本号
```
conda --version
conda -V
```

##### 更新至最新版本(同时也会更新包)
```
conda update conda
```

## 1.获取帮助
```
conda --help
conda -h
```

##### 查看某一命令的帮助，如update命令及remove命令
```
conda update --help
conda remove --help
```

## 2.环境管理

##### 查看环境管理的全部命令帮助
```
conda env -h
```

##### 创建环境
```
conda create --name your_env_name
```

##### 创建制定python版本的环境
```
conda create --name your_env_name python=2.7
conda create --name your_env_name python=3.8
```
##### 创建包含某些包的环境
```
conda create --name your_env_name numpy scipy
```

##### 复制某个环境
```
conda create --name new_env_name --clone old_env_name 
```

##### 列举当前所有环境
```
conda info --envs
conda env list
```

##### 进入某个环境
```
activate your_env_name
```

##### 退出当前环境
```
deactivate 
```

##### 删除某个环境
```
conda remove --name your_env_name --all
```

## 3.分享环境
```
1. activate target_env                      进入环境target_env     

2. conda env export > environment.yml       当前工作目录下生成一个environment.yml文件     

3. conda env create -f environment.yml      目的工作目录下，可以通过以下命令从该文件创建环境       
```

## 4.包管理

##### 列举当前环境下的所有包
```
conda list
```

##### 列举一个非当前环境下的所有包
```
conda list -n your_env_name
```

##### 为指定环境安装某个包
```
conda install -n env_name package_name
```

##### 删除当前环境中的包
```
conda remove package
```

##### 删除指定环境中的包
```
conda remove -- name env_name package
```

##### 更新指定的包
```
conda update package_name
```

##### 更新所有包
```
conda update --all
```