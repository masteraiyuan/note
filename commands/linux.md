[TOC]

## linux常用命令
### 端口和进程

1. 根据进程pid查端口

	> lsof -i | grep pid

2. 根据端口port查进程（某次面试还考过）：

	> lsof  -i:port     

3. 根据进程pid查端口：

	> netstat -nap | grep pid

4. 根据端口port查进程

	> netstat -nap | grep port
	
### 压缩和解压 【tar(选项)(参数)】

1. 参数

	- -c ：建立一个压缩文件的参数指令(create 的意思)；
	- -x ：解开一个压缩文件的参数指令;
	- -z ：是否同时具有 gzip 的属性？亦即是否需要用 gzip 压缩。
	- -j ：是否同时具有 bzip2 的属性？亦即是否需要用 bzip2 压缩
	- -f ：使用档名，请留意，在 f 之后要立即接档名喔！不要再加参数
	- -t或--list：列出备份文件的内容
	- -p ：使用原文件的原来属性（属性不会依据使用者而变）

2. 范例

	压缩
	> tar -zcvf /tmp/etc.tar.gz /etc
	
	查阅tar包内有哪些文件：
	> tar -ztvf /tmp/etc.tar.gz
	
	解压缩
	> tar -zxvf /tmp/etc.tar.gz

### XZ压缩
##### 解压tar.xz文件：先 xz -d xxx.tar.xz 将 xxx.tar.xz解压成 xxx.tar 然后，再用 tar xvf xxx.tar来解包。
1. xz -d 要解压的文件
2. xz -z 要压缩的文件
	
### 软连接 【ln –s 源文件 目标文件】

1. 添加软连接
	- ln -s /home/soft/node-v0.10.28-linux-x64/bin/node /usr/local/bin/node