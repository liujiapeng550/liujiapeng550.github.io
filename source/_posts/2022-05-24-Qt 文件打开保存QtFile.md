---
title: Qt 文件打开保存
date: 2022-05-24 19:41:15
tags: c++
categories: Qt
---

```python
from PyQt5.QtWidgets import QFileDialog
from shutil import copyfile

def select(self,filepathname, isrelpath=False):
		if isrelpath:
			filepathname = self.get_file_abspath(filepathname)

		filepath = os.path.dirname(filepathname)

		selectfile, options = QFileDialog.getOpenFileName(
			None, "选择材质", filepath, "材质文件 (*.mtg)"
		)

        pass



def save_as(self,filepathname, isrelpath=False):
		if not filepathname:
			return
		#
		if isrelpath:
			filepathname = self.get_file_abspath(filepathname)
		#

		#if os.path.isfile(filepath):
		filepath = os.path.dirname(filepathname)
		suffix = os.path.splitext(filepathname)[-1]

		filepath_new, options = QFileDialog.getSaveFileName(
			None, "另存为", filepath, suffix
		)
		if not filepath_new:
			return

		try:
			copyfile(filepathname, filepath_new)
		except IOError as e:
			print("Unable to copy file. %s" % e)
		pas
```

