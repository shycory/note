# Java 注解学习

### java基础注解

> 标注在注解上的注解,称为元注解

***

`@Target` 

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

`@Retention`

- 注解的保留时间/生命周期

> `RetentionPolicy.SOURCE`:只在源代码级别保留,编译时就会被忽略
> `RetentionPolicy.CLASS`:编译时被保留,默认的保留策略,在class文件中存在,但JVM将会忽略,运行时无法获得。
> `RetentionPolicy.RUNTIME`:被JVM保留,能在运行时被JVM或其他使用反射机制的代码所读取和使用。

***

`@Document`

- 加入文档注解

> javadoc中默认没有注解,想保留注解信息,在注解中添加`@Document`

***

`@Inherited`

- 放在注解上,可被继承注解

> 父类标注的注解中含有`@Inherited` , 则子类继承也会继承该注解

***



### sprint-boot

#### 概览

````java
@SpringBootApplication
	@SpringBootConfiguration
		@Configuration
			@Component
	@EnableAutoConfiguration
		@AutoConfigurationPackage
			@Import(AutoConfigurationPackages.Registrar.class)
		@Import(AutoConfigurationImportSelector.class)
	@ComponentScan(includeFilters,excludeFilters)
		@Retention
		
````

***

#### 作用

> spring-boot的注解都是基于spring等其他框架注解的组合;

`@Configuration`

- spring注解---配置类
  - 与spring-application.xml的配置文件功能相同

- `@Component`
  - spring注解---组件,将实例放入spring容器中.
    - 与在xml中配置bean实体功能相同;
    - 有三个子注解,分别为`@Controller`,`@Service`,`@Repository`;
      - 其中`@Controller`又有子注解`@RestController`

***

`@ComponentScan`

- spring注解---扫描
  - 扫描当前注解所在类 的包下所有文件
    - 使用 includeFilters 来按照规则只包含某些包的扫描
    - 使用 excludeFilters 来按照规则排除某些包的扫描
- `@Repeatable`
  - java注解---元注解
    - 被标注的注解,可以在同一个地方使用相同的注解

***

`@EnableAutoConfiguration`

- spring-boot注解---自动配置

- `@AutoConfigurationPackage`

  - spring-boot注解---自动导入配置包
  - `@Import(AutoConfigurationPackages.Registrar.class)`
    - spring注解---导入,导入配置包

- `@Import(AutoConfigurationImportSelector.class)`

  - spring注解---导入配置选择器

  

