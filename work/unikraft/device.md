## uk9p
虽然这个模块单独存在于一个文件夹中，但是这个模块实际适合 `9pfs` 绑定的，其实放在一起也可以，拆出来也是绑定的。

## ukbus
包含总线相关的操作和一些总线操作相关的宏：`UK_BUS_REGISTER` 等等。
```c
void _uk_bus_register(struct uk_bus *b);
void _uk_bus_unregister(struct uk_bus *b);
unsigned int uk_bus_count(void);
static int uk_bus_init(struct uk_bus *b, struct uk_alloc *a);
static int uk_bus_probe(struct uk_bus *b);
static unsigned int uk_bus_init_all(struct uk_alloc *a);
static unsigned int uk_bus_probe_all(void);
static int uk_bus_lib_init(void);
```
利用 `uk_initcall_class_prio` 加入 `section` 中在系统启动时初始化。利用 `UK_LIST_HEAD(uk_bus_list);` 做了一些操作

## ukblkdev
下面是官方的说明:
>This is derived from uknetdev because of consistency reasons

这个模块包含了 `blk` 设备的操作接口定义和基本的函数以及注册方法。主要功能在头文件中。

## uknetdev
网络设备，没有详细去看。

## posix-socket
`socket` 相关的设备驱动和操作