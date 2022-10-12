## ukring
这个库包含了 `ring` 相关的数据结构和函数
```c
struct uk_ring {
	volatile uint32_t br_prod_head;
	volatile uint32_t br_prod_tail;
	int br_prod_size;
	int br_prod_mask;
	uint64_t br_drops;
	volatile uint32_t br_cons_head __aligned(CACHE_LINE_SIZE);
	volatile uint32_t br_cons_tail;
	int br_cons_size;
	int br_cons_mask;
#ifdef DEBUG_BUFRING
	struct uk_mutex *br_lock;
#endif
	void *br_ring[0] __aligned(CACHE_LINE_SIZE);
};
```

## isrlib
`isrlib` 中仅包含了一个头文件 `string.h` 和 `string.c`，包含了一些字符串操作相关的函数。

## ubsan
也许和 Linux 一样是定义了一些工具？ 目前还没有看的很明白，也许后面可以慢慢研究。
[https://blog.csdn.net/CHS007chs/article/details/80241661](https://blog.csdn.net/CHS007chs/article/details/80241661)