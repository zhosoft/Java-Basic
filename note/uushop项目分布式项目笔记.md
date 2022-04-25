# 技术选型

## 技术架构图

![image-20220408174404065](uushop项目分布式项目笔记.assets\image-20220408174404065.png)

## 技术选型

| 名称               | 说明         |
| ------------------ | ------------ |
| JDK                | JDK 1.8      |
| 开发工具           | IDEA         |
| 数据库             | MySQL、Redis |
| 数据库的客户端工具 | DataGrip     |
| Maven              | 项目管理工具 |
| 消息中间件         | RocketMQ     |
| Linux              | CentOS7      |

| 开发语言      | Java                                                         |
| ------------- | ------------------------------------------------------------ |
| 开发框架      | Spring Cloud+Vue+MyBatis Plus+Spring Data Redis+Spring Boot+Spring Boot RocketMQ |
| 数据库        | MySQL                                                        |
| 缓存数据库    | Redis                                                        |
| 接口用例测试  | Swagger                                                      |
| 登录权限      | JWT                                                          |
| Excel导入导出 | EasyExcel                                                    |
| 短信接口      | 京东万象                                                     |

# 创建工程

pom.xml文件继承关系：父工程->base-service->repository-service->common-service



```xml
<!-- 继承common-service的pom.xml依赖 -->
<parent>
    <artifactId>common-service</artifactId>
    <groupId>com.zhosoft</groupId>
    <version>0.0.1-SNAPSHOT</version>
</parent>
```



```xml
<!-- 引入 common-service、repository-service的java代码 -->
<dependencies>
    <!-- Common Service -->
    <dependency>
        <groupId>com.zhosoft</groupId>
        <artifactId>common-service</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>

    <!-- Repository Service -->
    <dependency>
        <groupId>com.zhosoft</groupId>
        <artifactId>repository-service</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
</dependencies>
```

```
src右键-> Mark Directory as -> unmark...
java右键 -> Mark Directory as -> Sources root
```

# Swagger 

