---
title: 辅助代码生成服务CodeWhisperer
date: 2024-02-19 15:02:31
tags:
---

## 简介 CodeWhisperer

Amazon CodeWhisperer支持多种**编程语言**，包括Python、Java、JavaScript、TypeScript、C#、Go、Rust、PHP、Ruby、Kotlin、C、C++、Shell 脚本、SQL、Scala、JSON、YAML和HCL等。

CodeWhisperer支持的**IDE**包括VS Code、IntelliJ IDEA、Visual Studio、PyCharm、WebStorm和Rider等。此外，CodeWhisperer还支持MacOS下的终端/Iterm2用于CLI交互。

Amazon CodeWhisperer目前提供**个人版（免费）**和专业版（收费）。使用CodeWhisperer的个人版不需要AWS账号，也不需要配置AWS的Access Key，只需要使用邮箱免费注册AWS Builder ID平台即可。

## 为VS Code配置 CodeWhisperer

- VScode 插件名称: AWS Toolkit
  
注：免费版CodeWhisperer需要邮箱注册

1. 在vscode插件市场中搜索关键字AWS Toolkit，注意发布者是经过认证的Amazon Web Services官方.

2. 在右侧的位置选中第一项Amazon Q + CodeWhisperer，点击下方的Use for free with AWS Builder ID
   
3. 此时弹出窗口，提示要通过浏览器注册和登录

4. 点击打开链接启动浏览器

5. 如果此前注册过，这点击Already have AWS Builder ID

6. 邮箱验证码登录

7. 可以看到CODEWHISPERER菜单下，代码完成Auto-Suggestions功能已经是打开状态。

## 使用 CodeWhisperer


现在开始编写一段代码，为了让代码自动提示，需要两个前提：

VS Code当前编辑的文件，不能是未保存的文件，也不能是文本文件，需要有文件扩展名，例如test.py，这样CodeWhisperer即可根据扩展名识别语言

可以适当增加注释，注释请用英文简单编写，例如Upload a file to s3，或者Create EMR task等
此时按下默认的快捷键ALT+C，即可看到代码补全出现。按TAB键，即可将代码补全结果打在屏幕上。

在补全一行后，代码还会继续补全。

> 关闭使用用户数据进行服务质量改进
> 新安装完毕后，服务质量改进的选项是默认打开的，这里可以通过设置界面将其关闭。
> 在插件中，找到AWS Toolkit插件，点击齿轮符号进入设置界面。然后点击Extension Settings按钮。
> 从设置界面中找到Share Code Whisperer Content With AWS，这个选项默认是选中的，将选中取消，即可关闭服务质量改进功能。

参考地址: https://blog.bitipcman.com/codewhisperer/