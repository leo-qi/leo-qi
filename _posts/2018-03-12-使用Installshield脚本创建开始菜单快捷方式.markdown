---
layout : post
title : 使用Installshield脚本创建开始菜单快捷方式
author : "Leo Qi"
Date : 2018-03-13
catalog: true
tags:
    - Windows
    - InstallShield
---

### 1. 示例 ###
记录一下，方便以后学习和参考。
```C#
// Include Ifx.h for built-in InstallScript function prototypes.
#include "Ifx.h"

export prototype ExFn_AddFolderIcon(HWND);
function ExFn_AddFolderIcon(hMSI)
    STRING  szProgramFolder, szItemName, szCommandLine, szWorkingDir;
    STRING  szIconPath, szShortCutKey, szProgram, szParam;
    NUMBER  nIcon, nFlag, nResult;
begin
    // Set the fully qualified name of the StartUp submenu.
    /**
     * FOLDER_STARTUP为开始菜单，"SubMenu Example"为开始菜单的目录
     * FOLDER_DESKTOP为桌面，默认情况下为“公用桌面”。
     *    可使用ProgDefGroupType (PERSONAL);将其设置成个人桌面
     * 删除桌面快捷方式使用 DeleteFolderIcon (FOLDER_DESKTOP, "Notepad Example1");
     * 删除开始菜单快捷方式使用 DeleteFolderIcon (SHELL_OBJECT_FOLDER, "登录配置");
     * SHELL_OBJECT_FOLDER代表着开始菜单中的文件夹与下面这种方式相同.
     * SHELL_OBJECT_FOLDER需要配置（参考官方文档）。
     */
    szProgramFolder = FOLDER_STARTUP ^ "SubMenu Example";

    // Construct the icon's command line property.
    // 设置目标程序，TARGETDIR代表着安装目录
    szProgram  = TARGETDIR + "\\WBConsole\\bin\\AppConfig.exe";
    // 设置程序参数
    szParam    = "-cfg";
    // 文件夹过长或含有空格时增加引号。
    LongPathToQuote (szProgram, TRUE);
    LongPathToShortPath (szParam);
    szCommandLine = szProgram + " " + szParam;

    // Set up the icon's other properties to pass to AddFolderIcon.
    // 设置快捷方式显示的名称
    szItemName = "Notepad Example1";
    szWorkingDir  = TARGETDIR + "\\WBConsole\\bin";
    // 设置快捷方式的图标
    szIconPath    = TARGETDIR + "\\WBConsole\\conf\\Resource\\ProductIcon.ico";;
    nIcon         = 0;
    szShortCutKey = "";
    nFlag         = REPLACE|RUN_MAXIMIZED;

    // Add the icon to the submenu; create the submenu if necessary.
    nResult = AddFolderIcon (szProgramFolder, szItemName, szCommandLine, szWorkingDir, szIconPath, nIcon, szShortCutKey, nFlag);

    // Report the results.
    if (nResult < 0) then
        MessageBox ("AddFolderIcon failed.", SEVERE);
    else
        SprintfBox (INFORMATION, "AddFolderIcon", "%s created successfully.", szItemName);
    endif;

end;
```
