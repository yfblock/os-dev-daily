# tlist

## fs


## net

目前 `lose-net-stack` 已经实现了 `udp`，可以对 `lose-net-stack` 进行扩展

- 添加 `lose-net-stack` `tcp` 支持
- 添加 `lose-net-stack` `ipv6` 支持
- 添加 `lose-net-stack` `icmp` 支持

或者基于 `lose-net-stack` 编写相关的用例.（也可以使用 `smoltcp`）

- 基于 `lose-net-stack` 的 `dns` `client`(`udp` 协议)
- 基于 `lose-net-stack` 的 `ntp` `client`(`udp` 协议 网络时间协议，可以通过网络同步操作系统时间，可以补全很多个人编写的 `os` 时间不准或者不支持的问题。)

## ui 

基于 `virtio-gpu` 来做一些东西，

- 利用字库显示文字(unicode)

- `bmp` 文件的解析和显示
- `ico` 文件的解析和显示
- `jpg` 文件的解析和显示
- `png` 文件的解析和显示

** 进阶 **

在有对图片的和字库的支持下，可以再结合 `virtio-input` 实现鼠标的移动和键盘输入的显示。