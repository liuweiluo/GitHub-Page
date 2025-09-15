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

