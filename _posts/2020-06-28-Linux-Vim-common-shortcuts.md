---
layout:     post                    
title:      Linux - Vim常用快捷键           
subtitle:   那些Vim上常用的快捷键🍗
date:       2020-06-28              
author:     Lebin                     
header-img: img/post-bg-Vimrc.jpg
catalog: true                       
tags:                               
    - Linux
    - Vim
    - note
---

## 启动Vim
```
vim + file              从文件的末尾开始
vim -c cmd file	        在打开文件前，先执行指定的命令；
vim -r file	            恢复上次异常退出的文件；
vim -R file	            以只读的方式打开文件，但可以强制保存；
vim -M file	            以只读的方式打开文件，不可以强制保存；
vim --remote file	    用已有的vim进程打开指定的文件。如果你不想启用多个vim会话，这个很有用。
```

## 文档操作
```
:e file	                关闭当前编辑的文件，并开启新的文件。 如果对当前文件的修改未保存，vi会警告。
:e! file	            放弃对当前文件的修改，编辑新的文件。
:e+file	                开始新的文件，并从文件尾开始编辑。
:enew	                编译一个未命名的新文档。(CTRL-W n)
:e	                    重新加载当前文档。
:e!	                    重新加载当前文档，并丢弃已做的改动。
:e#或ctrl+^	            回到刚才编辑的文件，很实用。
:f或ctrl+g	            显示文档名，是否修改，和光标位置。
:f filename	            改变编辑的文件名，这时再保存相当于另存为。
gf	                    打开以光标所在字符串为文件名的文件。
:w	                    保存修改。
:n1,n2w filename	    选择性保存从某n1行到另n2行的内容。
:wq	                    保存并退出。
ZZ	                    保存并退出。
:x	                    保存并退出。
:saveas newfilename	    另存为
```