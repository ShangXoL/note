1. SpringCLoud中的“Discovery Service”有多种实现，比如：eureka, consul, zookeeper。

2. @EnableDiscoveryClient注解是基于spring-cloud-commons依赖，并且在classpath中实现； 
3. @EnableEurekaClient注解是基于spring-cloud-netflix依赖，只能为eureka作用；

4. 如果你的classpath中添加了eureka，则它们的作用是一样的。