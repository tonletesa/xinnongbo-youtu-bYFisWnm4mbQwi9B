> 字数 1444，阅读大约需 8 分钟

# 如何在 Windows 中使用 Kimi CLI

## 目录

* • 如何在 Windows 中使用 Kimi CLI[1]
  + • 目录[2]
  + • 第一步：安装 Windows 子系统[3]
    - • 方法一：图形界面安装[4]
    - • 方法二：PowerShell 命令安装[5]
  + • 第二步：安装 Linux 子系统[6]
    - • 方法一：Microsoft Store 安装[7]
    - • 方法二：PowerShell 命令安装[8]
    - • 初始设置[9]
  + • 第三步：配置 Windows 和子系统网络互通[10]
    - • 为什么要配置网络互通？[11]
    - • 配置步骤[12]
  + • 第四步：进入 Linux 子系统并安装 Kimi CLI[13]
    - • 1. 进入 Linux 子系统[14]
    - • 2. 安装 Kimi CLI[15]
    - • 3. 启动 Kimi CLI[16]
    - • 4. 配置 Kimi CLI[17]
  + • 第五步：Kimi CLI 与 Visual Studio/Visual Studio Code 同时开发[18]
  + • 常见问题与解决方案[19]
    - • Q: 网络代理不生效[20]
    - • Q: Kimi CLI 安装失败[21]
  + • 总结[22]

---

## 第一步：安装 Windows 子系统

### 方法一：图形界面安装

1. 1. 按 `Win + Q` 打开搜索，搜索"启用或关闭 Windows 功能"
2. 2. 在打开的窗口中勾选以下选项：
   * • ✅ 适用于 Linux 的 Windows 子系统
   * • ✅ Hyper-V
   * ![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160148489-1441324172.png)

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160159203-250726637.png)

### 方法二：PowerShell 命令安装

以管理员身份打开 PowerShell，执行以下命令：

```
# 1. 启用 Hyper-V 全套组件
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All -NoRestart

# 2. 启用"适用于 Linux 的 Windows 子系统"功能
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux -NoRestart

# 3. 把 WSL 更新到最新内核（微软商店版，推荐）
wsl --update          # 会自动下载安装 Store 版 WSL
```

> ⚠️ **注意**：安装完成之后需要重启系统

---

## 第二步：安装 Linux 子系统

### 方法一：Microsoft Store 安装

1. 1. 打开 Microsoft Store
2. 2. 搜索 "Ubuntu 22.04.5 LTS"
3. 3. 点击"获取"或"安装"按钮

### image方法二：PowerShell 命令安装

1. 1. 以管理员身份打开 PowerShell
2. 2. 查询可安装的 Linux 发行版：

```
wsl --list --online
```

1. ![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160227005-1061166185.png)
2. 3. 安装所需的发行版（例如 Ubuntu 22.04）：

```
wsl --install Ubuntu-22.04
```

### 初始设置

* • 如果使用 Store 安装，直接打开应用即可自动初始化
* ![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160241511-376280458.png)
* • 如果使用 `wsl --install` 命令，系统会自动完成安装过程
* • 安装完成后，设置用户名和密码

## 第三步：配置 Windows 和子系统网络互通

### 为什么要配置网络互通？

如果不进行此配置，Windows 的代理设置在 Linux 子系统中将不可用，可能导致网络访问问题。

### 配置步骤

1. 1. 进入当前用户文件夹：

```
%USERPROFILE%
```

1. ![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160312824-569073036.png)
2. 2. 找到并编辑 `.wslconfig` 文件（如果不存在则创建）
3. 3. 添加以下配置内容：

```
# 全局 WSL2 配置文件，路径：%USERPROFILE%\.wslconfig
# 修改后需执行 wsl --shutdown 再重启 WSL 才会生效

[wsl2]
# 是否启用调试控制台（true=开启，false=关闭）
debugConsole=false

# 以下字段属于实验性功能，需要 Win11 22H2+ 以及 WSL 2.0.0+
[experimental]
# 网络模式： mirrored=镜像主机网络，默认 NAT
networkingMode=mirrored

# DNS 隧道： true=让 WSL 通过主机解析 DNS，避免国内常见 DNS 污染
dnsTunneling=true

# 让 Windows 防火墙规则对 WSL 生效（默认 false）
firewall=true

# 自动继承 Windows 系统代理设置
autoProxy=true
```

1. 4. 保存文件后，在 PowerShell 中执行以下命令重启 WSL：

```
wsl --shutdown
```

---

## 第四步：进入 Linux 子系统并安装 Kimi CLI

### 1. 进入 Linux 子系统

打开 PowerShell 或 CMD，输入：

```
wsl
```

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160445496-1611368838.png)

成功进入子系统后，切换到用户主目录：

```
cd ~
```

### 2. 安装 Kimi CLI

参考文档：Kimi CLI 使用说明 | Kimi For Coding[23]

首先安装 uv（Python 包管理器）：

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

安装 uv 后，使用以下命令安装 Kimi CLI：

```
uv tool install --python 3.13 kimi-cli
```

验证安装是否成功：

```
kimi --version
```

### 3. 启动 Kimi CLI

```
kimi
```

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160502454-1098527230.png)

### 4. 配置 Kimi CLI

在 Kimi CLI 中输入：

```
/setup
```

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160534414-1678903267.png)

选择第一个选项，输入 API 密钥。

API 密钥获取方式：

1. 1. 访问 Kimi 官网
2. 2. 登录账户
3. 3. 在设置或开发者页面找到 API 密钥

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160540652-1859821918.png)

![image](https://img2024.cnblogs.com/blog/1687216/202510/1687216-20251029160544339-524247365.png)

将复制的密钥粘贴到 Kimi CLI 中，配置即完成。

---

## 第五步：Kimi CLI 与 Visual Studio/Visual Studio Code 同时开发

在 Linux 子系统中，导航到 Windows 项目目录：

```
cd /mnt/c/project
# mnt 是共享的文件夹 # c 是你的 C 盘，d 是你的 D 盘 # project 是你的开发文件夹
```

这样就可以在 Windows 中使用 Visual Studio/Visual Studio Code 进行开发，同时在 Linux 子系统中使用 Kimi CLI 提供智能编程辅助，实现高效的跨平台开发体验。

---

## 常见问题与解决方案

### Q: 网络代理不生效

A: 检查 `.wslconfig` 文件中的 `autoProxy=true` 设置是否正确，并确保已重启 WSL。

### Q: Kimi CLI 安装失败

A: 确保已正确安装 uv 和 Python 3.13，并检查网络连接是否正常。

---

## 下课

本博客参考[ssrdog](https://waling.org)。转载请注明出处！
