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
## 2022-10-18
1. 阅读 `embassy` 代码，[https://github.com/embassy-rs/embassy](https://github.com/embassy-rs/embassy)
2. 尝试利用宏实现模块化的相关信息 [[尝试利用宏实现模块化的相关信息]]

## 2022-11-10
1. 实现设备抽象
2. 完善 设备读取
3. 实现对于设备树的读取和使用