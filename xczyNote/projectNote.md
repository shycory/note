# 项目配置

## Project Settings

#### Project

- Project name:
  - ccic-xczy
- sdk
  - 1.8
- Project language level:
  - 8 //按照jdk8的版本检查代码
- Project compiler output
  - ......../ccic-xczy/classes

#### Modules

- mbpm 
  - Web

> mbpm 配置Sources,及各个文件夹的Mark
>
> Paths:
>
> - Compiler output : 选择默认的 **InHerit project compile output path**
>
> Dependencies: 依赖包

> Web配置web.xml及静态资源路径和需要打包的Source
>
> - Deployment Descriptors
>   - 项目的web.xml路径 
> - Web Resource Directory
>   - Web Resource Diretory
>     - 配置项目的WebContent
>   - Path Relative to Deployment Root 
>     - 配置文件根路径,: /
> - Source Roots
>   - ...\config + ...\src   + ...\WebContent

#### Artifacts

添加Web Application Exploded

-----

## Tomcat 

### Server //横向页签

#### VM options

- On 'Update' action //更新操作
  - 选择Update classes and resources //热部署
- On frame deactivation 
  - Do nothing  //切换页面的操作.

### Deployment //横向页签

添加project settings 中配置的artifacts

- Application context:
  - 访问项目的项目名 /mbpm

#### 最下方的Build

需要存在 Build ... Artifact 没有就添加一个;





