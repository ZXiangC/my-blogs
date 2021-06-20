## 一、nacos 简介

​        Nacos 是阿里巴巴推出来的一个新开源项目，是一个更易于构建云原生应用的动态服务发现、配置管理和服务管理平台。Nacos 致力于帮助您发现、配置和管理微服务。Nacos 提供了一组简单易用的特性集，帮助您快速实现动态服务发现、服务配置、服务元数据及流量管理。Nacos 帮助您更敏捷和容易地构建、交付和管理微服务平台。 Nacos 是构建以“服务”为中心的现代应用架构 (例如微服务范式、云原生范式) 的服务基础设施。

## 二、nacos 下载安装

**（1）下载地址和版本**

下载地址：https://github.com/alibaba/nacos/releases

下载版本：nacos-server-1.1.4.tar.gz或nacos-server-1.1.4.zip，解压任意目录即可

### 1、Windows 启动nacos服务

- （1）修改配置文件 mode :standalone

  ![image-20210615004035748](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615004035748.png)

- （2）启动

  ![image-20210615003900792](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615003900792.png)

  - nacos 启动有点慢

![image-20210615004331619](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615004331619.png)

- （3）访问：http://localhost:8848/nacos

​     - 用户名密码：nacos/nacos



![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/61a73801-aa89-43d2-ae67-1f66e9e862e2.png)

![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/70fbe767-f4fa-4c31-ac9f-b6b5adba6377.png)



### 2、Linux启动nacos服务

- Linux/Unix/Mac

启动命令(standalone代表着单机模式运行，非集群模式)

启动命令：sh startup.sh -m standalone

## 三、nacos注册中心

**1、Spring Cloud Config**

Spring Cloud Config 为分布式系统的外部配置提供了服务端和客户端的支持方案。在配置的服务端您可以在所有环境中为应用程序管理外部属性的中心位置。客户端和服务端概念上的Spring Environment 和 PropertySource 抽象保持同步, 它们非常适合Spring应用程序，但是可以与任何语言中运行的应用程序一起使用。当应用程序在部署管道中从一个开发到测试直至进入生产时，您可以管理这些环境之间的配置，并确保应用程序在迁移时具有它们需要运行的所有内容。服务器存储后端的默认实现使用git，因此它很容易支持标记版本的配置环境，并且能够被管理内容的各种工具访问。很容易添加替代的实现，并用Spring配置将它们插入。

Spring Cloud Config 包含了Client和Server两个部分，server提供配置文件的存储、以接口的形式将配置文件的内容提供出去，client通过接口获取数据、并依据此数据初始化自己的应用。Spring cloud使用git或svn存放配置文件，默认情况下使用git。



**2、Nacos替换Config**

Nacos 可以与 Spring, Spring Boot, Spring Cloud 集成，并能代替  **Eureka**, **Spring Cloud Config**。通过 Nacos Server 和 spring-cloud-starter-alibaba-nacos-config 实现配置的动态变更。

**（1）应用场景**

在系统开发过程中，开发者通常会将一些需要变更的参数、变量等从代码中分离出来独立管理，以独立的配置文件的形式存在。目的是让静态的系统工件或者交付物（如 WAR，JAR 包等）更好地和实际的物理运行环境进行适配。配置管理一般包含在系统部署的过程中，由系统管理员或者运维人员完成。配置变更是调整系统运行时的行为的有效手段。

如果微服务架构中没有使用统一配置中心时，所存在的问题：

- 配置文件分散在各个项目里，不方便维护

-  配置内容安全与权限

-  更新配置后，项目需要重启

nacos配置中心作用：

- 系统配置的集中管理（编辑、存储、分发）
- 动态更新不重启
- 回滚配置（变更管理、历史版本管理、变更审计）等所有与配置相关的活动。







**（2）**常见的注册中心：

\1. Eureka（原生，2.0遇到性能瓶颈，停止维护）

\2. Zookeeper（支持，专业的独立产品。例如：dubbo）

