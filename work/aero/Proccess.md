- [ ] Study the process and thread of aero

Aero 似乎并没有县城这个概念，Aero 中的函数在创建 Task 时 tid 与 pid 保持一致，似乎并没有线程的概念，全部用进程表示。
Aero 中 Task 的数据结构如下：
```rust
pub struct Task {
sref: Weak<Task>,
  
arch_task: UnsafeCell<ArchTask>,
state: AtomicU8,
  
// Note: Aero implementes the threads and as standard processes. This
// means that when a new process is created its TID == PID and when a new
// thread is created then the PID of the thread will be the process leader's
// PID and the TID will be uniquely generated.
pid: TaskId,
tid: TaskId,
parent: Mutex<Option<Arc<Task>>>,
children: Mutex<intrusive_collections::LinkedList<TaskAdapter>>,
  
zombies: Zombies,
  
sleep_duration: AtomicUsize,
signals: Signals,
  
executable: Mutex<Option<DirCacheItem>>,
pending_io: AtomicBool,
  
pub(super) link: intrusive_collections::LinkedListLink,
pub(super) clink: intrusive_collections::LinkedListLink,
  
pub vm: Arc<Vm>,
pub file_table: Arc<FileTable>,
  
pub message_queue: MessageQueue,
  
cwd: RwLock<Option<Cwd>>,
  
pub(super) exit_status: AtomicIsize,
}
```

`Task` 中包含了 `ArchTask` 结构，将架构相关的 `Task`  结构 `ArchTask` 独立出来。