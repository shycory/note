# 自定义服务

## 配置返回信息

### 1. 保存按钮的配置

> 高级->isPostRefresh->填写 false

>事件->onpostclick->填写 xxxx(data);  //xxxx方法名

### 2. 在源码中写js

> 实现上一步的方法.data为resultEntity的json数据

> 使用js的JSON.parse(str)转为js对象-->取值

> 判断返回状态,1-成功,0失败,-1系统繁忙

> 重置按钮,将保存按钮恢复到可点击
>
> $("#id_button").removeAttr("disabled");//将按钮设置为可用
>
> id_button为保存按钮的id属性

### 3. 后台代码

> 配置全局变量

```java
private ToolMgr toolMgr = MFormContext.getService("ToolMgr");
```

> 在方法中创建 ResultEntity实体.使具体情况设置参数:

```java
ResultEntity resultEntity=new ResultEntity();
        resultEntity.setStatus(ResultEnum.FAIL.getCode());
        resultEntity.setMsg("test");
```

> 在最后调用服务方法:

``` java
 toolMgr.outputData(resultEntity);
```

**注:该方法的if条件中添加了ResultEntity类的判断.并且重写了该类的toString方法.若要使用其他类型,如outputData(str)直接传字符串;**

