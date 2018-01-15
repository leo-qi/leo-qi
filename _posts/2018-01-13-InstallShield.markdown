---
layout : post
title : 使用InstallShield安装软件后注册到Windows系统服务
author : "Leo Qi"
Date : 2018-01-13
catalog: true
tags:
    - InstallShield
    - Windows
---

1. 编写Windows Service程序
Windows系统服务启动与停止需要符合特定的规范，利用Mircosoft Visual Studio 建立Windows Service程序。在.h文件中找到OnStart()和OnStop()方法，写入相关代码：
```C#
public ref class WBAnalysisWinService : public System::ServiceProcess::ServiceBase
	{
	public:
		WBAnalysisWinService()
		{
			InitializeComponent();
			//
			//TODO: Add the constructor code here
			//
		}
	protected:
        // 自己添加
		System::Diagnostics::Process^ process;
		/// <summary>
		/// Clean up any resources being used.
		/// </summary>
		~WBAnalysisWinService()
		{
			if (components)
			{
				delete components;
			}
		}

		/// <summary>
		/// Set things in motion so your service can do its work.
		/// </summary>
		virtual void OnStart(array<String^>^ args) override
		{
            // 当前程序的执行路径
			String^ path = System::Environment::CommandLine;
            // 调试日志，后续去掉
			System::Diagnostics::EventLog^ log = gcnew System::Diagnostics::EventLog();
			log->Source = "我的应用程序";
			log->WriteEntry(path, System::Diagnostics::EventLogEntryType::Information);

            // 可以调用bat，此处调用bat未做测试
			//String tempPath = System::Environment::CurrentDirectory;
			//String fileName = Path::Combine ( tempPath, "startup.bat"); 
			//System::Diagnostics::Debugger::Launch();

            // 通过此程序调用Python时，Python的路径有问题，使用全路径，不使用此方法
            // process = System::Diagnostics::Process::Start("cmd.exe","/k " + sb->ToString());
			process =gcnew System::Diagnostics::Process();
			StringBuilder^ sb = gcnew StringBuilder(path->Substring(0, path->LastIndexOf(L"\\")));
			String^ pyPath = sb->ToString();
			sb->Append(L"\\..\\python\\python.exe ");
			process->StartInfo->FileName = sb->ToString();
			process->StartInfo->Arguments = pyPath + "\\..\\WBAnalysis\\AysServer.py";
			process->Start();
		}

		/// <summary>
		/// Stop this service.
		/// </summary>
		virtual void OnStop() override
		{
            // 结束程序
			process->Kill();
		}
```
2. 编写InstallShield脚本
在installShield的Installation Designer视图中，选择Behavior and Logic->InstallScripts双击进入详情页面，选择FeatureEvents.rul，在右侧代码编辑框的上侧的第一个下拉列表框选择Move Data下的相应的自己的模块，后在第二个下拉列表框选择Installed，代码框会出现相关函数。
```shell
export prototype WBAnalysis_Installed();
function WBAnalysis_Installed()
STRING szAys;
NUMBER nvLineNumber;
begin
    // 注册服务
	SetStatusWindow (80, "正在安装服务UEM Analysis Services...");
	if (!ServiceExistsService("WBAnalysis")) then
		szAys = TARGETDIR + "\\WBAnalysis\\bin\\WBAnalysis.exe";
		if (ServiceAddService("WBAnalysis", "UEM Analysis Services", "", szAys, FALSE, "") < ISERR_SUCCESS) then
			SprintfBox(SEVERE, "Error", "安装UEM Analysis Services服务失败", SEVERE); 
		endif;
	endif;

    // 启动服务
    if (ServiceExistsService("WBAnalysis")) then 
		SetStatusWindow (90, "启动UEM Analysis Services服务，请稍候...");   
   		if (ServiceGetServiceState("WBAnalysis", nvLineNumber) >= ISERR_SUCCESS) then
   			if (nvLineNumber = SERVICE_STOPPED) then 
   				ServiceStartService ("WBAnalysis", "");
   			endif;
   		else   
			ServiceStartService ("WBAnalysis", "");   
		endif; 
	endif;
end;
```
3. 与模块相关联
选择Organization->SetUp Design，找到相应的模块在右侧详情页面，在Feature Events下的OnInstalled中选择上一步创建的函数即可。
