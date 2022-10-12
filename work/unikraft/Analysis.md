## Modules
- posix-process
- syscall_shim
- ukallocpool
- ukbus
- uklibparam
- ukring
- uksignal
- uktime
- nolibc
- posix-socket
- ubsan
- ukallocregion
- ukcpio
- uklock
- ukrust
- uksp
- uktimeconv
- posix-event
- posix-sysinfo
- uk9p
- ukargparse
- ukdebug
- ukmmap
- uksched
- ukstore
- fdt
- posix-futex
- posix-user
- ukalloc
- ukblkdev
- ukfalloc
- ukmpi
- ukschedcoop
- ukswrand
- isrlib
- posix-libdl
- ukallocbbuddy
- ukboot [[boot#ukboot]]
- ukfallocbuddy
- uknetdev
- uksglist
- uktest 包含了 `UK_ASSERT` 相关的数据
- 9pfs [[vfs#9pfs]]
- devfs [[vfs#devfs]]
- vfscore [[vfs#vfscore]]
- ramfs [[vfs#ramfs]]

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
根据注释函数名称和注释可以看出基本的逻辑，首先初始化 `CPU` 功能，然后初始化 `console`，最后进入 `Unikraft`。这里的 `argc` 和 `argv` 是由 `linux` 传递过来。 `ukplat_entry` 在 `lib/ukboot/boot.c` 中[[boot#ukboot]]。在其他平台是先进行初始化后再进入内核初始化。
