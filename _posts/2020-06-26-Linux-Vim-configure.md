---
layout:     post                    
title:      Linux - Vim - configure           
subtitle:   配置自己的Vim😎 
date:       2020-06-26              
author:     Lebin                     
header-img: img/post-bg-Vimrc.jpg
catalog: true                       
tags:                               
    - Linux
    - Vim
---

# Linux 配置自己的Vim

刚开始学习Linux，想在Linux上敲代码，Vim是个很好的选择， 但是它的配置有些一言难尽，身为小白，刚开始配的奇奇怪怪的，各种坑都踩了一遍，现在配好了，写篇博客记录一下吧。

我的配置主要是下载国人chxuan大神配置好的[vimplus](https://github.com/chxuan/vimplus)，后面加一些自己的配置。

如果vim完全自己从零开始打造，会踩很多坑，光一个YCM就够喝一壶了，费时费力，最后配出来的可能还不够vimplus用的的舒服。

（亲测难顶，建议不要头铁，vimplus香得不行😂）

参考链接：
- [Vim配置攻略](https://blog.csdn.net/liuchenxia8/article/details/79652847?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-15.nonecase)
- [Vim好用的配置](https://blog.csdn.net/qq_42698422/article/details/100148176)
- [vimplus](https://github.com/chxuan/vimplus)

## 在配之前需要做一些准备
- 如果 vi 上下左右变ABCD，可以输入下面命令行语句解决

```
sudo apt-get remove vim-common
sudo apt-get install vim
```

- 安装git

```
sudo apt install git
```

- 安装cmake

```
sudo apt install cmake
```

## 开始配置vimplus
```
git clone https://github.com/chxuan/vimplus.git ~/.vimplus
cd ~/.vimp
./install.sh
```
输入完命令之后等待安装（可能需要30分钟，不要退出）。

（具体参考[vimplus](https://github.com/chxuan/vimplus)）

## 根据自己需要适度修改vimplus设置
- 修改vim colors

在[~/.vim/colors](https://vimcolors.com/?)网站上寻找自己喜欢的colors

（plastic挺好看的😉）

使用git下载

```
git clone https://github.com/flrnd/plastic.vim.git
```

将下载的`plastic.vim`移动到`~/.vimplus/colors`

（其他文件可以删掉）

在文件`~/.vimrc.custom.config`中最后添加
```
set termguicolors
set background=dark
colorscheme plastic
let g:lightline = { 'colorscheme': 'plastic' }
```

（具体操作查看[colors相应的github](https://github.com/flrnd/plastic.vim)）

*这个colors和vimplus的光标所在行高亮设置冲突，会使光标所在行的代码高亮异常，所以需要修改vim配置的源文件`~/.vimr`，具体操作是注释`set cursorline`
```
" set cursorline 
```

- 设置光标移动

在文件`~/.vimrc.custom.config`中加入
```
" 光标移动到buffer的顶部和底部时保持3行距离
set scrolloff=3
```

- 设置插件NERDtree

在文件`~/.vimrc.custom.config`中加入
```
" NERDtree         
" NERD宽度设置 
let NERDTreeWinSize=23
" 打开vim时如果没有文件自动打开NERDTree
autocmd vimenter * if !argc()|NERDTree|endif
" 当NERDTree为剩下的唯一窗口时自动关闭
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
" 打开vim时自动打开NERDTree
autocmd vimenter * NERDTree
wincmd w
autocmd VimEnter * wincmd w
" 修改树的显示图标
let g:NERDTreeDirArrowExpandable = '~'
let g:NERDTreeDirArrowCollapsible = '~'
let NERDTreeAutoCenter=1
```

- 设置插件tarbag

在文件`~/.vimrc.custom.config`中加入
```
" tarbag图标设置
let g:tagbar_iconchars = ['+','-']
```

### 经过以上配置之后，Vim基本打造完成了，能很舒服的在Linux上敲代码了😄。

### 若有想配置的插件和设置，可以参考[vimplus](https://github.com/chxuan/vimplus)自行配置。