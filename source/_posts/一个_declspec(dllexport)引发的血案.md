---
title:  一个_declspec(dllexport)引发的血案
date: 2022-05-24 19:41:15
tags: c++
typora-root-url: ..
categories: ++基础
---

#  1.vs无法解析的外部符号常见解决方法


visaul Studio 经常遇到   "error LNK2019: 无法解析的外部符号 **，该符号在函数 **中被引用"的报错

常见原因是，没有加载到，动态库和静态库的.lib 文件，可能没有生成，或者路径不对。常规修改属性中的这连个配置。

![image-20220526153101516](/images/%E4%B8%80%E4%B8%AA_declspec(dllexport)%E5%BC%95%E5%8F%91%E7%9A%84%E8%A1%80%E6%A1%88/image-20220526153101516.png)

![image-20220524123658895](/images/%E4%B8%80%E4%B8%AA_declspec(dllexport)%E5%BC%95%E5%8F%91%E7%9A%84%E8%A1%80%E6%A1%88/image-20220526153159453.png)

# 2. 缺少**_declspec(dllexport)** 声明出现的两个报错

## 1.LNK1104	无法打开文件“**.lib”

在**导出函数在声明和定义**时，都一定要有关键字 **_declspec(dllexport)** ，这样才会同时生成 .dll 和 .lib 文件。

例如：

vs创建两个项目，GenDll项目生成动态库，UseDll项目使用GenDll中的函数，其中GenDll项目中，class A没有声明**_declspec(dllexport)**  ，在生成的动态库，只会生成dll，不会生成.lib文件。在编译UseDll报错，无法链接到GenDll.lib

```c++
//项目GenDll

//GenDll.h
class A
{
public:
	int res(int a, int b); 
};

//GenDll.cpp
#include "GenDll.h"

int A::add(int a,int b) {
	return a + b; 
}
```

```c++
//项目UseDll

//main.cpp
#include <iostream>
#include "../../GenDll/Gendll/GenDll.h"

int main(int argc, char* argv[])
{
	A a;
	std::cout << "100+1=" << a.add(100, 1) << std::endl;
	system("pause");
	return 0;
}

//严重性	代码	说明	项目	文件	行	禁止显示状态
//错误	LNK1104	无法打开文件“GenDll.lib”	DllExample	C:\Users\liujiapeng01\Desktop\DllExample\DllExample\LINK	1	

```

将class A 改为 class  __declspec(dllexport) A ，就可以正常运行不会报错。

## 2. LNK2019无法解析外部符合***

对上面的项目进行修改，增加一个other class.

```c++
//项目GenDll

//GenDll.h
class __declspec(dllexport) A
{
public:
	int res(int a, int b); 
};

class  other {
public:
	int res(int a, int b);
};

//GenDll.cpp
#include "GenDll.h"

int A::add(int a,int b) {
	return a + b; 
}

int other::res(int a, int b) {
	return a + b;
}
```

```c++
//项目UseDll

//main.cpp
#include <iostream>
#include "../../GenDll/Gendll/GenDll.h"

int main(int argc, char* argv[])
{
	A a;
	other o;
	std::cout << "100+1=" << a.res(100, 1) << std::endl;
	std::cout << "100+1=" << o.res(100, 1) << std::endl;
	return 0;
}

//main.obj : error LNK2019: 无法解析的外部符号 "public: int __thiscall other::res(int,int)" (?res@other@@QAEHHH@Z)，该符号在函数 _main 中被引用1>C:\Users\liujiapeng01\Desktop\DllExample\Debug\DllExample.exe : fatal error LNK1120: 1 个无法解析的外部命令

```

从错误中可以看出，无法解析other::res, 原因是项目GenDll 虽然生成了GenDll.lib，但是GenDll.lib中只有，Class A的链接信息，没有Class other的，因为Class A做了__declspec(dllexport)修饰，Class B没有做修饰。



## 3.总结 

__declspec(dllexport)可以修饰Class 也可以单独修饰一个函数，缺少修饰，其他项目在调用动态库的过程中都会缺少链接信息报错。
