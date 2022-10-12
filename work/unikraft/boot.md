## ukboot
在 `ukplat_entry` 中进行设备和模块的初始化，[Unikraft Booting](https://unikraft.org/docs/develop/booting/)中有对启动顺序的简单描述。
`ukboot` 首先进行初始化（在 `uk_ctortab` 中的），然后选择一个 `alloc` 并根据内存布局 `struct ukplat_memregion_desc md;` 进行初始化。

简单初始化后进入同文件的 `main_thread_func` 中，`main_thread_func` 对一些内容进行初始化操作后进入 `main` 函数。

## uksp
只包含了 `uk_stack_chk_guard_setup` 和 `__stack_chk_fail`