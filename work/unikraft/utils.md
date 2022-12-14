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

## posix-sysinfo
这个模块包含了 `sysinfo` 相关的系统调用，并将系统信息定义在这个模块内部。
```c
static struct utsname utsname = {
	.sysname = "Unikraft",
	.nodename = "unikraft",
	/* glibc looks into the release field to check the kernel version:
	* We prepend '5-' in order to be "new enough" for it.
	*/
	.release = "5-" STRINGIFY(UK_CODENAME),
	.version = STRINGIFY(UK_FULLVERSION),
#ifdef CONFIG_ARCH_X86_64
	.machine = "x86_64"
#elif CONFIG_ARCH_ARM_64
	.machine = "arm64"
#elif CONFIG_ARCH_ARM_32
	.machine = "arm32"
#else
#error "Set your machine architecture!"
#endif
};
```
下面是函数表：
```c
UK_SYSCALL_R_DEFINE(int, sysinfo, struct sysinfo *, info);
long fpathconf(int fd __unused, int name __unused);
long pathconf(const char *path __unused, int name __unused);
long sysconf(int name);
size_t confstr(int name __unused, char *buf __unused, size_t len __unused);
int getpagesize(void);
UK_SYSCALL_R_DEFINE(int, uname, struct utsname *, buf);
UK_SYSCALL_R_DEFINE(int, sethostname, const char*, name, size_t, len);
int gethostname(char *name, size_t len);

```

## uklibparam
包含了 `unikraft` 参数列表读取设置。 包含 `uk_usage`，主要用在 `boot` 期间，当 `CONFIG_LIBUKLIBPARAM` 开启时会读取参数，如果参数不匹配则输出 `usage` 信息，如果匹配则进行处理。

## ukargparse
包含两个函数：`left_shift` 和 `uk_argnparse`。似乎是用来从字符串中匹配参数用的。

## ukcpio
似乎是一个解压工具，包含了相关操作。包含 `ukcpio_extract`、`read_section`、`absolute_path` 等。 似乎和其他模块的耦合度也不是很高。

## ukdebug
很像 `Rust` 的 `log` 库，包含了输出相关的函数和宏，不过额外的存在一些其他的调试功能，比如 `hexdump` 和 `asmdump`。`hexdump` 似乎是用来输出内存，而 `asmdump` 用来反汇编代码？

## ukmpi
好像是一个叫 `mailbox` 的东西的库，不太了解。

## ukswrand
```c
ssize_t getrandom(void *buf, size_t buflen, unsigned int flags);
void uk_swrand_init_r(struct uk_swrand *r, unsigned int seedc, const __u32 seedv[]);
__u32 uk_swrand_randr_r(struct uk_swrand *r);
  
__u32 uk_swrandr_gen_seed32(void);
/* Uses the pre-initialized default generator  */
/* TODO: Add assertion when we can test if we are in interrupt context */
/* TODO: Revisit with multi-CPU support */
static inline __u32 uk_swrand_randr(void)
{
    unsigned long iflags;
    __u32 ret;
  
    iflags = ukplat_lcpu_save_irqf();
    ret = uk_swrand_randr_r(&uk_swrand_def);
    ukplat_lcpu_restore_irqf(iflags);
  
    return ret;
}
ssize_t uk_swrand_fill_buffer(void *buf, size_t buflen);;

```
根据接口来看似乎是一个生成随机数的工具库。

## ukstore
包含了一些存取操作，根据函数名和系统调用来看似乎就是一个 `type` 库，而且实现了引用计数相关的功能。