\3. Consul（原生，GO语言开发）

\4. Nacos

相对于 Spring Cloud Eureka 来说，Nacos 更强大。Nacos = Spring Cloud Eureka + Spring Cloud Config

 Nacos 可以与 Spring, Spring Boot, Spring Cloud 集成，并能代替 Spring Cloud Eureka, Spring Cloud Config

\- 通过 Nacos Server 和 spring-cloud-starter-alibaba-nacos-discovery 实现服务的注册与发现。



**（3）**Nacos是以服务为主要服务对象的中间件，Nacos支持所有主流的服务发现、配置和管理。

Nacos主要提供以下四大功能：

\1. 服务发现和服务健康监测

\2. 动态配置服务

\3. 动态DNS服务

\4. 服务及其元数据管理

**（4）**Nacos结构图

![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/2bed8eac-e227-4224-a6cc-5fb634420a24/128/index_files/6e5b55f7-3252-4dea-81e9-e0ffd86987b4.jpg)

### 2、案例



#### 1）、在service模块配置pom

配置Nacos客户端的pom依赖

 

```java
<!--服务注册-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>
```

#### 2）、添加服务配置信息

配置application.properties，在客户端微服务中添加注册Nacos服务的配置信息

 

```java
# nacos服务地址
spring.cloud.nacos.discovery.server-addr=127.0.0.1:8848
```

#### **3）、添加Nacos客户端注解**

在客户端微服务启动类中添加注解

 

```java
@EnableDiscoveryClient
```

#### **4）、启动客户端微服务**

启动注册中心

启动已注册的微服务，可以在Nacos服务列表中看到被注册的微服务

![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/2bed8eac-e227-4224-a6cc-5fb634420a24/128/index_files/e61822a5-f8db-4ea3-b54e-df6ee00b886e.png)

## 四、Nacos配置中心

### 1、配置文件移动到 nacos 中

将application.properties 配置文件移动到nacos中

- **第一步：填写Data ID**

Data ID 的完整规则格式如下

```properties
${prefix}-${spring.profile.active}.${file-extension}
```

- *prefix* 默认为所属工程配置spring.application.name 的值（即：service-edu），也可以通过配置项 spring.cloud.nacos.config.prefix来配置。
- *spring.profiles.active*=dev 即为当前环境对应的 profile。 注意：当 spring.profiles.active 为空时，对应的连接符 - 也将不存在，dataId 的拼接格式变成 ${prefix}.${file-extension}
- *file-exetension* 为配置内容的数据格式 以 properties 或者 yaml 结尾。

- **第二步：填写分组 ：**

  分组名称自己写，但是需要在配置文件中填写名称不填就是默认分组

  ```properties
  spring.cloud.nacos.config.group=guli
  ```

- **第三步：配置文件移入**

案例：

- 1.点击配置管理-配置列表-点击 + 号

![image-20210615001130989](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615001130989.png)

- 2.添加配置文件

![image-20210615001409433](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615001409433.png)



