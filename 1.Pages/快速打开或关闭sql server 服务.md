---
Type:
  - Page
aliases: 
tags: []
Status: 
modifiedDate: 2025/06/12, 16:29:33
---

# 快速打开或关闭sql Server 服务

bat文件

```
chcp 65001
set/p ID= 请选择：
if "%id%"=="1" (
sc start mssqlfdlauncher$learn
sc start mssql$learn
sc start mssqllaunchpad$learn
sc start sqlagent$learn
sc start sqlbrowser
sc start sqlpbdms$learn
sc start sqlpbengine$learn
sc start sqltelemetry$learn
sc start sqlwriter
)
if "%id%"=="2" (
sc stop sqlbrowser
sc stop sqlpbdms$learn
sc stop sqlpbengine$learn
sc stop sqltelemetry$learn
sc stop sqlwriter
sc stop mssqllaunchpad$learn
sc stop mssqlfdlauncher$learn
sc stop sqlagent$learn
timeout /t 3 /nobreak >nul
sc stop mssql$learn
)
if "%id%"=="9" exit
pause

```
