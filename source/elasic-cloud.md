1、注册Elastic云服务器

https://cloud.elastic.co/region/gcp-europe-west1/deployment/f8cf04c727d7412c9faf010344835a66/activity/elasticsearch



ELKForGds   v6.3.0 

用户名：elastic 密码：5RTqxDR4yFwsYmZWBA7Id5xr 

Cloud ID：ELKForGds:ZXVyb3BlLXdlc3QxLmdjcC5jbG91ZC5lcy5pbyRmOGNmMDRjNzI3ZDc0MTJjOWZhZjAxMDM0NDgzNWE2NiQ1NjBjZmI3NWU5Njk0NTdjYTY1MzA4ZTVlNGQxMzY1Mg== 





安装metricbeat用来收集本机数据到elastic中，参见：https://560cfb75e969457ca65308e5e4d13652.europe-west1.gcp.cloud.es.io:9243/app/kibana#/home/tutorial/systemMetrics?_g=()


PS E:\home\metricbeat-6.3.0-windows-x86_64> .\install-service-metricbeat.ps1
.\install-service-metricbeat.ps1 : 无法加载文件 E:\home\metricbeat-6.3.0-windows-x86_64\install-service-metricbeat.ps1
，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http://go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_P
olicies。
所在位置 行:1 字符: 1

+ .\install-service-metricbeat.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
    PS E:\home\metricbeat-6.3.0-windows-x86_64> ./install-service-metricbeat.ps1
    ./install-service-metricbeat.ps1 : 无法加载文件 E:\home\metricbeat-6.3.0-windows-x86_64\install-service-metricbeat.ps1
    ，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http://go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_P
    olicies。
    所在位置 行:1 字符: 1
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ ./install-service-metricbeat.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
    PS E:\home\metricbeat-6.3.0-windows-x86_64> get-executionpolicy
    Restricted
    PS E:\home\metricbeat-6.3.0-windows-x86_64> set-executionpolicy remotesigned
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

执行策略更改
执行策略可帮助你防止执行不信任的脚本。更改执行策略可能会产生安全风险，如 http://go.microsoft.com/fwlink/?LinkID=135170
中的 about_Execution_Policies 帮助主题所述。是否要更改执行策略?
[Y] 是(Y)  [N] 否(N)  [S] 挂起(S)  [?] 帮助 (默认值为“Y”):
PS E:\home\metricbeat-6.3.0-windows-x86_64> .\install-service-metricbeat.ps1

Status   Name               DisplayName
------   ----               -----------
Stopped  metricbeat         metricbeat


PS E:\home\metricbeat-6.3.0-windows-x86_64> metricbeat.exe modules enable system
metricbeat.exe : 无法将“metricbeat.exe”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括
路径，请确保路径正确，然后再试一次。
所在位置 行:1 字符: 1
+ metricbeat.exe modules enable system
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : ObjectNotFound: (metricbeat.exe:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
    ~~~~~~~~~~~~~~

Suggestion [3,General]: 未找到命令 metricbeat.exe，但它确实存在于当前位置。Windows PowerShell 默认情况下不从当前位置加载
命令。如果信任此命令，请改为键入 ".\metricbeat.exe"。有关更多详细信息，请参阅 "get-help about_Command_Precedence"。
PS E:\home\metricbeat-6.3.0-windows-x86_64> .\metricbeat.exe modules enable system
Error initializing beat: error loading config file: yaml: line 77: could not find expected ':'
PS E:\home\metricbeat-6.3.0-windows-x86_64> .\metricbeat.exe modules enable system
Error initializing beat: error loading config file: yaml: line 76: could not find expected ':'
PS E:\home\metricbeat-6.3.0-windows-x86_64> .\metricbeat.exe modules enable system
Module system is already enabled
PS E:\home\metricbeat-6.3.0-windows-x86_64> .\metricbeat.exe setup
Loaded index template
Loading dashboards (Kibana must be running and reachable)
Loaded dashboards
PS E:\home\metricbeat-6.3.0-windows-x86_64> Start-Service metricbeat
PS E:\home\metricbeat-6.3.0-windows-x86_64>





### 我的云kabana

https://560cfb75e969457ca65308e5e4d13652.europe-west1.gcp.cloud.es.io:9243/app/kibana

用户名/密码：wangf/a1b2c3

gdsweb搜索引擎：https://app.swiftype.com/engines/gdsweb/preview





