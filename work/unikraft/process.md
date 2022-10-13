## posix-process
这个模块包含了进程相关的系统调用 `fork` 和 `vfork`，但是目前还没有内容的填充，返回值也是 `ENOSYS`。
```c
UK_SYSCALL_R_DEFINE(int, fork)
{
	/* fork() is not supported on this platform */
	return -ENOSYS;
}
  
UK_SYSCALL_R_DEFINE(int, vfork)
{
	/* vfork() is not supported on this platform */
	return -ENOSYS;
}
```
后面又大致看了一下其他的东西，感觉这个模块也是一个半成品。

## uklock
包含了 `mutex` 操作，以及一个原子操作的初始化函数。
```c
void uk_mutex_init(struct uk_mutex *m);
static int mutex_metrics_ctor(void);
void uk_mutex_get_metrics(struct uk_mutex_metrics *dst);
void uk_semaphore_init(struct uk_semaphore *s, long count);
```

## ukshed
一个线程调度器并且包含了线程相关的结构。

## ukshedcoop
同样也是一个线程调度器。

## uksglist
定义了信号系统的基本结构和函数，包含添加、删除等操作，主要是对信号队列的处理。

## uksignal
这个模块包含了信号相关的系统调用和函数，主要是针对信号的具体处理。

