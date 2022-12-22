---
theme: default
header: 模块化操作系统的设计与实现
marp: true
paginate: true
backgroundColor: white
footer: 开题报告
---

<!-- 全局样式 -->
<style>
    section {
        justify-content: start;
    }
</style>

<!-- 首页样式 -->
<style scoped>
    section {
        justify-content: center;
        align-items: center;
    }

    section h1, h2, h3, h4, h5, h6 {
        padding: 0;
        margin: 6px;
    }

    section h6 {
        font-weight: normal;
        font-size: 29px;
    }
</style>

# 模块化操作系统的设计与实现
## 毕业设计开题报告

#
#
#
#

###### 河南科技大学软件学院 杨金博
###### 2023年 x 月 x 日
###### 指导教师：陈渝 张晓玲

---

## 课题意义

- 快速构建
- 基于不同平台的快速移植(甚至移植到浏览器里？WASM)
- 不单单提供一个操作系统，而是提供和操作系统相关的一套框架

---

## 模块化操作系统
- 提高了操作系统中模块的复用性
- 更好的可移植性  针对某个平台实现操作系统移植的时候，不需要修改所有的模块，只需要对一些模块进行修改或者添加新的模块即可运行。
- 可以根据需要选择内核模块，减小内核体积，降低维护成本

---

## 为什么用 Rust ?

- 零开销抽象
- 独特的所有权机制
- 没有 gc
- 内存安全
- 有一套很方便的工具

---

## 设计背景：相关工作
#### rcore tutorial 模块化/微库化
- 操作系统模块 Crate 化

#### unikernel
- 静态链接

#### Theseus-OS
- 动态加载

---

## 模块化 VS 模块化

- 基于不同的理解和目的有不同的方式

---

### 实现模块化的措施

- 尽可能使用抽象来构建模块
- 尽可能的分离非必要模块
- 尽量减少模块之间的接触，利用接口去感知和使用模块
- 利用元编程、链接工具减少模块之间的代码接触(类 `unikernel` 方式)。
- 如果一个下层模块需要使用，必须保证他的上层模块必须被选择。
- 同类型的模块不能被同时选择

---

### 开题前进度
- 构建了系统的骨架
- 利用设备树感知设备和内存
- 实现 `console` 的抽象

---

## 设计任务: 模块抽象规范与工具
- 为模块设计一个统一的模块抽象
- 为模块提供链接工具


---

## 实现计划

#### 开题 —— 第二周
- 
#### 第二周 —— 中期前后

#### 中期 —— 十二周

#### 十二周 —— 十五周

---

## 参考资料


---
<style scoped>
    section {
        justify-content: center;
        align-items: center;
    }

    section h1, h2, h3, h4, h5, h6 {
        padding: 0;
        margin: 6px;
    }

    section h6 {
        font-weight: normal;
        font-size: 29px;
    }
</style>

# 谢谢