* ~~ClassRoom添加lmbench相关测例和测试脚本。（完成一部分，包含了自己的OS能跑通的和闭浩扬OS能跑通的）~~
  * ~~目前已经实现了一部分的 lmbench测试脚本，等待完善~~
  * ~~统一编译工具链，添加lmbench源码级编译 目前已经统一了编译工具链。~~
* ~~新排行榜系统的设置及发布，新排行榜系统采用一个 latest.json 来存储最新的设置。~~
  * ~~目前排行榜已经实现了在 README 里查看最新的成绩，并且支持生成最新的 latest.json 文件，暂时不在 os-autograding 里使用最新的排行榜，只尝试在 rcore 中使用。~~
  * ~~后面不再进行 ClassRoom 的体验相关优化。注重功能优化。体验再优化下去就永无止境了，感觉越想越多😅。~~
  * ~~最新的 ClassRoom 的效果 https://github.com/os-autograding/oskernel2022-byte-os/tree/gh-pages~~
* ~~`C910`操作系统移植（暂时也许不用做了？）~~
* ~~zCore添加lmbench支持~~
* ~~操作系统专题训练，模块化？（可能每周六要去开会？）~~
* ~~看论文 `unikraft` 和 `theseus` 并阅读源代码，主要是`unikraft`，目前可能还需要看 `theseus`。~~
* ~~2022-9-30 晚 7 点有操作系统交流会，分享 unikraft 相关的解析。~~
* 根据 `unikraft`提供的思路实现一个基于 `Rust` 的模块化文件系统。可以选择支持哪些文件系统。
* ~~**2022-10-13 周四上午** 分享关于 `theseus` 和 `unikraft` 的解析。[[/paper/unikraft paper]]~~
- 2022-10-16 安排下周工作：
	1. 继续阅读 `Theseus` 和 `redleaf` 的源码和论文
	2. 根据目前已经了解的模块相关的信息编写模块：
		- alloc 相关模块，包括 alloc 和 falloc，目前观看了  `Theseus`、`redleaf`、`unikraft` 的做法，可能会综合一下。
		- vfs 相关的模块，预期将 vfs 接口做一层抽象，单独放在 `vfs-interface` 文件夹中，然后实现如 `fat32`、`ramfs` 等文件系统，最后创建一个 `vfs-core` 来管理这些文件系统模块，通过 `feature` 或者其他的方式来选择使用哪些文件系统。
	3. 可能的工作是：每天阅读一篇操作系统相关的英文文献或者文章，锻炼一下这方面的能力（如果有时间的话）。
- 许善朴 可能需要一些 `ClassRoom` 方面相关的支持。
- `syzkaller` 尝试跑起来。测试实现 `os` 的 `bug`。尝试生成测试用例。可能降低一些优先级。
- `M1s Dock` 基于平头哥，尝试 `os` 跑在这个开发板上。