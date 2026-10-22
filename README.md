# Linux 内核设计实验 - 调度器统计模块

## 仓库说明
本仓库包含实验所需的**初始基线源码**以及**核心修改补丁**。

## 目录结构
- `baseline-ubuntu-5.15.0.tar.gz`：实验基线环境（基于 Ubuntu Linux 5.15.0-191.201 源码）。
- `patches/`：存放各个功能的修改补丁。
  - `sched_pick.patch`：实现了进程被 CFS 调度器选中次数的统计，并暴露 `/proc/<pid>/sched_pick` 接口。

## 使用指南
1. 下载并解压 `baseline-ubuntu-5.15.0.tar.gz`。
2. 在解压出的源码目录下，执行 `git apply patches/sched_pick.patch` 打入补丁。
3. 正常执行 `make` 编译并安装内核即可。