### 常用方法

#### 查询列表

##### 查询列表获取选择行

```js
var row = Matrix.getGridSelections('formDataGrid001');
if(row==undefined||row.length==0){
	Matrix.error("请选择表单");
	return;
}
```
##### 列表伪列调用js

```js
'javascript:funcName(\\''+record+'\\');'
```

##### 刷新列表

```js
Matrix.refreshDataGridData('DataGrid001');
```

##### 获取所有数据

```js
Matrix.getGridData('DataGrid001');
```



#### 窗口

##### 打开窗口

```js
layer.open({
        id: 'editFun',
        type: 2,
        title: ['新增船舶'],
        shade: [0.1, '#000'],
        shadeClose: false, //开启遮罩关闭
        area: ['80%', '80%'],
        content: webContextPath + '/addVesselInfo.rform?mHtml5Flag=true',
    });
```

##### 关闭窗口

```js
Matrix.closeWindow();
```





#### 消息

##### 成功

```js

```

##### 失败

```js
Matrix.error(result.msg);
```

##### 警告

```js

```





#### javaScript

##### json字符串转js对象

```

```

##### js对象转json字符串

``` 
JSON.stringify();
```

