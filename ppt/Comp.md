---
theme: gaia
header: "**OS 内核赛参赛分享**"
marp: true
paginate: true
backgroundColor: white
---

<style>
section.split {
    overflow: visible;
    display: grid;
    grid-template-columns: 500px 500px;
    grid-template-rows: 100px auto;
    grid-template-areas: 
        "slideheading slideheading slideheading"
        "leftpanel centerpanel rightpanel";
}
/* debug */
section.split h3, 
section.split .ldiv, 
section.split .rdiv { border: 1.5pt dashed dimgray; }
section.split h3 {
    grid-area: slideheading;
    font-size: 50px;
}
section.split .ldiv { grid-area: leftpanel; }
section.split .cdiv { grid-area: centerpanel; }
section.split .rdiv { grid-area: rightpanel; }
</style>

<!-- _class: lead -->
# `OS` 内核赛参赛经验分享
##### 分享人：杨金博  河南科技大学

---
### OS 比赛

- 全称：2022全国大学生计算机系统能力大赛操作系统设计大赛
- 链接：[https://os.educg.net/2022CSCC](https://os.educg.net/2022CSCC)

### 我的比赛成绩

- 内核赛二等奖

---

### 我能参加 OS 比赛吗？

先介绍一下我在参加 OS 比赛时的基础，对 riscv 有一点了解，在四年前使用 C 写过一点 x86 的内核（或许称为裸机程序更为合理），今年刚刚开始了解 Rust, 但是对一些语法仍旧不是很熟悉，比如 Rust 的 Result 是在比赛一段时间后刷 Rustlings 才了解并使用，在比赛的初期仍处于和 Rust 编译器高强度对抗阶段。参加比赛时刚刚开始看 rcore-tutorial V3，开始写比赛的内核时学习到第四章。在比赛期间逐渐了解更多内核的知识和内核相关应用程序(比赛测例)。

比赛的周期比较长，有足够的时间去学习需要用的知识，但是有一些前置的知识会更轻松一点。

---

### 比赛期间


![bg right auto](comp/code.png)

- 438 次提交
- 总代码 62712 行
- 现存代码 15995 行
- 大概12000行自己的代码
- 初赛测例全过
- 决赛第一阶段测例全过
- 决赛第二阶段测例过大部分

---

### 值得注意的地方

- 尽量不要重复造轮子，可以在不同的地方寻找你需要的东西。比如 OS 比赛需要的文件系统。可以在 Github 和 crates.io 搜索到合适的进行使用，使用已经有的库可以让我们把精力集中在其他的地方。
- 在 crates.io 上找到需要的库后可以进入他的 Github 查看具体的版本和使用方式。例如 fatfs (一个 fat32 文件系统) 在 crates.io 上最新版本为 0.3.5，在 github 上的最新版本为0.4.0。
- 遇到问题时可以去读读测例的源码，查看它的逻辑，对实现可能有一些帮助，在实现的过程中也需要经常性的去 Debug。

---

### 值得注意的地方

- 采用适合自己的开发环境。

- 善用 Git 的版本控制和分支功能，每一个阶段都可以去创建一个 branch 或者记录当前的 commit，方便后面遇到问题时进行回滚。

- 在开发的过程中完善自己的文档，每一个阶段都对文档进行补充，方便后面的更新。

- 善于利用 Debug 功能，不仅仅是对于内核的 Debug，同样也有对于应用程序的 Debug（默认情况下比赛提供的应用程序不包含完整的符号表，需要自己去编译，可以看到具体是程序的哪一部分出现问题，然后针对性的调试）。

---

### 一些工具

- 建议使用 Ubuntu 或者其他的 Linux 发行版进行开发，可以很方便的安装工具和环境。

- Debug 工具 riscv-elf-gdb (也可以是其他的工具)

- 调试 libc 和测例的编译工具链 Buildroot，buildroot 可以构建编译器，构建的 libc 可以选择包含符号表，可以针对 libc 进行调试。

- hexdump 和 readelf，方便查看编译后的程序信息。

- Linux manual page 搜索系统调用相关功能和参数。

---

### 初赛

2022 年初赛内容为一些 Syscall 测试用例，这个阶段难度并不大，需要的是对一些系统调用进行支持。系统调用的数量也并不多，基本上都是常用的一些简单的系统调用，如果基于 rcore 或者其他的系统，可以很快的完成这部分功能。

如果在比赛初期能够支持一个 sh 的应用程序，会对后面的任务和调试起到很大的帮助（但不是必须的）。

---

### 决赛第一阶段

2022 年决赛第一阶段是 libc-test 的相关内容，这个阶段需要内核对 musl libc 进行支持，并通过 libc-test 的测试。

这个阶段并不是只在内核中做工作就可以了，而是需要深入到 libc 的源代码中，观察和调试 libc 需要哪些环境变量，需要哪些参数，参数怎么设置才能启动起来。在这个阶段工程量非常大，需要支持和思考的东西比较多。

完成这个阶段的东西后，支持其他的应用程序就会比较简单。

---

### 决赛第二阶段

2022 年决赛的第二阶段是 busybox、lua 和 lmbench，同时这些也是 2021 年决赛两个阶段的内容。但是比较幸运的是，这些东西都是基于 libc 的，busybox 和 lua 基于 musl-libc，lmbench 基于 glibc（但是差距也不是很大，这个地方需要一些工作量）。

这个阶段时间比较紧张，支持的难度也比较大，且需要大量的 Debug，需要耐心的测试。

---

### 遇到问题怎么办

OS 比赛的遇到问题解决方案大致有以下几种：

- 用搜索引擎进行搜索
- 去 Stack overflow 搜索
- 去 Github 查 Issues (一部分问题可以)
- 读相关手册
- 查看相关项目的代码
- 去啃 Linux 项目的代码
- 看看其他参赛队怎么做的

---

### 是否可以基于已有的项目

如果已经学过 rcore、ucore 或者 xv6 等教学操作系统并且有兴趣继续使用他们或者想减少一些工作量，我很推荐基于已有的项目。

从零开始去构建一个 OS，我认为是很有帮助的，但是不推荐在 OS 比赛中这么做。我是从零开始构建的，在比赛的前期并没有带来什么压力，但是在比赛的后期测例逐渐复杂，早期的设计已经不足以胜任，需要进行部分重构。因此才出现前面叙述的代码新增和删除量都很大，也会对后面功能的编写带来一些心智负担。

---

### 组队参加会不会更好
组队参加自然更好，这样就可以分担一下压力，如果比较有默契会大大减少工作量。但是如果和别人的配合并不是很默契，或者更希望一个人去挑战，自己一个人也是可以参加的。

---

### 给后面的同学一些选题上的建议

- 建议不要去选内存特别小的设备，因为这会给你带来很多额外的工作，千方百计的去思考怎么去减少内存使用，怎么去优化，这会带来很大的时间消耗。
- 选择相对熟悉的硬件，这样出现问题也知道怎么去解决。
- 搜索哪个设备的驱动和文档更完善，在文档比较完善的情况下，开发会比较容易。

---

### 对 OS 比赛有兴趣

- 如果对 OS 比赛有兴趣，想看看今年的比赛内容并进行尝试，可以参考一下内容
- 比赛测例仓库：[https://github.com/oscomp/testsuits-for-oskernel](https://github.com/oscomp/testsuits-for-oskernel)
- os 训练仓库(基于 Github Classroom 对 os 比赛内核赛环境的复现，包含 libc-test、busybox、lua、lmbench) [https://github.com/learningOS/oscomp-kernel-training](https://github.com/learningOS/oscomp-kernel-training)
- 比赛官网 [https://os.educg.net/2022CSCC](https://os.educg.net/2022CSCC)

---

<!-- _class: lead -->

# 感谢各位的聆听