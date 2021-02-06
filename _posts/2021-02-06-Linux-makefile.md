---
layout:     post                    
title:      Linux - makefile         
subtitle:   编写makefile文件🌰
date:       2020-02-06            
author:     Lebin                     
header-img: img/post-bg-Vimrc.jpg
catalog: true                       
tags:                               
    - Linux
    - makefile
---

# makefile : 一次编写，终身受益

```
执行makefile ： make
执行指定makefile ： make -f file
```

### makefile命名规则
- makefile
- Makefile

### makefile三要素
- 目标
- 依赖
- 规则命令

### makefile写法
- 目标：依赖
- tab键 规则命令

### makefile进阶

#### 第一版
```
app:main.c add.c sub.c mul.c div.c
    gcc -o app -I./include main.c add.c sub.c mul.c div.c
```
如果修改其中一个文件，所有源码都要重新编译。

可以将编译过程分解，先生成.o文件，再生成结果。

#### 第二版
```
# define variable
ObjFiles = main.o add.o sub.o mul.o div.o

# use variable
app:$(ObjFiles)
	gcc -o app -I./include main.o add.o sub.o mul.o div.o

main.o:main.c
	gcc -c main.c -I ./include
add.o:add.c
	gcc -c add.c -I ./include
sub.o:sub.c
	gcc -c sub.c -I ./include
mul.o:mul.c
	gcc -c mul.c -I ./include
div.o:div.c
	gcc -c div.c -I ./include
```
makefile隐含规则： 默认处理第1个目标

#### 第三版
```
# get all .c OilbjFiles
SrcFiles = $(wildcard *.c)

# all .c files --> .o files 
ObjFiles = $(patsubst %.c, %.o, $(SrcFiles))

app:$(ObjFiles)
	gcc -o app -I./include $(ObjFiles)

# Pattern matching
%.o:%.c
	gcc -c $< -I./include -o $@

test:
	echo $(SrcFiles)
	echo $(ObjFiles1)
```
函数：
- wildcard 进行文件匹配
- patsubst 进行内容替换

变量：
- $@  目标
- $^  全部依赖
- $<  第一个依赖
- $?  第一个变化的依赖

#### 第四版
```
# get all .c OilbjFiles
SrcFiles = $(wildcard *.c)

# all .c files --> .o files 
ObjFiles = $(patsubst %.c, %.o, $(SrcFiles))

all:app 

app:$(ObjFiles)
	gcc -o app -I./include $(ObjFiles)
	
# Pattern matching
%.o:%.c
	gcc -c $< -I./include -o $@

test:
	@echo $(SrcFiles)
	@echo $(ObjFiles)

clean:
	-@rm -f *.o
	-@rm -f app

# define pseudo target
.PHONY:clean all  
```

指定编译目标：make test

增加清理目标：make clean

规则前符号：
- '@' 代表不输出该规则
- '-' 代表若规则报错，仍继续执行

定义伪目标：all

防止目标歧义：.PHONY