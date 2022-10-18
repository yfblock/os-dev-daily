链接： [https://github.com/embassy-rs/embassy](https://github.com/embassy-rs/embassy)
`embassy` 是一个使用 `Rust` 编写的嵌入式框架，它实现了 `Rust` 的 `async` 功能，利用 `async` 实现了简单的多任务处理。
`embassy` 利用了过程宏来收集用户程序的信息，在用户程序上附加 `#[embassy_executor::main]` 和 `#[embassy_executor::task]` 来识别用户程序。并动态生成线程启动代码。