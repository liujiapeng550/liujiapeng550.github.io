---
title: 2022-09-02-ElementTree修改
date: 2022-09-02 18:07:45
tags:
---

先对XML的格式做一些说明：

- **Tag**： 使用<和>包围的部分，如<rank>成为start-tag，</rank>是end-tags；

- **Element**：被Tag包围的部分，如<rank>68</rank>，可以认为是一个节点，它可以有子节点；

- **Attribute**：在Tag中可能存在的name/value对，如<country name="Liechtenstein">中的name="Liechtenstein"，一般表示属性。

  

  下面所有的操作都将下面这段XML为例，我们将它保存为**sample.xml**。

```xml

	<SubMesh>
		<Sub0
			Name = "otherback01"
			MtlIdx = "0"
			BoundingCenter = "-8.379703,134.129578,19.337904"
			BoundingHalf = "8.946298,12.765850,6.331268"
			RenderGroup = "0"
			RenderOffset = "0"
			ShadowBias = "0.000000"
			ShadowNormalBias = "0.000000"
			IsSkin4S = "false" />
	</SubMesh>
```

例如test.xml 如上文件修改上面的xml文件
```python
#解析xml
import xml.etree.ElementTree as ET
tree = ET.parse('sample.xml')
root = tree.getroot()

```

```python
#修改SubMesh为0_SubMesh
root.tag =  0_SubMesh
#修改Name的attrib为back
root.find(Sub0).attrib["Name"] = "back"
```

