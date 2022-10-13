## ukalloc
`alloc` 模块和其他模块采用的写入 `section` 然后被感知不同，`alloc` 模块只需存在一个，因此只需导出一个模块然后在 `boot` 期间选择合适的 `alloc` 模块初始化即可。
`ukalloc` 更像是一个抽象层，定义了 `alloc` 的操作和接口。方便在后面进行调用操作。

初始化期间的 `alloc` 选项。
```c
if (!a) {
#if CONFIG_LIBUKBOOT_INITBBUDDY
	a = uk_allocbbuddy_init(md.base, md.len);
#elif CONFIG_LIBUKBOOT_INITREGION
	a = uk_allocregion_init(md.base, md.len);
#elif CONFIG_LIBUKBOOT_INITMIMALLOC
	a = uk_mimalloc_init(md.base, md.len);
#elif CONFIG_LIBUKBOOT_INITTLSF
	a = uk_tlsf_init(md.base, md.len);
#elif CONFIG_LIBUKBOOT_INITTINYALLOC
	a = uk_tinyalloc_init(md.base, md.len);
#endif
} else {
	uk_alloc_addmem(a, md.base, md.len);
}
```
但是这个 `a` 是什么还没有搞清楚，但是根据 `uk_alloc_addmem` 的函数描述来看，这个 `a` 似乎就是一个分配器，这段代码的逻辑是如果没有分配器的话就初始化一个分配器，如果已经有分配器的话就将内存加到分配器上。

## allocbbuddy
实现了一个 `buddy` 分配器，在 [[alloc#ukalloc]] 中介绍过，分配器会在 `boot` 期间被初始化。