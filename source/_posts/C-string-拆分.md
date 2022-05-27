---
title: C++ string 拆分
date: 2022-05-24 19:41:15
tags: c++
categories: C++基础
---

## 1.find_***   

##  查找单个字符位置

1. find_first_not_of

2. find_first_of

   size_t find_first_of ( const string& str, size_t pos = 0 ) const;
   size_t find_first_of ( const char* s, size_t pos, size_t n ) const;
   size_t find_first_of ( const char* s, size_t pos = 0 ) const;
   size_t find_first_of ( char c, size_t pos = 0 ) const;



1. find_last_of() 字符串从后往前第一个指定字符的位置 ，返回int值位置
2. find_first_not_of  字符串从后往前，第一个与指定字符不同的位置，返回int值位置

```c++
// string::find_first_not_of
#include <iostream>       // std::cout
#include <string>         // std::string
#include <cstddef>        // std::size_t

int main ()
{
  std::string str ("look for non-alphabetic characters...");

  std::size_t found = str.find_first_not_of("abcdefghijklmnopqrstuvwxyz ");

  if (found!=std::string::npos)
  {
    std::cout << "The first non-alphabetic character is " << str[found];
    std::cout << " at position " << found << '\n';
  }

  return 0;
}

————》》》
The first non-alphabetic character is - at position 12


```

## 2. substr

```c++
// string::substr
#include <iostream>
#include <string>
int main ()
{
std::string str="We think in generalities, but we live in details.";
// (quoting Alfred N. Whitehead)
std::string str2 = str.substr (3,5); // "think"
std::size_t pos = str.find("live"); // position of "live" in str
std::string str3 = str.substr (pos); // get from "live" to the end
std::cout << str2 << ' ' << str3 << '\n';
return 0;
}
```



```c++
// string::find_first_not_of
#include <iostream>       // std::cout
#include <string>         // std::string
#include <cstddef>        // std::size_t

 int main()
 {

	 //std::string path = "D:/mydoc/VS-proj/SMTDetector/x64/Release/0001.bmp";
	 std::string path = "D:\\mydoc\\VS-proj\\SMTDetector\\x64\\Release\\0001.bmp";

	int index = path.find_last_of("\\");

	std::string folderPath = path.substr(0, index);
	std::string filename = path.substr(index+1, -1);

	int index2 = path.find_last_of("."); //
	std::string extendName = path.substr(index2 + 1, -1);

	std::cout << "path:\t" << path << std::endl; //path:	D:\mydoc\VS-proj\SMTDetector\x64\Release\0001.bmp
	std::cout << "folderPath:\t" << folderPath << std::endl;//folderPath:	D:\mydoc\VS-proj\SMTDetector\x64\Release
	std::cout << "filename:\t" << filename << std::endl;// filename:	0001.bmp
	std::cout << "extendName:\t" << extendName << std::endl;// bmp

	 return 0;
 }

```

## 3.replace

指定字符串中替换的的字符位置，替换长度，将要替换的的

```
string& replace (size_t pos,  size_t len,  const string& str);
pos:替换字符串的起始位置
len:原始字符串中要被替换掉字符串的界限[pos,len]
str：新替换的字符串
```

```c++
// replacing in a string
#include <iostream>
#include <string>

int main ()
{
  std::string base="this is a test string.";
  std::string str2="n example";
  std::string str3="sample phrase";
  std::string str4="useful.";

  // replace signatures used in the same order as described above:

  // Using positions:                 0123456789*123456789*12345
  std::string str=base;           // "this is a test string."
  str.replace(9,5,str2);          // "this is an example string." (1)
  str.replace(19,6,str3,7,6);     // "this is an example phrase." (2)
  str.replace(8,10,"just a");     // "this is just a phrase."     (3)
  str.replace(8,6,"a shorty",7);  // "this is a short phrase."    (4)
  str.replace(22,1,3,'!');        // "this is a short phrase!!!"  (5)

  // Using iterators:                                               0123456789*123456789*
  str.replace(str.begin(),str.end()-3,str3);                    // "sample phrase!!!"      (1)
  str.replace(str.begin(),str.begin()+6,"replace");             // "replace phrase!!!"     (3)
  str.replace(str.begin()+8,str.begin()+14,"is coolness",7);    // "replace is cool!!!"    (4)
  str.replace(str.begin()+12,str.end()-4,4,'o');                // "replace is cooool!!!"  (5)
  str.replace(str.begin()+11,str.end(),str4.begin(),str4.end());// "replace is useful."    (6)
  std::cout << str << '\n';
  return 0;
}
```

