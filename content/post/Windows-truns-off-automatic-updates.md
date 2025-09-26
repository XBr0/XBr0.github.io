---
title: "Windows禁止/恢复系统自动更新"
date: 2025-09-26T14:54:44+08:00
categories: ["Windows"]
tags: ["Windows","系统更新"]
---

本文提供了两段 Windows 批处理脚本，用于一键禁止或恢复系统的自动更新功能，解决了 Windows 强制更新带来的困扰。

### Win禁止更新系统
禁止更新系统.bat
```powershell
@echo off
:: Kingfall Script: Disable Windows 11 Update
:: Version 1.0
:: IMPORTANT: This script must be run as an administrator.

echo.
echo ==================================================
echo         正在停止并禁用 Windows 更新服务...
echo ==================================================
echo.

:: 停止 Windows Update 服务 (wuauserv)
net stop wuauserv
:: 停止 Update Orchestrator 服务 (UsoSvc)
net stop UsoSvc
:: 停止 Background Intelligent Transfer Service (BITS)
net stop BITS

echo.
echo 正在配置服务启动类型为“禁用”...
echo.

:: 禁用 Windows Update 服务
sc config wuauserv start=disabled
:: 禁用 Update Orchestrator 服务
sc config UsoSvc start=disabled
:: 禁用 Background Intelligent Transfer Service (BITS)
sc config BITS start=disabled

echo.
echo 正在通过注册表强化禁用策略...
echo.

:: 添加注册表项以禁止自动更新
reg add "HKEY_LOCAL_MACHINE/SOFTWARE/Policies/Microsoft/Windows/WindowsUpdate/AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f

echo.
echo =======================================================
echo  **成功！Windows 更新相关服务已被彻底禁用。**
echo =======================================================
echo.
pause
```
### Win恢复更新系统
恢复更新系统.bat
```powershell
@echo off
:: Kingfall Script: Enable Windows 11 Update
:: Version 1.0
:: IMPORTANT: This script must be run as an administrator.

echo.
echo ==================================================
echo         正在恢复 Windows 更新服务...
echo ==================================================
echo.

echo 正在配置服务启动类型为"自动"...
echo.

:: 恢复 Windows Update 服务启动类型
sc config wuauserv start=auto
:: 恢复 Update Orchestrator 服务启动类型
sc config UsoSvc start=auto
:: 恢复 Background Intelligent Transfer Service (BITS)
sc config BITS start=delayed-auto


echo.
echo 正在启动相关服务...

echo.

:: 启动 Windows Update 服务 (wuauserv)
net start wuauserv
```
