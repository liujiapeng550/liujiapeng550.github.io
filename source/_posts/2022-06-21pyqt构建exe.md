---
title: pyqt构建exe
date: 2022-06-21 10:52:56
tags:
---

### 1. 单文件打包

单文件打包会将整个项目和相关依赖都打包进一个exe，
此时**一般只需要发送exe文件给别人即可正常运行。**
它的缺点是：**启动相对另一种打包方式更缓慢**。
输入命令：

```
 pyinstaller -F -w -i .\windowIco1.ico .\imageBrowser.py
```

**参数解释：**
-F :单文件打包
-w:不要console(取消类似于cmd的黑框框)
-i:后面接图标地址（图标一定要是标准的ico格式）
我用的是：`.\windowIco1.ico`
最后面接的是要打包的程序：`.\imageBrowser.py`

运行命令成功后：

会生成三个文件夹和一个`.spec`文件，前两个文件夹是没用的（建议删除~）。exe在dist文件夹中。
![在这里插入图片描述](../images/pyqt%E6%9E%84%E5%BB%BAexe/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RiX3lvdXRo,size_16,color_FFFFFF,t_70.png)
双击运行exe：
