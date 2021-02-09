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

##### 删除的js

```js
function deleteFun(deviceId){
	var newData = "{'deviceId':'"+ deviceId; 
	newData+="','matrix_user_command':'deleteSelect'";
	newData+="}";
	var url=webContextPath+'/matrix.rform?				  matrix_send_request=true&matrix_form_tid='+document.getElementById("	matrix_form_tid").value;
	var synJson = isc.JSON.decode(newData);
	Matrix.confirm("确认删除设备信息？",function(value){
			value = true;
			if(value)
			Matrix.sendRequest(url,synJson,function(){
			Matrix.refreshDataGridData("DataGrid001");})});
}
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
Matrix.success();
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

 

##### ajax

```js
$.ajax({
  //请求方式
  type : "POST",
  //请求的媒体类型
  contentType: "application/json;charset=UTF-8",
  //请求地址
  url : url,
  //数据，json字符串
  data : newData,
  //请求成功
  success : function(result) {
    var resultData=JSON.parse(result);
    if(resultData.status==1){

    }else{
      
    }
  },
  //请求失败，包含具体的错误信息
  error : function(e){
      console.log(e.status);
      console.log(e.responseText);
  }
});
```

