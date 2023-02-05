---
title: char[]，char *，string之间转换
date: 2022-05-24 20:05:15
tags: c++
---

[char[]，char *，string之间转换](https://blog.csdn.net/yzhang6_10/article/details/51164300)

## 1. char []与char *之间转换

  ### char []转char *：直接进行赋值即可

```c++
// char[] 转char *
char str[] = "lala";
char *str1 = str;
cout << str1 << endl;

```

 ### char *转char[]：字符拷贝实现，不能进行赋值操作

```c++
// char *转换为char []
const char *st = "hehe";
char st1[] = "lalalala";
strncpy(st1, st, strlen(st) + 1);    // 注意加1操作 
// tp = temp;                        //错误，不能实现 
cout << st1 << endl; 

```

## 2. char 与const char 之间转换

### const char 转char ：拷贝实现，不能进行赋值

```c++
// const char *转char *
const char *st = "lala";
// 直接赋值不可以 
//char *st1 = st;           // （不可以编译器报错） 
//cout << st1 << endl;
// 另外开辟空间，将字符一个一个复制过去
char *ncstr = new char[strlen(st) + 1];
strcpy(ncstr, st);
cout << ncstr << endl; 
```
### char 转const char ：直接进行赋值
```c++
// char *转const char * 
char *st = "hehe";         // （编译提示警告）
const char *st1 = st;
cout << st1 << endl; 

```

## 3. char *与string之间转换
### char *转string：1）直接赋值；2）构造转换实现
```c++
// char*转换为string
// （注意，定义char *变量，并直接赋值，最好定义为const变量，否则编译器警告） 
const char *st = "hello";
// 赋值转换 
string st1 = st;
cout << st1 << endl;
// 构造转换 
string s1(st, st + strlen(st));
cout << s1 << endl;
// 改变const char *变量值 
st = "lalala";
cout << st << endl; 
```

### string转char *：赋值操作（注意类型转换）
```c++
// string转char *
string st = "My test";
//char *st1 = st;           // 错误类型不同 
//char *st1 = st.c_str();   // 错误类型不同
char *st1 = const_cast<char *>(st.c_str()) ;
cout << st1 << endl; 
```

## 4. char[]与string之间转换

### char []转string：1）直接赋值；2）构造转换实现
```c++
// char[]转换为string 
char st[] = "hello";   
// 直接赋值实现 
string st1 = st;
cout << st1 << endl;
// 构造实现 
string st2(st, st + strlen(st));
cout << st2 << endl;
```
### string转char[]：拷贝实现，不能直接赋值
```c++
// string转char []
string ts = "My test1";
//char ts1[] = ts;          // 错误
//char ts1[] = const_cast<char *>(ts.c_str());   // 错误 
char ts1[] = "lalallalalaaaa";
strncpy(ts1, ts.c_str(), ts.length() + 1);       // 注意，一定要加1，否则没有赋值'\0' 
cout << ts1 << endl; 
return 0;
```
***总结***

***涉及到char []字符数组与其它类型转换，一般需要进行拷贝，不能直接赋值实现。char []和char *都可以通过构造新的string完成其对string的转换。涉及到到char *转换，需要注意类型一致，同时注意const的使用。***