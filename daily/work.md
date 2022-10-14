# Daily Work
> 从 `2022-10-10` 开始记录, 😰

## 2022-10-10
1. 读一篇论文 [[The case for writing a kernel in Rust]]
2. 开始

## 2022-10-14
1. 分享了一下 之前的进度
2. 准备开始做 `basic` `os`
3. 目前可以准备实现的接口 `alloc` 相关的，`alloc`，`falloc`。以及文件系统相关的。
可能的模块列表（目前考虑能解耦的，但是怎么初始化？  靠构建工具在运行时初始化？）：

- kernel
	- console
	- print
	- random
	- alloc
		- allocbuddy
		- allocpool
		- 待续
	- vfscore
		- ramfs
		- fat32fs
		- devfs
