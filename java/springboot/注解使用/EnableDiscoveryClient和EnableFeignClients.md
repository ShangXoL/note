## @EnableDiscoveryClient、@EnableFeignClients注解的使用

### 服务提供：

* 1.配置文件application.properties配置如下：
      
```
spring.application.name=spring-cloud-producer
server.port=9000
eureka.client.serviceUrl.defaultZone=http://localhost:8000/eureka/
```


* 2.启动类中添加@EnableDiscoveryClient注解
 
```
@SpringBootApplication
@EnableDiscoveryClient
public class ProducerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ProducerApplication.class, args);
	}
}

// 添加@EnableDiscoveryClient注解后，项目就具有了服务注册的功能。
// 启动工程后，就可以在注册中心的页面看到SPRING-CLOUD-PRODUCER服务。
```

  
* 3.controller提供hello服务

```
@RestController
public class HelloController {
	
    @RequestMapping("/hello")
    public String index(@RequestParam String name) {
        return "hello "+name+"，this is first messge";
    }
}
```


### 服务调用：

* 1.配置文件application.properties配置如下：
        
```
spring.application.name=spring-cloud-consumer
server.port=9001
eureka.client.serviceUrl.defaultZone=http://localhost:8000/eureka/
```

* 2.启动类启动类添加@EnableDiscoveryClient和@EnableFeignClients注解。
       
```
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class ConsumerApplication {

	public static void main(String[] args) {
		SpringApplication.run(ConsumerApplication.class, args);
	}

}


// @EnableDiscoveryClient :启用服务注册与发现
// @EnableFeignClients：启用feign进行远程调用
```

* 3.feign调用实现：

```
@FeignClient(name= "spring-cloud-producer")
public interface HelloRemote {
    @RequestMapping(value = "/hello")
    public String hello(@RequestParam(value = "name") String name);
}
// name:远程服务名，及spring.application.name配置的名称
// 此类中的方法和远程服务中contoller中的方法名和参数需保持一致。
```

* 4.web层调用远程服务，将HelloRemote注入到controller层，像普通方法一样去调用即可。

```
@RestController
public class ConsumerController {

    @Autowired
    HelloRemote HelloRemote;
	
    @RequestMapping("/hello/{name}")
    public String index(@PathVariable("name") String name) {
        return HelloRemote.hello(name);
    }

}
```

附录：
* <a href="http://www.ityouknow.com/springcloud/2017/05/12/eureka-provider-constomer.html">参考资料</a>
