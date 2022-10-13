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

## posix-futex
这个模块包含了 `futex` 模块的相关操作，包含一个 `futex_list` (双向链表？)和相关的函数以及一个系统调用定义。
```c
static int futex_wait(uint32_t *uaddr, uint32_t val, const struct timespec *tm);
static int futex_wake(uint32_t *uaddr, uint32_t val);
static int futex_cmp_requeue(uint32_t *uaddr, uint32_t val, uint32_t val2,uint32_t *uaddr2, uint32_t val3);

UK_LLSYSCALL_R_DEFINE(int, futex, uint32_t *, uaddr, int, futex_op, uint32_t, val, const struct timespec *, timeout, uint32_t *, uaddr2, uint32_t, val3);
```

## posix-libdl
这个模块包含了对于动态链接库的调用，但是似乎并没有填充内容，目前只是为了防止一个框架，方便后期填充？
```c  
void *dlopen(const char *filename, int flags)
{
	uk_pr_debug("%s(%s, %d)\n", __func__, filename, flags);
	return 0;
}
  
int dlclose(void *handle __unused)
{
	return 0;
}
  
void *dlsym(void *handle __unused, const char *symbol __unused)
{
	return 0;
}

char *dlerror(void)
{
	return 0;
}
  
int dladdr(const void *addr __unused, Dl_info *info __unused)
{
	return 0;
}
  
int dlinfo(void *handle __unused, int request __unused, void *info __unused)
{
	return 0;
}
  
void *dlvsym(void *handle __unused, const char *symbol __unused,
const char *version __unused)
{
	return 0;
}
```