![image-20210615001531991](https://gitee.com/ZXiangC/picture/raw/master/imgs/image-20210615001531991.png)

### 2、添加依赖，配置

添加依赖，创建bootstrap.properties配置文件

- (1) 添加依赖

```java
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>
```

- (2) 创建配置文件：bootstrap.properties

```java
#配置中心地址
spring.cloud.nacos.config.server-addr=127.0.0.1:8848

#配置环境
spring.profiles.active=dev

# 该配置影响统一配置中心中的dataId
spring.application.name=service-edu

# 配置命名空间
#spring.cloud.nacos.config.namespace=26bec7ed-2658-43bb-a07f-c850af9dd245
# 配置分组 (不配置默认default)
spring.cloud.nacos.config.group=guli
```

### 3、把项目之前的内容注释

把项目之前的application.properties内容注释

**springboot配置文件加载顺序**

其实yml和properties文件是一样的原理，且一个项目上要么yml或者properties，二选一的存在。推荐使用yml，更简洁。

bootstrap与application
**（1）加载顺序**
这里主要是说明application和bootstrap的加载顺序。

- bootstrap.yml（bootstrap.properties）先加载
- application.yml（application.properties）后加载
- bootstrap.yml 用于应用程序上下文的引导阶段。

- bootstrap.yml 由父Spring ApplicationContext加载。

父ApplicationContext 被加载到使用 application.yml 的之前。

**（2）配置区别**

- bootstrap.yml 和application.yml 都可以用来配置参数。

- bootstrap.yml 可以理解成系统级别的一些参数配置，这些参数一般是不会变动的。

- application.yml 可以用来定义应用级别的。





### 4、名称空间切换环境

在实际开发中，通常有多套不同的环境（默认只有public），那么这个时候可以根据指定的环境来创建不同的 namespce，例如，开发、测试和生产三个不同的环境，那么使用一套 nacos 集群可以分别建以下三个不同的 namespace。以此来实现多环境的隔离。

**1、创建命名空间**

**![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/19a76d33-7bd1-4f45-a3c7-458a1f37288d.png)**



**默认只有public，新建了dev、test和prod命名空间**



​	![img](https://gitee.com/ZXiangC/picture/raw/master/imgs/eb62bd69-9b4f-4e3e-8f5f-db13891ae546.png)

**2、克隆配置**

**（1）切换到配置列表：**

![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/9863c44f-2253-4b05-9811-43312b4dffcd/128/index_files/f53e8c0a-6b7a-43ea-9aa5-6e98f9c20b0f.png)
        

​        可以发现有四个名称空间：public（默认）以及我们自己添加的3个名称空间（prod、dev、test），可以点击查看每个名称空间下的配置文件，当然现在只有public下有一个配置。默认情况下，项目会到public下找 服务名.properties文件。接下来，在dev名称空间中也添加一个nacos-provider.properties配置。这时有两种方式：

- 第一，切换到dev名称空间，添加一个新的配置文件。缺点：每个环境都要重复配置类似的项目
- 第二，直接通过clone方式添加配置，并修改即可。推荐

**![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/9863c44f-2253-4b05-9811-43312b4dffcd/128/index_files/46582e35-11b0-4946-98fe-6c182a23a4f6.png)点击编辑：修
**



```
spring.cloud.nacos.config.server-addr=127.0.0.1:8848

spring.profiles.active=dev

# 该配置影响统一配置中心中的dataId，之前已经配置过
spring.application.name=service-statistics

spring.cloud.nacos.config.namespace=13b5c197-de5b-47e7-9903-ec0538c9db01
```

- 命名空间 id

**![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/9863c44f-2253-4b05-9811-43312b4dffcd/128/index_files/a2eeac43-a089-452a-9228-7fbf23527ded.png)



### 5、多配置文件加载

在一些情况下需要加载多个配置文件。假如现在dev名称空间下有三个配置文件：service-statistics.properties、redis.properties、jdbc.properties



![img](C:/Users/%E7%A5%A5%E5%AD%90/Documents/My%20Knowledge/temp/9863c44f-2253-4b05-9811-43312b4dffcd/128/index_files/aa2fe27d-fc96-430c-93c7-159b33bdd23e.png)

**添加配置，加载多个配置文件**



```java
spring.cloud.nacos.config.server-addr=127.0.0.1:8848

spring.profiles.active=dev

# 该配置影响统一配置中心中的dataId，之前已经配置过
spring.application.name=service-statistics

spring.cloud.nacos.config.namespace=13b5c197-de5b-47e7-9903-ec0538c9db01

spring.cloud.nacos.config.ext-config[0].data-id=redis.properties
# 开启动态刷新配置，否则配置文件修改，工程无法感知
spring.cloud.nacos.config.ext-config[0].refresh=true
spring.cloud.nacos.config.ext-config[1].data-id=jdbc.properties
spring.cloud.nacos.config.ext-config[1].refresh=true
```

