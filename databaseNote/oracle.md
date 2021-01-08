# Oracle Note

## 遇到的问题

##### ojdbc问题

在navicate等软件可多条语句执行,但在java中调用ojdbc执行sql语句时,一次只能执行一条sql语句.

所以用java执行ddl语句时需要注意分开写,不要图省事写在一个方法里...

##### 批量插入问题

与mysql的多条插入不同

````sql
insert all
	into table_name (column1,column2,....) values (value1,value2,...)
	into table_name (column1,column2,....) values (value1,value2,...)
	...
select 1 from dual;    //这个select 不加报错.为啥要加还没搞清楚.
````

> mybatis写法

```xml
INSERT ALL
 <foreach collection="data" item="obj">
     INTO ${tableName}
     <foreach collection="obj.keySet()" item="entry" open="(" separator="," close=")">
         "${entry}"
     </foreach>
     VALUES
     <foreach collection="obj.keySet()" item="key" open="(" separator="," close=")">
         #{obj.${key}}
     </foreach>
 </foreach>
 SELECT 1 FROM DUAL
```

##### 添加字段问题

oracle添加字段只能在表的末尾加,不能像mysql调节顺序;

##### 建表字段不能加""

建表时,字段不能用双引号包含,因为在查询时直接查会报错.需用select * 或者select "字段名" 这种方式,显而易见,这是个bug.但是oracle会让这么建表...