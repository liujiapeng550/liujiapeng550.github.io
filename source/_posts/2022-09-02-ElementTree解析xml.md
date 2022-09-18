---
title: 2022-09-02-ElementTree解析xml
date: 2022-09-02 10:28:09
tags:
---

https://www.jianshu.com/p/cf6fa10045a6

## 一、引用方法

ElementTree所在文件保存在Lib/xml/etree/ElementTree.py，所以我们通过下面的代码引用它，之后就可以使用ET.来访问ElementTree中的函数。



```python
import xml.etree.ElementTree as ET
```

## 二、一个XML例子

下面所有的操作都将下面这段XML为例，我们将它保存为**sample.xml**。



```xml
<?xml version="1.0"?>
<data>
    <country name="Liechtenstein">
        <rank>1</rank>
        <year>2008</year>
        <gdppc>141100</gdppc>
        <neighbor name="Austria" direction="E"/>
        <neighbor name="Switzerland" direction="W"/>
    </country>
    <country name="Singapore">
        <rank>4</rank>
        <year>2011</year>
        <gdppc>59900</gdppc>
        <neighbor name="Malaysia" direction="N"/>
    </country>
    <country name="Panama">
        <rank>68</rank>
        <year>2011</year>
        <gdppc>13600</gdppc>
        <neighbor name="Costa Rica" direction="W"/>
        <neighbor name="Colombia" direction="E"/>
    </country>
</data>
```

先对XML的格式做一些说明：

- **Tag**： 使用<和>包围的部分，如<rank>成为start-tag，</rank>是end-tags；
- **Element**：被Tag包围的部分，如<rank>68</rank>，可以认为是一个节点，它可以有子节点；
- **Attribute**：在Tag中可能存在的name/value对，如<country name="Liechtenstein">中的name="Liechtenstein"，一般表示属性。



## 三、解析XML

### 读入XML数据

首先读入XML，有两种途径，从文件读入和从字符串读入。
 从文件读入：



```python
import xml.etree.ElementTree as ET
tree = ET.parse('sample.xml')
root = tree.getroot()
```

从字符串读入：



```python
root = ET.fromstring(sample_as_string)
```

tree和root分布是ElementTree中两个很重要的类的对象：

- ElementTree
- Element

### 查看Tag和Attribute

这时得到的root是一个指向<data>Element对象，我们可以通过查看root的tag和attrib来验证这一点：



```python
>>> root.tag
'data'
>>> root.attrib
{}
```

上面的代码说明了查看一个Element的Tag和Attribute的方法，Tag是一个*字符串*，而Attribute得到的是一个*字典*。

另外，还可以使用

- Element.get(AttributeName)

来代替Element.attrib[AttributeName]来访问。

### 查看孩子

root.attrib返回的是一个空字典，如果看root的孩子，可以得到非空的attrib字典。

#### 1、使用for...in...访问



```python
for child in root:
    print child.tag, child.attrib
```

得到

> country {'name': 'Liechtenstein'}
>  country {'name': 'Singapore'}
>  country {'name': 'Panama'}

#### 2、使用下标访问

如:



```python
>>> print root[0].tag
country
>>> print root[0][0].tag
rank
```

#### 3、使用Tag名称访问

下标访问的方法虽然简单，但是在未知XML具体结构的时候并不适用，通过Tag名称访问的方法更具有普适性。这里用到Element类的几个函数，分别是

- Element.iter()
- Element.findall()
- Element.find()

这两个函数使用的场景有所差异：
 **Element.iter()**用来寻找**所有**符合要求的Tag，注意，这里查找的范围**是所有孩子和孩子的孩子 and so on**。如果查看所有的year，可以使用下面的代码：



```python
for neighbor in root.iter('year'):
    print neighbor.text
```

返回

> 2008
>  2011
>  2011

**Element.findall()**只查找**直接的孩子**，返回所有符合要求的Tag的Element，而**Element.find()**只返回符合要求的第一个Element。如果查看Singapore的year的值，可以使用下面的代码：



```python
for country in root.findall('country'):
        if country.attrib['name'] == 'Singapore':
            year = country.find('year')  # 使用Element.find()
            print year.text
```



```python
for country in root.findall('country'):
        if country.attrib['name'] == 'Singapore':
            years = country.findall('year')  # 使用Element.findall()
            print years[0].text  # 注意和上段的区别
```

### 查看Element的值

我们可以直接用Element.text来得到这个Element的值。

## 四、修改XML

前面已经介绍了如何获取一个Element的对象，以及查看它的Tag、Attribute、值和它的孩子。下面介绍如何修改一个Element并对XML文件进行保存

### 修改tag

```python
#修改SubMesh为0_SubMesh
root.tag =  0_SubMesh
```
### 修改attrib

```
#修改Name的attrib为back
root.find(Sub0).attrib["Name"] = "back"
```

### 修改Element

修改Element可以直接访问Element.text。
 修改Element的Attribute，也可以用来新增Attribute：

> Element.set('AttributeName','AttributeValue')

新增孩子节点：

> Element.append(childElement)

删除孩子节点：

> Element.remove(childElement)

### 保存XML

我们从文件解析的时候，我们用了一个ElementTree的对象tree，在完成修改之后，还用tree来保存XML文件。



```python
tree.write('output.xml')
```

### 构建XML

ElementTree提供了两个静态函数（直接用类名访问，这里我们用的是ET）可以很方便的构建一个XML，如：



```python
root = ET.Element('data') 
    country = ET.SubElement(root,'country', {'name':'Liechtenstein'})
    rank = ET.SubElement(country,'rank')
    rank.text = '1'
    year = ET.SubElement(country,'year')
    year.text = '2008'
    ET.dump(root)
```

就可以得到

> <data><country name="Liechtenstein"><rank>1</rank><year>2008</year></country></data>

## 五、XPath支持

XPath表达式用来在XML中定位Element，下面给一个例子来说明：



```python
import xml.etree.ElementTree as ET

root = ET.fromstring(countrydata)

# Top-level elements
root.findall(".")

# All 'neighbor' grand-children of 'country' children of the top-level
# elements
root.findall("./country/neighbor")

# Nodes with name='Singapore' that have a 'year' child
root.findall(".//year/..[@name='Singapore']")

# 'year' nodes that are children of nodes with name='Singapore'
root.findall(".//*[@name='Singapore']/year")

# All 'neighbor' nodes that are the second child of their parent
root.findall(".//neighbor[2]")
```





