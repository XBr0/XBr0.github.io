---
title: "win11修改右键显示win10样式"
date: 2025-09-26T15:07:51+08:00
categories: ["Windows"]
tags: ["右键设置"]
---

本文提供了一个通过修改注册表来切换 Windows 11 和 Windows 10 经典右键菜单的实用技巧。文中包含了恢复经典 Win10 右键菜单和还原 Win11 默认右键菜单的两条命令，读者只需在管理员终端中执行相应命令并重启资源管理器即可生效。

### 切换回经典的 Win10 右键菜单

1.  **以管理员身份打开 Windows 终端**
    
    按 `Win + X` 组合键，在弹出的快捷菜单中，选择 **“Windows 终端 (管理员)”**。

2.  **执行修改命令**
    
    在打开的终端窗口中，复制并粘贴以下命令，然后按回车键：
    
    ```powershell
    reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
    ```
    
    > **提示**：在 Windows 终端中，单击鼠标右键即可快速粘贴。
    
    执行成功后，会显示 “操作成功完成。” 的提示。

3.  **应用更改**
    
    为了使更改生效，你需要重启 “Windows 资源管理器”。最快的方法是：打开任务管理器 (`Ctrl + Shift + Esc`)，在进程列表中找到 “**Windows 资源管理器**”，右键单击并选择 “**重新启动**”。或者直接重启电脑。

### 恢复为 Win11 默认的右键菜单

如果你想换回 Windows 11 默认的右键菜单，只需按照相同的步骤，在管理员终端中执行以下恢复命令即可：

```powershell
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f

