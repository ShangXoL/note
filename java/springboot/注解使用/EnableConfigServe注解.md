### @EnableConfigServer注解的使用介绍

* 应用场景：使用Spring Cloud构建统一配置中心

* Spring Cloud Config项目是一个解决分布式系统的配置管理方案。<br>
  它包含了Client和Server两个部分。

## server端配置：
【Spring Cloud Config Server本质上也是一个Spring Boot的web项目，只需要添加对应的parent，然后加入相关的依赖就可以启动这个工程了】

* 1.pom.xml

```
<parent>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-parent</artifactId>
    <version>Brixton.BUILD-SNAPSHOT</version>
    <relativePath /> <!-- lookup parent from repository -->
</parent>

<dependencies>

    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-config-server</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-eureka</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>

<!-- repositories also needed for snapshots and milestones -->

<repositories>
    <repository>
        <id>spring-snapshots</id>
        <name>Spring Snapshots</name>
        <url>http://repo.spring.io/libs-snapshot-local</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
    <repository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>http://repo.spring.io/libs-milestone-local</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
    <repository>
        <id>spring-releases</id>
        <name>Spring Releases</name>
        <url>http://repo.spring.io/libs-release-local</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </repository>
</repositories>
<pluginRepositories>
    <pluginRepository>
        <id>spring-snapshots</id>
        <name>Spring Snapshots</name>
        <url>http://repo.spring.io/libs-snapshot-local</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </pluginRepository>
    <pluginRepository>
        <id>spring-milestones</id>
        <name>Spring Milestones</name>
        <url>http://repo.spring.io/libs-milestone-local</url>
        <snapshots>
            <enabled>false</enabled>
        </snapshots>
    </pluginRepository>
</pluginRepositories>

```

* 2.config server的resource目录下的application.properties

```
server.port=8888

## 配置环境仓库
spring.cloud.config.server.git.uri=file://Users/whthomas/config-repo

spring.application.name=configserver
spring.cloud.config.uri=http://localhost:8888

```

* 3.启动类

```
@SpringBootApplication
@EnableConfigServer
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## client端：

* 1.pom.xml

```
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-dependencies</artifactId>
      <version>Brixton.RC2</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
    
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-starter-config</artifactId>
    </dependency>
    
  </dependencies>
</dependencyManagement>

```

* 2.配置文件

```
# 配置中心服务的地址
spring.cloud.config.uri=localhost:8888
# 要读取的配置文件application属性
spring.cloud.config.name=cloud-config
# 要读取的配置文件profile属性,默认是dev
spring.cloud.config.profile=${config.profile:dev}

```

* 以上配置完成之后，在远端配置中心的对应的配置就会加载到项目中，<br>
    和本地使用application.properties配置中添加配置是几乎一样的效果，<br>
    使用@Value注解的配置也可以顺利读取到对应的配置。
  



