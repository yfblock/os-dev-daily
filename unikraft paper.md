 # UniKraft 论文

## Abstract 

> Unikernels are famous for providing excellent performance in terms of boot times, throughput and memory consumption, to name a few metrics. However, they are infamous for making it hard and extremely time consuming to extract such performance, and for needing significant engineering effort in order to port applications to them. We introduce Unikraft, a novel micro-library OS that (1) fully modularizes OS primitives so that it is easy to customize the unikernel and include only relevant components and (2) exposes a set of composable, performance-oriented APIs in order to make it easy for developers to obtain high performance. Our evaluation using off-the-shelf applications such as nginx, SQLite, and Redis shows that running them on Unikraft results in a 1.7x-2.7x performance improvement compared to Linux guests. In addition, Unikraft images for these apps are around 1MB, require less than 10MB of RAM to run, and boot in around 1ms on top of the VMM time (total boot time 3ms-40ms). Unikraft is a Linux Foundation open source project and can be found at www.unikraft.org.

Unikraft 因提供启动方面出色的性能，吞吐量和内存消耗而闻名，举几个指标。然而，它因导出工作变得困，需要大量的时间消耗并且需要大量工程师的努力才能将程序移植到它上面而臭名昭著。我们介绍 Unikraft

### 3.2 ukalloc API

> The struct uk_alloc * argument represents the allocation backend. This structure contains function pointers that refer to the allocator’s implementation of the POSIX allocation interface: malloc(), calloc(), posix_memalign(), etc. Note that uk_malloc(), like most of the internal allocation interface, is designed as an inline method in order to avoid any additional function call overhead in the allocation path. Allocators must specify an initialization function which is called by ukboot at an early stage of the boot process. Initialization functions are passed a void * base pointer to the first usable byte of the heap, along with a size_t len argument which specifies the size of the heap. They must fully initialize the allocator and register the allocator with the ukalloc interface. The allocator is considered ready to satisfy memory allocations as soon as the initialization function returns. The boot process sets the association between memory allocators and memory sources.

`uk_alloc`结构参数存代表了`allocation`后端。这个结构包含了指向 `Posix` 分配器接口： `malloc`，`calloc`，`posix_memalign` 等`allocator`实现的函数指针。注意`uk_malloc()`像大部分内部的分配器接口，被设计作为`inline`方法以避免在申请路径中出现一下额外的函数调用。申请者必须识别一个在引导过程早期阶段被`ukboot`调用的初始化函数。初始化函数被传递一个`void*`指向堆第一个可用字节的基指针，伴随一个制定堆大小的`size_t`的`len`参数。他们必须完全初始化分配器并且利用`ukalloc`接口注册分配器。分配器被认为一旦初始化函数返回就已准备好能满足内存分配。引导过程设置了内存分配器和内存源之间的关联。

> Unikraft supports five allocation backends: a buddy system, the Two-Level Segregated Fits [53] (TLSF) real-time memory allocator, tinyalloc [67], Mimalloc [42] (version 1.6.1) and the Oscar [12] secure memory allocator. A special case are garbage-collection (GC) memory allocators that require a thread to perform GC. We can implement these with two allocators, one for the early boot time that initializes the GC thread, and then the main GC allocator, which takes over as soon as its thread is started; we use this solution for Mimalloc because it has a pthread dependency.

Unikraft 支持五种分配器后端：a buddy system，the Two-Level Segregated Fits [53] (TLSF) real-time memory allocator，tinyalloc， Mimalloc 和 the Oscar [12] secure memory allocator. 一个特殊的情况是垃圾回收内存分配器需要一个线程去执行 GC(垃圾回收)，我们能用两个分配器来实现它，一个在引导早期阶段初始化垃圾回收线程，然后是主垃圾回收分配器，一旦垃圾回收分配器启动就接受。我们使用这种对`mimalloc`的解决方案因为他有一个`pthread`依赖。

### 3.3 uksched and uklock APIs

> Unlike many OSes, scheduling in Unikraft is available but optional; this enables building lightweight single-threaded unikernels or run-to-completion unikernels, avoiding the jitter caused by a scheduler within the guest. Example use cases are to provide support functions as virtual machines (as in driver domains), or Virtual Network Functions (VNFs).

不想很多操作系统，调度在 Unikraft 中存在但是是可选的。这使构建轻量级单线程 unikernel 或者 运行完整版 unikernel 成为可能。避免客户机调度造成的抖动。使用例子如提供支持功能作为虚拟机（或者驱动程序域中）或者虚拟网络功能（VNFs）。

> Similar to ukalloc, uksched abstracts actual scheduler interfaces. The platform library provides only basic mechanisms like context switching and timers so that scheduling logic and algorithms are implemented with an actual scheduler library (Unikraft supports co-operative and pre-emptive schedulers as of this writing).

