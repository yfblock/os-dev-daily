> Theseus-os 分析。

目前了解了 `Theseus` 的 `heap` 和 `multi-heaps` 机制，对后面写操作系统很有帮助，`Theseus` 的 `alloc` 会在内存不足时向 `os` 申请内存。

## Theseus 模块概览
>Theseus 的 `kernel` 文件夹中含有一个 `_doc_root.rs` 文件，里面包含了模块的基本介绍。
* `acpi`: ACPI (Advanced Configuration and Power Interface) support for Theseus, including multicore discovery.
* `apic`: APIC (Advanced Programmable Interrupt Controller) support for Theseus (x86 only), including apic/xapic and x2apic.
* `ap_start`: High-level initialization code that runs on each AP (core) after it has booted up
* `ata_pio`: Support for ATA hard disks (IDE/PATA) using PIO (not DMA), and not SATA.
* `captain`: The main driver of Theseus. Controls the loading and initialization of all subsystems and other crates.
* `compositor`: The trait of a compositor. It composites a list of buffers to a final buffer.
* `event_types`: The types used for passing input and output events across the system.
* `device_manager`: Code for handling the sequence required to initialize each driver.
* `displayable`: Defines a displayable trait. A displayable can display itself in a framebuffer.
* `text_display`: A text display is a displayable. It contains a block of text and can display in a framebuffer.
* `e1000`: Support for the e1000 NIC and driver.
* `exceptions_early`: Early exception handlers that do nothing but print an error and hang.
* `exceptions_full`: Exception handlers that are more fully-featured, i.e., kills tasks on an exception.
* `font`: Defines font for an array of ASCII code.
* `framebuffer_compositor`: Composites a list of framebuffers to a final framebuffer which is mapped to the screen.
* `framebuffer_drawer`: Basic draw functions.
* `framebuffer_printer`: Prints a string in a framebuffer.
* `framebuffer`: Defines a Framebuffer structure. It is a buffer of pixels in which an application can display.
* `fs_node`: defines the traits for File and Directory. These files and directories mimic that of a standard unix virtual filesystem
* `gdt`: GDT (Global Descriptor Table) support (x86 only) for Theseus.
* `interrupts`: Interrupt configuration and handlers for Theseus. 
* `ioapic`: IOAPIC (I/O Advanced Programmable Interrupt Controller) support (x86 only) for Theseus.
* `keyboard`: simple PS2 keyboard driver.
* `memory`: The virtual memory subsystem.
* `mod_mgmt`: Module management, including parsing, loading, linking, unloading, and metadata management.
* `mouse`: simple PS2 mouse driver.
* `nano-core`: a tiny module that is responsible for bootstrapping the OS at startup.
* `panic_entry`: Default entry point for panics and unwinding, as required by the Rust compiler.
* `panic_wrapper`: Wrapper functions for handling and propagating panics.
* `path`: contains functions for navigating the filesystem / getting pointers to specific directories via the Path struct 
* `pci`: Basic PCI support for Theseus, x86 only.
* `pic`: PIC (Programmable Interrupt Controller), support for a legacy interrupt controller that isn't used much.
* `pit_clock`: PIT (Programmable Interval Timer) support for Theseus, x86 only.
* `ps2`: general driver for interfacing with PS2 devices and issuing PS2 commands (for mouse/keyboard).
* `root`: special implementation of the root directory; initializes the root of the filesystem
* `rtc`: simple driver for handling the Real Time Clock chip.
* `scheduler`: The scheduler and runqueue management.
* `serial_port`: simple driver for writing to the serial_port, used mostly for debugging.
* `spawn`: Functions and wrappers for spawning new Tasks.
* `task`: Task types and structure definitions, a Task is a thread of execution.
* `tsc`: TSC (TimeStamp Counter) support for performance counters on x86. Basically a wrapper around rdtsc.
* `tss`: TSS (Task State Segment support (x86 only) for Theseus.
* `vfs_node`: contains the structs VFSDirectory and VFSFile, which are the most basic, generic implementers of the traits Directory and File
* `vga_buffer`: Simple routines for printing to the screen using the x86 VGA buffer text mode.
* `window`: Defines window structure which wraps a window inner object.
* `window_inner`: Defines a `WindowInner` structure which contains the information required by the window manager.
* `window_manager`: A window manager maintains a list of existing windows.

## 中文版本
* `acpi`：对 Theseus 的 ACPI（高级配置和电源接口）支持，包括多核发现。
* `apic`：APIC（高级可编程中断控制器）支持 Theseus（仅限 x86），包括 apic/xapic 和 x2apic。
* `ap_start`：启动后在每个 AP（核心）上运行的高级初始化代码
* `ata_pio`：支持使用 PIO（不是 DMA）而不是 SATA 的 ATA 硬盘（IDE/PATA）。
* `captain`：忒修斯的主要驱动。控制所有子系统和其他 crate 的加载和初始化。
* `compositor`：合成器的特征。它将缓冲区列表组合到最终缓冲区。
* `event_types`：用于跨系统传递输入和输出事件的类型。
* `device_manager`：处理初始化每个驱动程序所需的序列的代码。
* `displayable`：定义一个可显示的特征。可视组件可以在帧缓冲区中显示自己。
* `text_display`：文本显示是可显示的。它包含一个文本块，可以显示在帧缓冲区中。
* `e1000`：支持 e1000 网卡和驱动程序。
* `exceptions_early`：早期的异常处理程序，除了打印错误并挂起之外什么都不做。
* `exceptions_full`：功能更全面的异常处理程序，即在异常时终止任务。
* `font`：定义 ASCII 码数组的字体。
* `framebuffer_compositor`：将帧缓冲区列表合成为映射到屏幕的最终帧缓冲区。
* `framebuffer_drawer`：基本绘制函数。
* `framebuffer_printer`：在帧缓冲区中打印一个字符串。
* `framebuffer`：定义一个Framebuffer结构。它是应用程序可以在其中显示的像素缓冲区。
* `fs_node`：定义文件和目录的特征。这些文件和目录模仿标准的 unix 虚拟文件系统
* `gdt`：对忒修斯的 GDT（全局描述符表）支持（仅限 x86）。
* `interrupts`：忒修斯的中断配置和处理程序。
* `ioapic`：对忒修斯的 IOAPIC（I/O 高级可编程中断控制器）支持（仅限 x86）。
* `keyboard`：简单的 PS2 键盘驱动程序。
* `memory`：虚拟内存子系统。
* `mod_mgmt`：模块管理，包括解析、加载、链接、卸载和元数据管理。
* `mouse`：简单的 PS2 鼠标驱动程序。
* `nano-core`：一个微型模块，负责在启动时引导操作系统。
* `panic_entry`：panic 和默认入口点，根据 Rust 编译器的要求。
* `panic_wrapper`：用于处理和传播 panic 的包装函数。
* `path`：包含用于导航文件系统/通过 Path 结构获取指向特定目录的指针的功能
* `pci`：对 Theseus 的基本 PCI 支持，仅限 x86。
* `pic`：PIC（可编程中断控制器），支持不常用的传统中断控制器。
* `pit_clock`：PIT（可编程间隔定时器）支持 Theseus，仅 x86。
* `ps2`：与 PS2 设备连接和发出 PS2 命令（用于鼠标/键盘）的通用驱动程序。
* `root`：根目录的特殊实现；初始化文件系统的根
* `rtc`：处理实时时钟芯片的简单驱动程序。
* `scheduler`：调度程序和运行队列管理。
* `serial_port`：用于写入serial_port的简单驱动程序，主要用于调试。
* `spawn`：用于生成新任务的函数和包装器。
* `task`：任务类型和结构定义，一个Task就是一个执行线程。
* `tsc`：TSC（时间戳计数器）支持 x86 上的性能计数器。基本上是 rdtsc 的包装器。
* `tss`: TSS（任务状态段支持（仅限 x86）为忒修斯。
* `vfs_node`：包含结构体 VFSDirectory 和 VFSFile，它们是特征目录和文件的最基本、通用的实现者
* `vga_buffer`：使用 x86 VGA 缓冲区文本模式打印到屏幕的简单例程。
* `window`：定义包装窗口内部对象的窗口结构。
* `window_inner`：定义一个`WindowInner`结构，其中包含窗口管理器所需的信息。
* `window_manager`：窗口管理器维护现有窗口的列表。