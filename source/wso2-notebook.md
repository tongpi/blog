感谢您对WSO2的兴趣！

快速介绍，我的名字是哈斯，我将成为您的客户经理。 请随时与我联系，以获得您可能需要的任何帮助。

我想就目前正在进行的项目进行讨论，我很乐意协助您获得适当的支持，并指导您获取正确的信息。
您能否提供以下方面的一些反馈？

[1]您的项目/需求信息以及您打算解决的业务问题？
[2]你能分享一个高层次的用例/架构图吗？
[3]你目前在哪个阶段的项目？
[4]什么时候开发阶段开始，你希望什么时候投入生产？
[5]您是否在寻找商业支持计划？ 如果是这样，你能告诉我该项目的资金来自哪个季度吗？


期待收到你的回复！



感谢你并致以真诚的问候，


===========================================



Hi Wang Feng,

Thank you for your interest in WSO2!

A quick introduction, my name is Hasith and I will be your Account Manager. Please feel free to reach out to me for any help you may need.

I would like to discuss on the project you are currently working on, and I'd be happy to assist you in obtaining the proper level of support and guide you in getting the right information.
Would you be able to provide some feedback on the following areas please?

[1] Information on your project/requirement and the business problem you intend to solve?
[2] Are you able to share a high-level use-case/ architectural diagrams?
[3] What phase of the project are you currently in?
[4] When would the development phase commence and when do you hope to go into production?
[5] Are you looking for a commercial support plan? If so, are you able to give me an indication of which quarter the funding for this project is coming from?


Look forward to hear from you!



Thank you and best regards,


Hasith Pathirage
Account Manager | WSO2 Inc.

Email: hasith@wso2.com
Mobile: (+94)77 777 1131
Web: http://www.wso2.com


# wso2-iot-server启动批处理文件

- 把以下脚本添加到新的文件iot-server-startup.bat中，放到<wso2-iot-server-home>\bin\目录下


```shell
start broker.bat 
rem *** HACK ALERT: Sleep for 20 seconds ***
ping 127.0.0.1 -n 90 > nul
rem ***************************************
start iot-server.bat
ping 127.0.0.1 -n 120 > nul
rem ***************************************
call analytics.bat
pause
```

# 常见问题

> iot-server analytics server console 中报java.lang.NoClassDefFoundError: org/xerial/snappy/SnappyInputStream 异常

> 解决办法：

> Download the snappy-java_1.1.1.7.jar from [here](http://mvnrepository.com/artifact/org.xerial.snappy/snappy-java/1.1.1.7).
> Copy the jar to <IOTS_HOME>\lib directory.
> If WSO2 IoT Server is currently running, restart it to apply the changes.

> 参见： 
> [IoTS/Analytics/Windows] Exception on startup in analytics pack · Issue #1102 · wso2/product-iots · GitHub
> https://github.com/wso2/product-iots/issues/1102

> Installing on Windows - IoT Server 3.1.0 - WSO2 Documentation
> https://docs.wso2.com/display/IoTS310/Installing+on+Windows


# 汉化方法

> 汉化页面方法：
> 修改 
> E:\home\wso2iot-3.2.0\repository\deployment\server\jaggeryapps\devicemgt\app\pages
> E:\home\wso2iot-3.2.0\repository\deployment\server\jaggeryapps\devicemgt\app\units
> 两个文件夹中的.hbs文件
> +
> E:\home\wso2iot-3.2.0\repository\deployment\server\jaggeryapps\devicemgt\error-pages下的文件
> +
> E:\home\wso2iot-3.2.0\repository\deployment\server\devicetypes

> 页面布局修改方法：
> 修改E:\home\wso2iot-3.2.0\repository\deployment\server\jaggeryapps\devicemgt\app\layouts文件夹下的相应文件




