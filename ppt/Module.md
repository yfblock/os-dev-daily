---
theme: gaia
header: "**模块化操作系统的设计与实现**"
marp: true
paginate: true
backgroundColor: white
---

<!-- _class: lead -->
# 模块化操作系统的设计与实现
##### 分享人：杨金博

---

### 模块化的背景

---

### 模块化的好处

- 提高了操作系统中模块的复用性
- 更好的可移植性  针对某个平台实现操作系统移植的时候，不需要修改所有的模块，只需要对一些模块进行修改或者添加新的模块即可运行。
- 可以根据需要选择内核模块，减小内核体积，降低维护成本

---

### 实现模块化的措施

- 尽可能使用抽象来构建模块
- 尽可能的分离非必要模块
- 尽量减少模块之间的接触，利用接口去感知和使用模块
- 利用元编程、链接工具减少模块之间的代码接触。

---

### 采用机制

- 如果一个下层模块需要使用，必须保证他的上层模块必须被选择，
- 同类型的模块不能被同时选择

上述内容利用`Rust`的宏和编译系统来实现。


---

### 核心组件
操作系统的核心模块:
- kalloc
- console (可选，但是大部分系统需要使用)

---

### 目前的进度
- 构建了系统的骨架
- 利用设备树感知设备和内存
- 实现 `console` 的抽象