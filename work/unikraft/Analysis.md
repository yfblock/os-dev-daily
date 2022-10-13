> Unikraft 的模块并不是完全做到松耦合的，一些模块之间还是存在一些依赖关系，一个模块被选择的时候一定要含有另一个模块。可能会存在循环依赖的模块使用写入 `section` 指针的方式来感知模块。因此实现的一种模块化。

## Modules
- ukalloc [[alloc#ukalloc]]
- ukallocpool [[alloc#ukallocpool]]
- ukallocregion [[alloc#ukallocregion]]
- ukallocbbuddy [[alloc#ukallocbbuddy]]
- ukfalloc [[alloc#ukfalloc]]
- ukfallocbuddy [[alloc#ukfallocbuddy]]
- posix-process [[process#posix-process]]
- syscall_shim [[libc#syscall_shim]]
- ukbus [[device#ukbus]]
- uklibparam [[utils#uklibparam]]
- ukring [[utils#ukring]]
- uksignal [[process#uksignal]]
- nolibc [[libc#nolibc]]
- posix-socket [[device#posix-socket]]
- ubsan [[utils#ubsan]]
- ukcpio [[utils#ukcpio]]
- uklock [[process#uklock]]
- ukrust [[ukrust#ukrust]]
- uksp [[boot#uksp]]
- posix-event  [[Interrupt#posix-event]]
- posix-sysinfo [[utils#posix-sysinfo]]
- uk9p [[device#uk9p]]
- ukargparse [[utils#ukargparse]]
- ukdebug [[utils#ukdebug]]
- ukmmap [[libc#ukmmap]]
- uksched [[process#ukshed]]
- ukschedcoop [[process#ukshedcoop]]
- ukstore [[utils#ukstore]]
- fdt [[boot#fdt]]
- posix-futex [[utils#posix-futex]]
- posix-user [[libc#posix-user]]
- ukblkdev [[device#ukblkdev]]
- ukmpi [[utils#ukmpi]]
- ukswrand [[utils#ukswrand]]
- isrlib [[utils#isrlib]]
- posix-libdl [[utils#posix-libdl]]
- ukboot [[boot#ukboot]]
- uknetdev [[device#uknetdev]]
- uksglist [[process#uksglist]]
- uktest 包含了 `UK_ASSERT` 相关的数据
- uktime [[Interrupt#uktime]]
- uktimeconv [[Interrupt#uktimeconv]]
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
