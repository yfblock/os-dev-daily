## Abstract

>In prior work, we have shown that the underdiagnosed problem of state spill remains a barrier to realizing complex systems that are easy to maintain, evolve, and run reliably. This paper shares our early experience building Theseus from scratch, an OS with the guiding principle of eliminating state spill. Theseus takes inspiration from distributed systems to rethink state management, and leverages Rust language features for maximum safety, code reuse, and efficient isolation. We intend to demonstrate Theseus as a runtime composable OS, in which entities are easily interchangeable and can evolve independently without reconfiguring or rebooting.

在先前的工作中，我们已经表明未经审查的状态溢出问题仍然是易于维护、发展和可靠运行的复杂系统化的障碍。这篇论文展示了我们从头开始构建 `Theseus` 的早期经历和一个以消除状态溢出为指导准则的操作系统。`Theseus` 从分布式系统获取灵感重新思考状态管理，

