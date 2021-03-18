# Java 注解学习

## java基础注解

> 标注在注解上的注解,称为元注解

***

### `@Target` 

- 表示该注解的使用位置

> @Target(`ElementType.TYPE`)——接口、类、枚举、注解
> @Target(`ElementType.FIELD`)——字段、枚举的常量
> @Target(`ElementType.METHOD`)——方法
> @Target(`ElementType.PARAMETER`)——方法参数
> @Target(`ElementType.CONSTRUCTOR`) ——构造函数
> @Target(`ElementType.LOCAL_VARIABLE`)——局部变量
> @Target(`ElementType.ANNOTATION_TYPE`)——注解
> @Target(`ElementType.PACKAGE`)——包

***

### `@Retention`

- 注解的保留时间/生命周期

> `RetentionPolicy.SOURCE`:只在源代码级别保留,编译时就会被忽略
> `RetentionPolicy.CLASS`:编译时被保留,默认的保留策略,在class文件中存在,但JVM将会忽略,运行时无法获得。
> `RetentionPolicy.RUNTIME`:被JVM保留,能在运行时被JVM或其他使用反射机制的代码所读取和使用。

***

### `@Document`

- 加入文档注解

> javadoc中默认没有注解,想保留注解信息,在注解中添加`@Document`

### `@Inherited`

- 放在注解上,可被继承注解

> 父类标注的注解中含有`@Inherited` , 则子类继承也会继承该注解

### `@Repeatable`

- java注解---元注解
  - 被标注的注解,可以在同一个地方多次使用

## sprint-boot

### `@SpringBootApplication`

> -----> spring-boot的注解都是基于spring等其他框架注解的组合;

````java
@SpringBootApplication
	@SpringBootConfiguration //仅有Component注解的功能
		@Configuration//与spring-application.xml的配置文件功能相同
			@Component//组件,将实例放入spring容器中.
	@EnableAutoConfiguration //
		@AutoConfigurationPackage
			@Import(AutoConfigurationPackages.Registrar.class)
		@Import(AutoConfigurationImportSelector.class)
	@ComponentScan(includeFilters,excludeFilters)//spring注解---扫描
		//扫描当前注解所在类 的包下所有文件
			//使用 includeFilters 来按照规则只包含某些包的扫描
			//使用 excludeFilters 来按照规则排除某些包的扫描
````

#### `@SpringBootConfiguration`

springboot的配置注解,继承自@Configuration

##### 	`@Configuration`

​	与spring-application.xml的配置文件功能相同,通常和@Bean(创建对象)联用.

#### `@EnableAutoConfiguration`

启用自动配置

##### 			`@AutoConfigurationPackage`

###### 					`@Import(Registrar.class)` 

​			作用是将 **添加该注解的类所在的package** 作为 **自动配置package** 进行管理

##### 	`	@Import(AutoConfigurationImportSelector.class)`

```
getCandidateConfigurations(annotationMetadata, attributes);//获取所有配置
configurations = removeDuplicates(configurations);//去掉重复配置
classLoader.getResources("META-INF/spring.factories")//默认扫描这个文件下的所有 配置类
```



​	 作用是将springboot写好的一堆默认配置导入,且过滤掉没有引入某些功能的默认配置类

### `@Conditional`

​	条件注解.满足条件,才会....

### `@importResource`

若项目中存在xml配置文件,可以使用该注解引入

### `@ConfigurationProperties`

将springboot中的配置文件 中配置的变量绑订java类上 .作用和@Value类似,

需要与`@Component`连用,

或者在 配置类上使用`@EnableConfigurationProperties(Test.class)`

```java
@ConfigurationProperties(prefix="test")
public class Test(){
	private String name;
	public String get(){return this.name} 
    public void set(String name){this.name=name}
}
application.yml中
	test:
		name: 哈哈
```

### 

