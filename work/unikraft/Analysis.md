> Unikraft 的模块并不是完全做到松耦合的，一些模块之间还是存在一些依赖关系，一个模块被选择的时候一定要含有另一个模块。可能会存在循环依赖的模块使用写入 `section` 指针的方式来感知模块。因此实现的一种模块化。

## Modules
- ukalloc [[/work/unikraft/alloc#ukalloc| 点击查看]]
- ukallocpool [[/work/unikraft/alloc#ukallocpool| 点击查看]]
- ukallocregion [[/work/unikraft/alloc#ukallocregion| 点击查看]]
- ukallocbbuddy [[/work/unikraft/alloc#ukallocbbuddy| 点击查看]]
- ukfalloc [[/work/unikraft/alloc#ukfalloc| 点击查看]]
- ukfallocbuddy [[/work/unikraft/alloc#ukfallocbuddy| 点击查看]]
- posix-process [[/work/unikraft/process#posix-process| 点击查看]]
- syscall_shim [[/work/unikraft/libc#syscall_shim| 点击查看]]
- ukbus [[/work/unikraft/device#ukbus| 点击查看]]
- uklibparam [[/work/unikraft/utils#uklibparam| 点击查看]]
- ukring [[/work/unikraft/utils#ukring| 点击查看]]
- uksignal [[/work/unikraft/process#uksignal| 点击查看]]
- nolibc [[/work/unikraft/libc#nolibc| 点击查看]]
- posix-socket [[/work/unikraft/device#posix-socket| 点击查看]]
- ubsan [[/work/unikraft/utils#ubsan| 点击查看]]
- ukcpio [[/work/unikraft/utils#ukcpio| 点击查看]]
- uklock [[/work/unikraft/process#uklock| 点击查看]]
- ukrust [[/work/unikraft/ukrust#ukrust| 点击查看]]
- uksp [[/work/unikraft/boot#uksp| 点击查看]]
- posix-event  [[/work/unikraft/Interrupt#posix-event| 点击查看]]
- posix-sysinfo [[/work/unikraft/utils#posix-sysinfo| 点击查看]]
- uk9p [[/work/unikraft/device#uk9p| 点击查看]]
- ukargparse [[/work/unikraft/utils#ukargparse| 点击查看]]
- ukdebug [[/work/unikraft/utils#ukdebug| 点击查看]]
- ukmmap [[/work/unikraft/libc#ukmmap| 点击查看]]
- uksched [[/work/unikraft/process#ukshed| 点击查看]]
- ukschedcoop [[/work/unikraft/process#ukshedcoop| 点击查看]]
- ukstore [[/work/unikraft/utils#ukstore| 点击查看]]
- fdt [[/work/unikraft/boot#fdt| 点击查看]]
- posix-futex [[/work/unikraft/utils#posix-futex| 点击查看]]
- posix-user [[/work/unikraft/libc#posix-user| 点击查看]]
- ukblkdev [[/work/unikraft/device#ukblkdev| 点击查看]]
- ukmpi [[/work/unikraft/utils#ukmpi| 点击查看]]
- ukswrand [[/work/unikraft/utils#ukswrand| 点击查看]]
- isrlib [[/work/unikraft/utils#isrlib| 点击查看]]
- posix-libdl [[/work/unikraft/utils#posix-libdl| 点击查看]]
- ukboot [[/work/unikraft/boot#ukboot| 点击查看]]
- uknetdev [[/work/unikraft/device#uknetdev| 点击查看]]
- uksglist [[/work/unikraft/process#uksglist| 点击查看]]
- uktest 包含了 `UK_ASSERT` 相关的数据
- uktime [[/work/unikraft/Interrupt#uktime| 点击查看]]
- uktimeconv [[/work/unikraft/Interrupt#uktimeconv| 点击查看]]
- 9pfs [[/work/unikraft/vfs#9pfs| 点击查看]]
- devfs [[/work/unikraft/vfs#devfs| 点击查看]]
- vfscore [[/work/unikraft/vfs#vfscore| 点击查看]]
- ramfs [[/work/unikraft/vfs#ramfs| 点击查看]]

## Boot Time
为了更简单的分析 `Unikraft` 的内核，因此采取最简单的路径进行分析，即 `x86` 的 `linux` 作为宿主机进入内核执行的代码。具体流程如下：
### Step 1
 `linux` 运行 `unikraft` 程序，入口地址为 `plat/linuxu/x86/entry64.S` 文件中的 `_liblinuxuplat_start`，完成参数入栈和栈对齐等操作后进入函数 `_liblinuxuplat_entry`(Step 2)。在 `entry64.S` 中还定义了一个 `_liblinuxuplat_start_err`，也许在后面可能会用到。
### Step 2
`_liblinuxuplat_entry` 位于 `plat/linuxu/setup.c`，函数内容如下:
```c
void _liblinuxuplat_entry(int argc, char *argv[])
{
	_init_cpufeatures();
	
	/*
	* Initialize platform console
	*/
	_liblinuxuplat_init_console();
	
	/*
	* Enter Unikraft
	*/
	ukplat_entry(argc, argv);
}
```
根据注释函数名称和注释可以看出基本的逻辑，首先初始化 `CPU` 功能，然后初始化 `console`，最后进入 `Unikraft`。这里的 `argc` 和 `argv` 是由 `linux` 传递过来。 `ukplat_entry` 在 `lib/ukboot/boot.c` 中[[/work/unikraft/boot#ukboot| 点击查看]]。在其他平台是先进行初始化后再进入内核初始化。