与`ukalloc`类似，`uksched`抽象了真实的调度器接口。平台库只提供了像上下文切换和计时器这些基础的机制，以便一个真实的调度器库实现调度逻辑和算法。 (在撰写本文时，Unikraft 支持协作式和抢占式调取器)

>  Additionally, Unikraft supports instantiating multiple schedulers, for example one per available virtual CPU or for a subset of available CPUs. For VNFs for example, one may select no scheduling or cooperative scheduling on virtual cores that run the data plane processing because of performance and delay reasons, but select a common preemptive scheduler for the control plane.

除此之外，Unkraft 支持实例化多个调度器，例如每个可用 CPU 或每个可用 CPU 的子集。对于虚拟网络功能，由于性能和延迟原因，可以在运行出具平面处理的虚拟内核上选择不调度或者协作式调度，但可以对控制平面选择一个普通的抢占式调取器。

> The uklock library provides synchronization primitives such as mutexes and semaphores. In order to keep the code of other libraries portable, uklock selects a target implementation depending on how the unikernel is configured. The two dimensions are threading and multi-core support. In the simplest case (no threading and single core), some of the primitives can be completely compiled out since there is no need for mutual exclusion mechanisms. If multi-core were enabled (we do not yet support this), some primitives would use spin-locks and RCUs, and so in this case they would be compiled in.

`uklock` 库提供了一个同步原语例如互斥锁和信号量。为了保持其他库的代码的可移植性，`uklock` 根据 unikraft 的配置来选择目标实现。这两个维度是线程和多核支持。在最简单的情况（单核心无线程），一旦没有互斥机制的需求，一些原语江北完全编译出去。如果多核被开启（我们还没有支持），一些原语将使用自旋锁和RCUs，因此在这种情况下他们将被编译进来。

## 4. Application Support and Porting

> Arguably, an OS is only as good as the applications it can actually run; this has been a thorn on unikernels’ side since their inception, since they often require manual porting of applications. More recent work has looked into using binary compatibility, where unmodified binaries are taken and syscalls translated, at run-time, into a unikernel’s underlying functionality [37, 54]. This approach has the advantage of requiring no porting work, but the translation comes with important performance penalties.

按理来说，一个操作系统和它能真正运行的应用程序一样好; 自 unikernel 成立以来，这一直是他的眼中钉, 因为他们经常需要手动移植应用程序。最近的工作研究了使用二进制兼容性，其中采用了未修改的二进制文件，并在运行时将系统调用转换的 unikernel 的底层功能。这个研究有无需移植工作的优势，但是翻译伴随着重要的性能损失。

> To quantify these, Table 1 shows the results of microbenchmarks ran on an Intel i7 9700K 3.6 GHz CPU and Linux 5.11 that compare the cost of no-op system and function calls in Unikraft and Linux (with run-time syscall translation for Unikraft). System calls with run-time translation in Unikraft are 2-3x faster than in Linux (depending on whether KPTI and other mitigations are enabled). However, system calls with run-time translation have a tenfold performance cost compared to function calls, making binary compatibility as done in OSv, Rump and HermiTux [36, 37, 54] expensive.

为了量化这些，表一展示了在 Intel i7 9700k 3.6GCPU 和 Linux 5.11 微基准的结果，比较了在 Unikraft 和 Linux 中无操作的系统和函数调用的消耗（Unikraft 有运行时系统调用翻译）。系统调用运行时翻译在 Unikraft 中比 Linux 快 2-3 倍。（依赖于 KPTI 和其他的缓解措施是否开启）。然而，运行时系统调用翻译对比函数调用会有十倍的性能消耗，这使得在 OSv Rump 和 HermiTux 中实现的二进制兼容性十分昂贵。

> For virtual machines running a single application, syscalls are likely not worth their costs, since isolation is also offered by the hypervisor. In this context, unikernels can get important performance benefits by removing the user/kernel separation and its associated costs. The indirection used by binary compatibility reduces unikernel benefits significantly.

对于运行一个应用程序的虚拟机，系统调用可能不值得花费成本，因为 Hypervisor 也提供了隔离。在这个上下文中，通过移除 用户态/内核态的隔离以及他们相关的消耗 unikernel 能获得更重要的性能优势。使用二进制兼容性间接大大地降低了 unikernel 的优势。

> To avoid these penalties but still minimize porting effort, we take a different approach: we rely on the target application’s native build system, and use the statically-compiled object files to link them into Unikraft’s final linking step. For this to work, we ported the musl C standard library, since it is largely glibc-compatible but more resource efficient, and newlib, since it is commonly used to build unikernels.

为了避免这些这些消耗但是仍旧最小化移植工作，我们做了一个不同的研究：我们依赖目标应用程序本地构建系统，并且使用静态编译的目标文件将他们链接进 Unikraft 的最终链接步骤里。为了让这个工作，我们移植了 musl C 的标准库，因为它很大程度上和 glibc 兼容但是更加节省内存，并且是一个新的库，因为它经常被用到编译 unikernels.

