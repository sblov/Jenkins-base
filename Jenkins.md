# Jenkins

**持续部署 | 持续集成 | 持续交付**

## 持续集成工具

### Jenkins & Hudson

​	**目前最流行的持续集成及自动化部署工具**

​	2009年，甲骨文收购Sun并继承了Hudson代码库。在2011年初，甲骨文和开源社区之间关系破裂，该项目被分为两个独立的项目：

​	**Jenkins：由大部分原始开发人员组成**

​	**Hudson：由甲骨文公司继续管理**

### 技术组成

​	**Jenkins与Hudson都能整合GitHub与Subversion**

## 自动化部署图示

![1557811694799](img\1557811694799.png)

## Jenkins+SVN

### 实例系统结构

> **版本控制系统**
>
> |------Subversion服务器
>
> |------项目对应版本库
>
> |------版本库中钩子程序
>
> **持续集成系统**
>
> |------JDK
>
> |------Tomcat
>
> |------Maven
>
> |------Jenkins
>
> ​	|-----主程序
>
> ​	|-----SVN插件
>
> ​	|-----Maven插件
>
> ​	|-----Deploy to Web Container 插件
>
> **应用发布系统**
>
> |------JDK
>
> |------Tomcat

### 版本控制系统

​	**配置版本库访问账号密码**

​	**在`svnserve.conf`中将 anon-access 设置为 none** 

​	版本库钩子脚本 `$svn/repository/项目库/hooks/post-commit.tmpl`（提交后的钩子脚本），重命名去掉后缀名，注释其中内容，编辑：

```sh
curl -X post -v -u jenkins用户名:密码 jenkinsURL/job/项目名/build?token=设置的token
```

### 应用发布系统

​	**配置tomcat，由于是程序访问项目，需要设置tomcat服务器的账号与密码，提高安全**

​	**`$tomcat/conf/tomcat-users.xml`**

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>

<user username="tomcat_user" password="123456" roles="manager-gui,manager-script,manager-jmx,manager-status"/>
```

### Jenkins安装配置

​	**1、将 jenkins.war 放在 tomcat 中**

​	**2、修改 `$tomcat/conf/server.xml 的url`地址的编码解码字符集**

```xml
 <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" URIEncoding="UTF-8" />
```

​	==**具体插件、token、tomcat、svn、maven、项目、构建配置参照官方文档**==

## Jenkins+GitHub

**注意点：**

> 1、Jenkins要部署到外网，内网地址github是无法访问的
>
> 2、Jenkins所在主机安装Git
>
> 3、指定Git程序位置，与jdk，maven程序指定类似
>
> 4、在GitHub上使用每个repository的WebHook方式远程触发Jenkins构建
>
> 5、Jenkins内关闭 “ 防止跨站点请求伪造”