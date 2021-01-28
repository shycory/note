# 导出

### 用到的类

1.ChangeDataTypeUtil   //转换数据结构

2.ExcelUtil 导出Excel的工具类

3.ExportFileEnum	//导出本地模板的文件名和路径

---

### 流程简介

前端按钮点击事件

​	-->ajax

​		-->后端Controller

后端方法中先查询数据

​	(由于用dataService搜索出的结构都是DataObject类型)

​	--->转换数据类型为Map<String,Object>(可加前缀)

​		--->详情数据的list直接转为list<map<String,Object>>

​			----> 将list转换后的list放入主信息的map中,**键为模板中的loop名;**

调用excelUtil的expFopExcel方法;



​				