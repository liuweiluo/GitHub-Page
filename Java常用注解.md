## Java常用注解

### MyBatis常用注解
#### Mapper 接口注解（SQL语句相关）
| 注解                | 作用          | 常见场景    | 示例                                                                                           |
| ----------------- | ----------- | ------- | -------------------------------------------------------------------------------------------- |
| `@Select`         | 执行查询语句，返回结果 | 查询单表或多表 | `java @Select("SELECT * FROM user WHERE id = #{id}") User getById(Integer id);`              |
| `@Insert`         | 执行插入语句      | 插入数据    | `java @Insert("INSERT INTO user(name, age) VALUES(#{name}, #{age})") int insert(User user);` |
| `@Update`         | 执行更新语句      | 修改数据    | `java @Update("UPDATE user SET name=#{name} WHERE id=#{id}") int update(User user);`         |
| `@Delete`         | 执行删除语句      | 删除数据    | `java @Delete("DELETE FROM user WHERE id=#{id}") int delete(Integer id);`                    |
| `@SelectProvider` | 动态生成查询语句    | 复杂 SQL  | `java @SelectProvider(type = SqlBuilder.class, method = "buildQuery")`                       |
| `@InsertProvider` | 动态生成插入语句    | 复杂插入    | `java @InsertProvider(type = SqlBuilder.class, method = "buildInsert")`                      |
| `@UpdateProvider` | 动态生成更新语句    | 动态更新    | `java @UpdateProvider(type = SqlBuilder.class, method = "buildUpdate")`                      |
| `@DeleteProvider` | 动态生成删除语句    | 动态删除    | `java @DeleteProvider(type = SqlBuilder.class, method = "buildDelete")`                      |

#### 结果映射注解
| 注解                 | 作用                   | 场景                   | 示例                                                                   |
| ------------------ | -------------------- | -------------------- | -------------------------------------------------------------------- |
| `@Results`         | 定义结果映射集合             | 列名与属性名不一致时           | `java @Results({ @Result(column="user_name", property="name") })`    |
| `@Result`          | 映射单个字段               | 单列到属性                | `java @Result(column="user_age", property="age")`                    |
| `@One`             | 一对一关联查询              | 关联对象                 | `java @One(select="getAddressById")`                                 |
| `@Many`            | 一对多关联查询              | 关联集合                 | `java @Many(select="getOrdersByUserId")`                             |
| `@ResultMap`       | 引用已定义的 `@Results` 映射 | 复用映射                 | `java @ResultMap("userMap")`                                         |
| `@ConstructorArgs` | 使用构造方法映射             | 无Setter类             | `java @ConstructorArgs({@Arg(column="id", javaType=Integer.class)})` |
| `@Arg`             | 构造参数映射               | 配合`@ConstructorArgs` | `java @Arg(column="name", javaType=String.class)`                    |

#### 参数注解
| 注解       | 作用             | 场景    | 示例                                                                                                  |
| -------- | -------------- | ----- | --------------------------------------------------------------------------------------------------- |
| `@Param` | 给方法参数命名，SQL中使用 | 多参数查询 | `java @Select("SELECT * FROM user WHERE name=#{name}") User getByName(@Param("name") String name);` |

#### Mapper 注册与扫描
| 注解            | 作用                    | 场景                           |
| ------------- | --------------------- | ---------------------------- |
| `@Mapper`     | 标记为 MyBatis Mapper 接口 | 配合 `@MapperScan` 扫描 Mapper 包 |
| `@MapperScan` | 指定扫描 Mapper 接口包路径     | 在配置类或启动类使用                   |

