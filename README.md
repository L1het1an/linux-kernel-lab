# Linux 内核设计实验

## 仓库说明
本仓库用于存放 Linux 内核实验的**初始基线获取方式**以及**各自代码修改补丁（Patch）**。
为了避免大文件导致仓库臃肿，完整的源码包请前往 Release 页面下载。

## 📌 一、 成员工作流（如何获取环境并提交代码）
### 1. 获取初始源码（基线）
1. 在本 GitHub 仓库页面右侧，点击 **Releases**。
2. 下载名为 `baseline-ubuntu-5.15.0.tar.gz` 的附件。
3. 在 Ubuntu 虚拟机中解压该源码包：
   ```bash
   tar -xzf baseline-ubuntu-5.15.0.tar.gz
   ```
   *解压后会得到 `baseline-ubuntu-5.15.0` 目录，这是完全纯净的初始内核源码。*
### 2. 修改与测试代码
在 `baseline-ubuntu-5.15.0` 目录下进行你自己的代码修改、编译并测试。
*(注意：大家尽量修改不同的内核模块文件，防止后续合并补丁时产生冲突)*
### 3. 提取自己的修改（生成 Patch）
测试通过后，生成属于你的补丁文件：
```bash
cd baseline-ubuntu-5.15.0
# 【极度重要】先清理编译产物，防止把中间文件打进补丁
make clean
# 生成补丁文件（文件名建议用自己的名字+功能，如 member_a_mem.patch）
git diff > ../member_a.patch
```
### 4. 提交你的补丁到仓库
**步骤一：传输补丁**
将刚才生成的 `member_a.patch` 通过 VMware 共享文件夹或 VS Code 下载到 Windows 宿主机。
**步骤二：推送至 GitHub**
在宿主机（Windows）上，克隆本仓库并提交你的补丁：
```bash
# 1. 克隆仓库（如果还没克隆的话）
git clone https://github.com/L1het1an/linux-kernel-lab.git
cd linux-kernel-lab
# 2. 将你的补丁放入 patches 文件夹
# 把刚才下载的 member_a.patch 复制到该目录下
# 3. 提交并推送
git add patches/member_a.patch
git commit -m "feat: 提交xxx的修改补丁"
git push origin master
```
---
## 🚀 二、 仓管工作流（如何合并所有人成果）
当所有成员都把自己的 `.patch` 文件推送到仓库后，仓管需要进行最终合并与全量编译：
**1. 拉取仓库最新内容：**
```bash
git pull origin master
```
**2. 在纯净源码目录中，依次应用所有补丁：**
```bash
cd baseline-ubuntu-5.15.0
git apply ../patches/member_a.patch
git apply ../patches/member_b.patch
git apply ../patches/member_c.patch
```
*(如果有冲突，按 Git 提示手动修改对应文件即可)*
**3. 全量编译最终版内核：**
```bash
make -j$(nproc)
sudo make modules_install
sudo make install
```
---
## 📂 仓库目录结构
- `README.md`：本说明文档。
- `patches/`：存放所有成员的代码修改补丁。