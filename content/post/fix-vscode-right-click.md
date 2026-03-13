---
title: "修复 Windows 右键菜单"使用 VS Code 打开"消失的问题"
date: 2026-03-13T21:01:08+08:00
categories: ["Windows"]
tags: ["vscode"]
---

## 1. 问题描述

右键菜单里的 **"Open with Code"** 消失了。

不用重装，一个 `.reg` 文件即可修复。

---

## 2. 操作步骤

**① 新建文本文件，粘贴以下内容：**

```reg
Windows Registry Editor Version 5.00

; 右键文件夹
[HKEY_CLASSES_ROOT\Directory\shell\VSCode]
@="使用VSCode打开"
"Icon"="D:\\001Software\\015MicrosoftVSCode\\Code.exe"
[HKEY_CLASSES_ROOT\Directory\shell\VSCode\command]
@="\"D:\\001Software\\015MicrosoftVSCode\\Code.exe\" \"%V\""

; 右键文件
[HKEY_CLASSES_ROOT\*\shell\VSCode]
@="使用VSCode打开"
"Icon"="D:\\001Software\\015MicrosoftVSCode\\Code.exe"
[HKEY_CLASSES_ROOT\*\shell\VSCode\command]
@="\"D:\\001Software\\015MicrosoftVSCode\\Code.exe\" \"%1\""

; 右键文件夹空白处
[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode]
@="使用VSCode打开"
"Icon"="D:\\001Software\\015MicrosoftVSCode\\Code.exe"
[HKEY_CLASSES_ROOT\Directory\Background\shell\VSCode\command]
@="\"D:\\001Software\\015MicrosoftVSCode\\Code.exe\" \"%V\""
```

**② 把路径改成你自己的 `Code.exe` 路径**（注意 `\` 要写成 `\\`）

**③ 另存为 `.reg` 文件，编码选 `UTF-16 LE`**

> ⚠️ 如果包含中文，编码必须选 **UTF-16 LE**（有的显示为 `Unicode`），否则会乱码。不想折腾编码的话，把中文改成英文 `Open with VSCode` 即可。

**④ 双击运行，点"是"，搞定 ✅**

---