### Spring 常用注解汇总
| 分类                 | 注解                                                                | 作用                                                                   | 使用位置      |
| ------------------ | ----------------------------------------------------------------- | -------------------------------------------------------------------- | --------- |
| **组件声明**           | `@Component`                                                      | 将类标记为 Spring 管理的组件                                                   | 类         |
|                    | `@Controller`                                                     | 表示控制层组件，用于处理 Web 请求                                                  | 类         |
|                    | `@Service`                                                        | 表示业务逻辑层组件                                                            | 类         |
|                    | `@Repository`                                                     | 表示持久层组件（DAO 层），支持异常转换                                                | 类         |
| **依赖注入**           | `@Autowired`                                                      | 按类型自动注入依赖对象                                                          | 构造器、字段、方法 |
|                    | `@Qualifier`                                                      | 和 `@Autowired` 搭配，按名称注入                                              | 参数、字段     |
|                    | `@Resource`                                                       | 按名称/类型注入，J2EE 标准注解                                                   | 字段、方法     |
|                    | `@Value`                                                          | 注入配置文件中的属性值                                                          | 字段、方法参数   |
|                    | `@Primary`                                                        | 设置优先注入的 Bean                                                         | 类         |
| **配置与 Bean**       | `@Configuration`                                                  | 声明配置类                                                                | 类         |
|                    | `@Bean`                                                           | 定义一个 Bean                                                            | 方法        |
|                    | `@Import`                                                         | 导入额外配置类                                                              | 类         |
|                    | `@PropertySource`                                                 | 加载外部属性文件                                                             | 类         |
|                    | `@ComponentScan`                                                  | 扫描指定包下的组件                                                            | 类         |
| **作用域与生命周期**       | `@Scope`                                                          | 指定 Bean 作用域，如 `singleton`、`prototype`                                | 类、方法      |
|                    | `@Lazy`                                                           | 延迟加载 Bean                                                            | 类、方法      |
|                    | `@PostConstruct`                                                  | Bean 初始化后执行的方法                                                       | 方法        |
|                    | `@PreDestroy`                                                     | Bean 销毁前执行的方法                                                        | 方法        |
| **AOP 相关**         | `@Aspect`                                                         | 声明切面类                                                                | 类         |
|                    | `@Before`                                                         | 方法执行前织入                                                              | 方法        |
|                    | `@After`                                                          | 方法执行后织入                                                              | 方法        |
|                    | `@AfterReturning`                                                 | 方法正常返回后织入                                                            | 方法        |
|                    | `@AfterThrowing`                                                  | 方法抛出异常后织入                                                            | 方法        |
|                    | `@Around`                                                         | 环绕通知，方法执行前后织入                                                        | 方法        |
|                    | `@EnableAspectJAutoProxy`                                         | 开启 AOP 自动代理                                                          | 配置类       |
| **事务管理**           | `@Transactional`                                                  | 声明方法或类的事务属性                                                          | 类、方法      |
| **Spring Boot 特有** | `@SpringBootApplication`                                          | 组合注解，包含 `@Configuration`、`@EnableAutoConfiguration`、`@ComponentScan` | 主类        |
|                    | `@EnableAutoConfiguration`                                        | 自动加载配置                                                               | 类         |
|                    | `@ConfigurationProperties`                                        | 将配置文件属性绑定到 Bean                                                      | 类         |
|                    | `@RestController`                                                 | `@Controller` + `@ResponseBody`                                      | 类         |
|                    | `@ResponseBody`                                                   | 方法返回值直接序列化为 JSON/XML                                                 | 方法        |
|                    | `@RequestMapping`                                                 | 映射请求路径                                                               | 类、方法      |
|                    | `@GetMapping` / `@PostMapping` / `@PutMapping` / `@DeleteMapping` | 简化请求映射                                                               | 方法        |
| **条件与环境**          | `@Profile`                                                        | 根据环境激活不同配置                                                           | 类、方法      |
|                    | `@Conditional`                                                    | 条件加载 Bean                                                            | 类、方法      |
|                    | `@EnableScheduling`                                               | 开启定时任务                                                               | 配置类       |
|                    | `@Scheduled`                                                      | 定义定时任务                                                               | 方法        |
| **测试相关**           | `@SpringBootTest`                                                 | Spring Boot 集成测试                                                     | 类         |
|                    | `@RunWith(SpringRunner.class)`                                    | 使用 Spring 测试运行器                                                      | 类         |

### SpringMVC 常用注解汇总
| 注解                      | 说明                                              | 使用位置    | 示例                                                |
| ----------------------- | ----------------------------------------------- | ------- | ------------------------------------------------- |
| `@Controller`           | 标注一个类为 SpringMVC 控制器，返回视图                       | 类       | `@Controller public class UserController {}`      |
| `@RestController`       | `@Controller + @ResponseBody` 的组合，返回 JSON 或 XML | 类       | `@RestController public class ApiController {}`   |
| `@RequestMapping`       | 映射 URL 到控制器方法，可标注在类或方法上                         | 类/方法    | `@RequestMapping("/user")`                        |
| `@GetMapping`           | 处理 GET 请求，`@RequestMapping(method=GET)`的简写      | 方法      | `@GetMapping("/list")`                            |
| `@PostMapping`          | 处理 POST 请求                                      | 方法      | `@PostMapping("/save")`                           |
| `@PutMapping`           | 处理 PUT 请求                                       | 方法      | `@PutMapping("/update")`                          |
| `@DeleteMapping`        | 处理 DELETE 请求                                    | 方法      | `@DeleteMapping("/delete/{id}")`                  |
| `@PatchMapping`         | 处理 PATCH 请求                                     | 方法      | `@PatchMapping("/patch")`                         |
| `@RequestParam`         | 获取请求参数（Query、Form），可设置默认值和是否必填                  | 方法参数    | `@RequestParam("id") Long id`                     |
| `@PathVariable`         | 获取路径中的参数                                        | 方法参数    | `@PathVariable("id") Long id`                     |
| `@RequestBody`          | 将请求体 JSON 转为对象                                  | 方法参数    | `@RequestBody User user`                          |
| `@ResponseBody`         | 将方法返回值序列化为 JSON/XML 响应                          | 方法      | `@ResponseBody String hello()`                    |
| `@ModelAttribute`       | 将请求参数绑定到对象，或在方法执行前加入模型                          | 方法参数/方法 | `@ModelAttribute User user`                       |
| `@RequestHeader`        | 获取请求头中的参数                                       | 方法参数    | `@RequestHeader("User-Agent") String ua`          |
| `@CookieValue`          | 获取 Cookie 中的参数                                  | 方法参数    | `@CookieValue("token") String token`              |
| `@SessionAttribute`     | 获取 session 中的参数                                 | 方法参数    | `@SessionAttribute("user") User user`             |
| `@CrossOrigin`          | 允许跨域请求                                          | 类/方法    | `@CrossOrigin(origins="*")`                       |
| `@InitBinder`           | 自定义参数绑定/类型转换                                    | 方法      | `@InitBinder public void initBinder(...)`         |
| `@ExceptionHandler`     | 处理控制器抛出的异常                                      | 方法      | `@ExceptionHandler(Exception.class)`              |
| `@ControllerAdvice`     | 全局控制器增强（统一异常、数据绑定等）                             | 类       | `@ControllerAdvice public class GlobalHandler {}` |
| `@ResponseStatus`       | 自定义响应状态码                                        | 类/方法    | `@ResponseStatus(HttpStatus.NOT_FOUND)`           |
| `@Validated` / `@Valid` | 参数校验注解（结合 JSR303 Bean Validation）               | 方法参数    | `public void save(@Valid User user)`              |
