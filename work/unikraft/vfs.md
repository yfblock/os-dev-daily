## ramfs
一个很简单的逻辑
## 9pfs

## devfs

## vfscore
在 `vfscore` 中的 `main.c` 最后使用 `UK_CTOR_PRIO(vfscore_init, 1);` 将模块加入 `uk_ctortab section` 在 `ukplat_entry` 进行初始化。

定义了文件操作相关接口
```c
struct vfsops {
	int (*vfs_mount)	(struct mount *, const char *, int, const void *);
	int (*vfs_unmount)	(struct mount *, int flags);
	int (*vfs_sync)		(struct mount *);
	int (*vfs_vget)		(struct mount *, struct vnode *);
	int (*vfs_statfs)	(struct mount *, struct statfs *);
	struct vnops	*vfs_vnops;
};

typedef	int (*vnop_open_t)	(struct vfscore_file *);
typedef	int (*vnop_close_t)	(struct vnode *, struct vfscore_file *);
typedef	int (*vnop_read_t)	(struct vnode *, struct vfscore_file *,
				 struct uio *, int);
typedef	int (*vnop_write_t)	(struct vnode *, struct uio *, int);
typedef	int (*vnop_seek_t)	(struct vnode *, struct vfscore_file *,
				 off_t, off_t);
typedef	int (*vnop_ioctl_t)	(struct vnode *, struct vfscore_file *, unsigned long, void *);
typedef	int (*vnop_fsync_t)	(struct vnode *, struct vfscore_file *);
typedef	int (*vnop_readdir_t)	(struct vnode *, struct vfscore_file *, struct dirent *);
typedef	int (*vnop_lookup_t)	(struct vnode *, char *, struct vnode **);
typedef	int (*vnop_create_t)	(struct vnode *, char *, mode_t);
typedef	int (*vnop_remove_t)	(struct vnode *, struct vnode *, char *);
typedef	int (*vnop_rename_t)	(struct vnode *, struct vnode *, char *,
				 struct vnode *, struct vnode *, char *);
typedef	int (*vnop_mkdir_t)	(struct vnode *, char *, mode_t);
typedef	int (*vnop_rmdir_t)	(struct vnode *, struct vnode *, char *);
typedef	int (*vnop_getattr_t)	(struct vnode *, struct vattr *);
typedef	int (*vnop_setattr_t)	(struct vnode *, struct vattr *);
typedef	int (*vnop_inactive_t)	(struct vnode *);
typedef	int (*vnop_truncate_t)	(struct vnode *, off_t);
typedef	int (*vnop_link_t)      (struct vnode *, struct vnode *, char *);
typedef int (*vnop_cache_t) (struct vnode *, struct vfscore_file *, struct uio *);
typedef int (*vnop_fallocate_t) (struct vnode *, int, off_t, off_t);
typedef int (*vnop_readlink_t)  (struct vnode *, struct uio *);
typedef int (*vnop_symlink_t)   (struct vnode *, char *, char *);
typedef int (*vnop_poll_t)	(struct vnode *, unsigned int *,
				 struct eventpoll_cb *);
/*
 * vnode operations
 */
struct vnops {
	vnop_open_t		vop_open;
	vnop_close_t		vop_close;
	vnop_read_t		vop_read;
	vnop_write_t		vop_write;
	vnop_seek_t		vop_seek;
	vnop_ioctl_t		vop_ioctl;
	vnop_fsync_t		vop_fsync;
	vnop_readdir_t		vop_readdir;
	vnop_lookup_t		vop_lookup;
	vnop_create_t		vop_create;
	vnop_remove_t		vop_remove;
	vnop_rename_t		vop_rename;
	vnop_mkdir_t		vop_mkdir;
	vnop_rmdir_t		vop_rmdir;
	vnop_getattr_t		vop_getattr;
	vnop_setattr_t		vop_setattr;
	vnop_inactive_t		vop_inactive;
	vnop_truncate_t		vop_truncate;
	vnop_link_t		vop_link;
	vnop_cache_t		vop_cache;
	vnop_fallocate_t	vop_fallocate;
	vnop_readlink_t		vop_readlink;
	vnop_symlink_t		vop_symlink;
	vnop_poll_t		vop_poll;
};
```

需要在其他的模块中实现相关的接口信息，然后利用宏 `UK_FS_REGISTER` 将相关的接口放在 `section uk_fs_list` 中暴露出去，可以被 `vfscore` 感知并使用。如果相关接口没有实现，可以将 `vfscore_nullop` 传递过去。

文件系统相关的系统调用使用宏 `UK_LLSYSCALL_R_DEFINE` 进行重定义和导出，可以被感知和使用。