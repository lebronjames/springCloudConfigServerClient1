SpringCloud分布式配置中心springCloudConfigServer的Client客户端
1) pom.xml增加Config Client(spring-cloud-starter-config)、Eureka、
Web(spring-boot-starter-web)、监控(spring-boot-starter-actuator)的依赖
2) 创建bootstrap.properties配置，来指定config server
spring.application.name=jianghu
spring.cloud.config.profile=dev
spring.cloud.config.label=master
spring.cloud.config.uri=http://localhost:7001/
server.port=7002

这里需要格外注意：上面这些属性必须配置在bootstrap.properties中，
config部分内容才能被正确加载。因为config的相关配置会先于application.properties，
而bootstrap.properties的加载也是先于application.properties。

3) 创建一个Rest Api来返回配置中心的from属性
@RefreshScope
@RestController
class TestController {
    @Value("${from}")
    private String from;
    @RequestMapping("/from")
    public String from() {
        return this.from;
    }
}

访问地址：http://10.5.2.241:7002/from