## 强力推荐
[java 架构视频](http://www.mqyjq.com/topics/video/27) <br/>
[java 视频](http://www.mqyjq.com/topics/video/17)

# 搭建一个简单的RESTfull API接口项目

## github demo地址
https://github.com/csyeva/springboot-eva/tree/master/spring-boot-learn-1

## 代码解析

IndexController

```java
@RestController
@RequestMapping(value = "/index")
public class IndexController {

    @RequestMapping
    public String index() {
        return "hello world2..";
    }

    // @RequestParam 简单类型的绑定，可以出来get和post
    @RequestMapping(value = "/get")
    public HashMap<String, Object> get(@RequestParam String name) {
        HashMap<String, Object> map = new HashMap<String, Object>();
        map.put("title", "hello eva");
        map.put("name", name);
        return map;
    }

    // @PathVariable 获得请求url中的动态参数
    @RequestMapping(value = "/get/{id}/{name}")
    public User getUser(@PathVariable int id, @PathVariable String name) {
        User user = new User();
        user.setId(id);
        user.setName(name);
        user.setDate(new Date());
        return user;
    }

}

```

### SpringBootDemo41Application

```java
@SpringBootApplication
public class SpringBootDemo41Application {

    public static void main(String[] args) {
        SpringApplication.run(SpringBootDemo41Application.class, args);
    }
}
```

## 启动项目
直接运行main方法或者使用maven命令: spring-boot:run

```java
测试： http://localhost:8080/index
带参数：http://localhost:8080/index/get?name=wujing
带参数有中文：http://localhost:8080/index/get?name=eva
url测试：http://localhost:8080/index/get/1/eva
url测试：http://localhost:8080/index/get/1/eva

```

## 打包
命令: clean package

# 配置文件详解：Properties和YAML

## 配置文件的生效顺序，会对值进行覆盖

>* 1. @TestPropertySource 注解
>* 2. 命令行参数
>* 3. Java系统属性（System.getProperties()）
>* 4. 操作系统环境变量
>* 5. 只有在random.*里包含的属性会产生一个RandomValuePropertySource
>* 6. 在打包的jar外的应用程序配置文件（application.properties，包含YAML和profile变量）
>* 7. 在打包的jar内的应用程序配置文件（application.properties，包含YAML和profile变量）
>* 8. 在@Configuration类上的@PropertySource注解
>* 9. 默认属性（使用SpringApplication.setDefaultProperties指定）


## 配置随机值

```java
roncoo.secret=${random.value}
roncoo.number=${random.int}
roncoo.bignumber=${random.long}
roncoo.number.less.than.ten=${random.int(10)}
roncoo.number.in.range=${random.int[1024,65536]}

读取使用注解：@Value(value = "${roncoo.secret}")

```

## 属性占位符

```xml
当application.properties里的值被使用时，它们会被存在的Environment过滤，所以你能够引用先前定义的值（比如，系统属性）。
roncoo.name=www.roncoo.com
roncoo.desc=${roncoo.name} is a domain name

```

## Application属性文件，按优先级排序，位置高的将覆盖位置低的
1. 当前目录下的一个/config子目录
2. 当前目录
3. 一个classpath下的/config包
4. classpath根路径（root）

> 这个列表是按优先级排序的（列表中位置高的将覆盖位置低的）


## 配置应用端口和其他配置的介绍

```xml
#端口配置：
server.port=8090
#时间格式化
spring.jackson.date-format=yyyy-MM-dd HH:mm:ss
#时区设置
spring.jackson.time-zone=Asia/Chongqing
```

## 使用YAML代替Properties

```xml
注意写法：冒号后要加个空格
```

# 配置文件-多环境配置

## 多环境配置的好处
>* 不同环境配置可以配置不同的参数
>* 便于部署，提高效率，减少出错

## Properties多环境配置

```xml
1. 配置激活选项
spring.profiles.active=dev

2.添加其他配置文件
application.properties
application-dev.properties
application-prod.properties
application-test.properties
```
## YAML多环境配置

```xml
1.配置激活选项
	spring:
	profiles:
active: dev
	2.在配置文件添加三个英文状态下的短横线即可区分
	---
spring:
		profiles: dev


```

## 两种配置方式的比较

>* Properties配置多环境，需要添加多个配置文件，YAML只需要一个配件文件
>* 书写格式的差异，yaml相对比较简洁，优雅
>* YAML的缺点：不能通过@PropertySource注解加载。如果需要使用@PropertySource注解的方式加载值，那就要使用properties文件。

## 如何使用

```xml
java -jar myapp.jar --spring.profiles.active=dev
```




