## nolibc
`nolibc` 提供了一个基本的 C 语言的接口库。

## syscall_shim
这个模块定义了系统调用相关的模块，以及 `libc` 常用的宏，`UK_SYSCALL_R_DEFINE` 宏可以对系统调用进行重定义。目前猜测是原有的系统调用是一个弱引用，在进行 `redefine` 后变成了制定的函数。这一层主要是对 `libc` 的支持，将原有的系统调用通过一层包装可以被统一改造。

## ukmmap
对映射相关的结构和系统调用进行了定义：
```c
struct mmap_addr {
    void *begin;
    void *end;
    struct mmap_addr *next;
};
static struct mmap_addr *mmap_addr;
```
以及一些系统调用 `mmap`、`munmap`。
还有 `mremap`、`madvise`、`mprotect`，不过这些系统调用是空的。

## posix-user
包含了用户和用户组的相关操作并重写了系统调用。同样也包含了密码操作。