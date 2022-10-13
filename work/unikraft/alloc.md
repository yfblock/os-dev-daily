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

## ukallocbbuddy
实现了一个 `buddy` 分配器，在 [[alloc#ukalloc]] 中介绍过，分配器会在 `boot` 期间被初始化。

## ukallocpool
类似于上述的 `allocbbuddy`，是一个可以初始化的 `allocator`。
似乎是将内存按照 `obj` 大小进行存放，然后在申请的时候计算需要多少个 `obj` 然后进行分配。

## ukallocregion
这同样也是一个分配器，下面是官方给出的一些说明

> ukallocregion is a minimalist region implementation. Note that deallocation is not supported. This makes sense because regions only allow for deallocation at region-granularity. In our case, this would imply the freeing of the entire heap, which is generally not possible.
Obviously, the lack of deallocation support makes ukallocregion a fairly bad
general-purpose allocator. This allocator is interesting in that it offers
maximum speed allocation and deallocation (no bookkeeping). It can be used as
a baseline for measurements (e.g., boot time) or as a first-level allocator
in a nested context.

ukallocregion 是一个极简的区域实现。请注意，不支持解除分配。 这是有道理的，因为区域只允许在区域粒度上释放。 在我们的例子中，这将意味着释放整个堆，这通常是不可能的。显然，缺乏释放支持使得 ukallocregion 相当糟糕通用分配器。 这个分配器很有趣，因为它提供最大速度分配和释放（无簿记）。 它可以用作测量的基线（例如，启动时间）或作为第一级分配器在嵌套上下文中。