> To support musl, which depends on Linux syscalls, we created a micro-library called syscall shim: each library that implements a system call handler registers it, via a macro, with this micro-library. The shim layer then generates a system call interface at libc-level. In this way, we can link to system call implementations directly when compiling application source files natively with Unikraft, with the result that syscalls are transformed into inexpensive function calls.

为了支持依赖系统调用的 musl，我们创建了一个叫做 syscall shim 的小型库：每一个实现系统调用处理的库通过宏和这个微型库注册它。Shim 层然后在 libc-level 生成一个系统调用接口。通过这种方式，我们在是使用 unikraft 本地编译文件系统源代码的时候能将他们与系统调用实现接口链接起来。然后系统调用会被翻译为更节省的函数调用。

> Table 2 shows results when trying this approach on a number of different applications and libraries when building against musl and newlib: this approach is not effective with newlib [2] (“std” column), but it is with musl: most libraries build fully automatically. For those that do not, the reason has to do with the use of glibc-specific symbols (note that this is not the case with newlib, where many glibc functions are not implemented at all). To address this, we build a glibc compatibility layer based on a series of musl patches [56] and 20 other functions that we implement by hand (mostly 64-bit versions of file operations such as pread or pwrite).

表 2 显示了当使用这个研究在针对 musl 和 newlib 的一些应用程序和库的结果：这个结果对于 newlib 不够有效，但是对于 musl 大多数库完全自动编译。对于那些不这样做的，原因是使用了 glibc 特定的符号（注意这不是 newlib 的原因，许多 glibc 函数根本没有实现。）为了定位这些，我们基于一系列 musl 补丁和 20 个我们手动实现的函数构建了 glibc 兼容层（大多数 64 位文件操作版本 比如 pread 和 pwrite）

> With this in place, as shown in the table (“compat layer” column), this layer allows for almost all libraries and applications to compile and link. For musl that is good news: as long as the syscalls needed for the applications to work are implemented, then the image will run successfully (for newlib the stubs would have to be implemented).

有了这个，如表中所示（兼容层列），这层允许几乎所有应用程序和库进行编译和链接。对于 musl 来说是好消息：只需要应用程序工作需要的系统调用被实现，然后镜像就能成功运行。（对于 newlib stubs需要被实现。）

### 4.1 应用程序兼容性

> How much syscall support does Unikraft have? As of this writing, we have implementations for 146 syscalls; according to related work [54, 74], in the region of 100-150 syscalls are enough to run a rich set of mainstream applications, frameworks and languages; we confirm this in Table 3, which lists software currently supported by Unikraft.

Unikraft 有多少系统调用支持？在撰写本文时，我们已经实现了 146 个系统调用； 根据相关工作[54, 74], 在 100-150 区间的系统调用数量已经能够运行主流的应用程序，框架和语言。我们在表 3 确认了它，列出了目前 Unikraft 支持的软件。

> Beyond this, we conduct a short analysis of how much more work it might take to support additional applications. We use the Debian popularity contest data [14] to select a set of the 30 most popular server applications (e.g., apache, mongodb, postgres, avahi, bind9). To derive an accurate set of syscalls these applications require to actually run, and to extend the static analysis done in previous work [61] with dynamic analysis, we created a small framework consisting of various configurations (e.g., different port numbers for web servers, background mode, etc.) and unit tests (e.g., SQL queries for database servers, DNS queries for DNS servers, etc.). These configurations and unit tests are then given as input to the analyzer which monitors the application’s behavior by relying on the strace utility. Once the dynamic analysis is done, the results are compared and added to the ones from the static analysis.

初次之外，我们对需要多少工作去支持额外的应用程序做了小小的分析。我们使用 debain 流行度数据去选择了一个 30 个流行应用程序的数据集（例如： apache，MongoDB，postgres，avahi，bind9）。为了得出这些应用程序真正运行所需要的系统调用集，并且使用动态分析扩展先前工作中用到的静态分析，我们创建了一个小的框架去包含不同的配置（例如：web服务的不同端口，后台模式等等）和单元测试（例如 数据库的 sql 执行，DNS 服务器的 DNS 查询等等）。这些配置和单元测试然后被作为输入给分析器，该分析器依靠 strace 实用程序去监控应用程序行为，一旦动态分析结束，然后将结果对比并添加到静态分析。

> We plot the results against the syscalls currently supported by our system in the heatmap on Figure 5 (the entire analysis and heatmap generation is fully automated by a set of tools we developed). Each square represents an individual syscall, numbered from 0 (read) to 313 (finit_module). Lightly colored squares are required by none of the applications (0 on the scale) or few of them (20% of them); black squares (e.g., square 1, write) are required by all. A number on a square means that a syscall is supported by Unikraft, and an empty square is a syscall not supported yet.

我们将目前支持的系统调用的结果绘制在热力图 5 中（整个静态分析和热力图生成完全由我们开发的一系列工具自动完成。）
