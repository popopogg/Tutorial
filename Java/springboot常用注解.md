在 Spring Boot 中，注解（Annotations）极大地简化了配置和开发流程。以下是一些常用的 Spring Boot 注解，按用途分类整理，方便查阅：

---

一、启动与配置类

注解	说明	
`@SpringBootApplication`	组合注解，等同于 `@Configuration` + `@EnableAutoConfiguration` + `@ComponentScan`，用于启动类。	

---

二、组件注册

注解	说明	
`@Component`	通用组件注解，标记为 Spring 容器管理的 Bean。	
`@Service`	业务逻辑层组件，本质上是 `@Component` 的特化。	
`@Repository`	数据访问层组件，用于 DAO 层，支持异常转换。	
`@Controller`	控制层组件，返回视图（配合模板引擎如 Thymeleaf）。	
`@RestController`	组合注解，等于 `@Controller` + `@ResponseBody`，返回 JSON 数据。	

---

三、依赖注入

注解	说明	
`@Autowired`	按类型自动注入依赖（Spring 提供）。	
`@Qualifier`	配合 `@Autowired` 使用，按名称指定注入的 Bean。	
`@Resource`	JSR-250 注解，按名称注入（Java 提供）。	
`@Inject`	JSR-330 注解，功能类似 `@Autowired`。	

---

四、配置与属性绑定

注解	说明	
`@Configuration`	声明当前类为配置类（替代 XML 配置）。	
`@ConfigurationProperties(prefix = "xxx")`	将配置文件（如 `application.yml`）中的属性绑定到 Bean。	
`@Value("${key}")`	注入单个配置值。	
`@PropertySource("classpath:xxx.properties")`	加载自定义属性文件。	

---

五、Web 开发

注解	说明	
`@RequestMapping`	通用映射注解，可修饰类或方法。	
`@GetMapping`、`@PostMapping`、`@PutMapping`、`@DeleteMapping`	REST 风格映射注解，分别对应 GET、POST、PUT、DELETE 请求。	
`@PathVariable`	获取 URL 中的占位符参数，如 `/users/{id}`。	
`@RequestParam`	获取请求参数，如 `?name=Tom`。	
`@RequestBody`	接收 JSON 格式的请求体，反序列化为对象。	
`@ResponseBody`	将方法返回值直接作为响应体（通常用于返回 JSON）。	

---

六、条件装配

注解	说明	
`@ConditionalOnClass`	类路径下存在指定类时才生效。	
`@ConditionalOnMissingBean`	容器中不存在指定 Bean 时才生效。	
`@ConditionalOnProperty`	配置文件中存在指定属性时才生效。	

---

七、定时任务与异步

注解	说明	
`@EnableScheduling`	启用定时任务功能。	
`@Scheduled(fixedRate = 5000)`	定义定时任务。	
`@EnableAsync`	启用异步方法执行。	
`@Async`	标记方法为异步执行。	

---

八、测试

注解	说明	
`@SpringBootTest`	加载完整的 Spring 应用上下文进行集成测试。	
`@MockBean`	在测试环境中注入 Mock 对象。	

---

九、日志与监控（可选）

注解	说明	
`@Slf4j`（Lombok）	自动生成日志对象 `log`，简化日志记录。	

---

✅ 示例：一个典型的 REST 控制器

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```
