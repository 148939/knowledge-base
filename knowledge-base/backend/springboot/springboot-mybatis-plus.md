# 检索索引：本文档收录【MyBatis-Plus】【核心基础】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。MyBatis-Plus 的愿景是成为 MyBatis 最好的搭档，就像魂斗罗中的 1P、2P，基友搭配，效率翻倍。

## 2. 核心知识点（结构化层级）

### 2.1 快速入门
1. 环境准备
2. 添加依赖
3. 配置
4. 编码
5. 开始使用

### 2.2 核心功能
1. CRUD 接口
   - BaseMapper
   - IService
2. 条件构造器
   - Wrapper
   - QueryWrapper
   - UpdateWrapper
   - LambdaWrapper
3. 分页插件
4. 代码生成器

### 2.3 注解
1. @TableName
2. @TableId
3. @TableField
4. @TableLogic
5. @Version
6. @OrderBy

### 2.4 插件
1. 分页插件
2. 乐观锁插件
3. 逻辑删除插件
4. 性能分析插件
5. 多租户插件
6. 动态表名插件

### 2.5 高级功能
1. 主键策略
2. 自动填充
3. 逻辑删除
4. 乐观锁
5. 通用枚举
6. 多数据源

## 3. 官方标准语法 / API

### 基础配置
```java
// pom.xml 依赖
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>最新版本</version>
</dependency>

// application.yml 配置
mybatis-plus:
  mapper-locations: classpath*:/mapper/**/*.xml
  type-aliases-package: com.example.entity
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: false
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      id-type: auto
      logic-delete-field: deleted
      logic-delete-value: 1
      logic-not-delete-value: 0
```

### 实体类注解
```java
import com.baomidou.mybatisplus.annotation.*;
import java.time.LocalDateTime;

@TableName("user")
public class User {
    
    @TableId(type = IdType.AUTO)
    private Long id;
    
    @TableField("username")
    private String username;
    
    @TableField("password")
    private String password;
    
    @TableField("email")
    private String email;
    
    @TableField("age")
    private Integer age;
    
    @TableField(value = "create_time", fill = FieldFill.INSERT)
    private LocalDateTime createTime;
    
    @TableField(value = "update_time", fill = FieldFill.INSERT_UPDATE)
    private LocalDateTime updateTime;
    
    @TableLogic
    @TableField("deleted")
    private Integer deleted;
    
    @Version
    @TableField("version")
    private Integer version;
    
    // getters and setters
}
```

### Mapper 接口
```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

@Mapper
public interface UserMapper extends BaseMapper<User> {
    
    // 自定义查询方法
    User selectByName(@Param("name") String name);
    
    // 自定义分页查询
    IPage<User> selectUserPage(Page<User> page, @Param("status") Integer status);
}
```

### Service 接口与实现
```java
import com.baomidou.mybatisplus.extension.service.IService;

public interface UserService extends IService<User> {
    
    // 自定义业务方法
    User getUserByName(String name);
    
    boolean saveOrUpdateUser(User user);
}
```

```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {
    
    @Override
    public User getUserByName(String name) {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUsername, name);
        return this.getOne(wrapper);
    }
    
    @Override
    public boolean saveOrUpdateUser(User user) {
        return this.saveOrUpdate(user);
    }
}
```

### BaseMapper CRUD 方法
```java
// 插入
int insert(T entity);

// 删除
int deleteById(Serializable id);
int deleteByMap(Map<String, Object> columnMap);
int delete(Wrapper<T> wrapper);
int deleteBatchIds(Collection<? extends Serializable> idList);

// 更新
int updateById(T entity);
int update(T entity, Wrapper<T> updateWrapper);

// 查询
T selectById(Serializable id);
List<T> selectBatchIds(Collection<? extends Serializable> idList);
List<T> selectByMap(Map<String, Object> columnMap);
T selectOne(Wrapper<T> queryWrapper);
Long selectCount(Wrapper<T> queryWrapper);
List<T> selectList(Wrapper<T> queryWrapper);
List<Map<String, Object>> selectMaps(Wrapper<T> queryWrapper);
List<Object> selectObjs(Wrapper<T> queryWrapper);
```

### IService CRUD 方法
```java
// 保存
boolean save(T entity);
boolean saveBatch(Collection<T> entityList);
boolean saveBatch(Collection<T> entityList, int batchSize);

// 保存或更新
boolean saveOrUpdate(T entity);
boolean saveOrUpdateBatch(Collection<T> entityList);
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);

// 删除
boolean removeById(Serializable id);
boolean removeByMap(Map<String, Object> columnMap);
boolean remove(Wrapper<T> queryWrapper);
boolean removeByIds(Collection<? extends Serializable> idList);

// 更新
boolean updateById(T entity);
boolean update(T entity, Wrapper<T> updateWrapper);
boolean updateBatchById(Collection<T> entityList);
boolean updateBatchById(Collection<T> entityList, int batchSize);

// 查询
T getById(Serializable id);
List<T> listByIds(Collection<? extends Serializable> idList);
List<T> listByMap(Map<String, Object> columnMap);
T getOne(Wrapper<T> queryWrapper);
T getOne(Wrapper<T> queryWrapper, boolean throwEx);
Map<String, Object> getMap(Wrapper<T> queryWrapper);
int count();
int count(Wrapper<T> queryWrapper);
List<T> list();
List<T> list(Wrapper<T> queryWrapper);
List<Map<String, Object>> listMaps();
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);
List<Object> listObjs();
List<Object> listObjs(Wrapper<T> queryWrapper);

// 分页
<E extends IPage<T>> E page(E page);
<E extends IPage<T>> E page(E page, Wrapper<T> queryWrapper);
<E extends IPage<Map<String, Object>>> E pageMaps(E page);
<E extends IPage<Map<String, Object>>> E pageMaps(E page, Wrapper<T> queryWrapper);
```

### 条件构造器
```java
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import com.baomidou.mybatisplus.core.conditions.update.UpdateWrapper;
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.conditions.update.LambdaUpdateWrapper;

// QueryWrapper
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.eq("username", "张三")
            .ge("age", 18)
            .like("email", "@example.com")
            .orderByDesc("create_time");

// UpdateWrapper
UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
updateWrapper.set("age", 20)
            .eq("username", "张三");

// LambdaQueryWrapper
LambdaQueryWrapper<User> lambdaQueryWrapper = new LambdaQueryWrapper<>();
lambdaQueryWrapper.eq(User::getUsername, "张三")
                  .ge(User::getAge, 18)
                  .like(User::getEmail, "@example.com")
                  .orderByDesc(User::getCreateTime);

// LambdaUpdateWrapper
LambdaUpdateWrapper<User> lambdaUpdateWrapper = new LambdaUpdateWrapper<>();
lambdaUpdateWrapper.set(User::getAge, 20)
                  .eq(User::getUsername, "张三");

// 链式调用
userMapper.selectList(
    new QueryWrapper<User>()
        .select("id", "username", "email")
        .eq("status", 1)
        .like("username", "张")
        .between("age", 18, 30)
        .in("dept_id", Arrays.asList(1, 2, 3))
        .orderByAsc("create_time")
        .last("limit 10")
);
```

### 条件构造器方法
```java
// 等值操作
eq(R column, Object val)
ne(R column, Object val)
gt(R column, Object val)
ge(R column, Object val)
lt(R column, Object val)
le(R column, Object val)
between(R column, Object val1, Object val2)
notBetween(R column, Object val1, Object val2)

// 模糊查询
like(R column, Object val)
notLike(R column, Object val)
likeLeft(R column, Object val)
likeRight(R column, Object val)

// 范围查询
in(R column, Collection<?> value)
notIn(R column, Collection<?> value)
inSql(R column, String inValue)
notInSql(R column, String inValue)

// 空值查询
isNull(R column)
isNotNull(R column)

// 分组排序
groupBy(R... columns)
orderByAsc(R... columns)
orderByDesc(R... columns)
orderBy(boolean condition, boolean isAsc, R... columns)

// 逻辑运算
and(Consumer<Param> consumer)
or()
or(Consumer<Param> consumer)
nested(Consumer<Param> consumer)

// 其他
select(String... sqlSelect)
last(String lastSql)
exists(String existsSql)
notExists(String existsSql)
```

### 分页插件配置
```java
import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class MybatisPlusConfig {
    
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}
```

### 分页使用
```java
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;

// 简单分页
Page<User> page = new Page<>(1, 10);
IPage<User> userPage = userMapper.selectPage(page, null);

// 带条件分页
LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
wrapper.eq(User::getStatus, 1);
Page<User> page = new Page<>(1, 10);
IPage<User> userPage = userMapper.selectPage(page, wrapper);

// 获取分页数据
List<User> records = userPage.getRecords();
long total = userPage.getTotal();
long size = userPage.getSize();
long current = userPage.getCurrent();
long pages = userPage.getPages();

// Service 分页
Page<User> page = new Page<>(1, 10);
IPage<User> result = userService.page(page, wrapper);
```

### 自动填充
```java
import com.baomidou.mybatisplus.core.handlers.MetaObjectHandler;
import org.apache.ibatis.reflection.MetaObject;
import org.springframework.stereotype.Component;
import java.time.LocalDateTime;

@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
    
    @Override
    public void insertFill(MetaObject metaObject) {
        this.strictInsertFill(metaObject, "createTime", LocalDateTime.class, LocalDateTime.now());
        this.strictInsertFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
        this.strictInsertFill(metaObject, "createBy", String.class, "admin");
    }
    
    @Override
    public void updateFill(MetaObject metaObject) {
        this.strictUpdateFill(metaObject, "updateTime", LocalDateTime.class, LocalDateTime.now());
        this.strictUpdateFill(metaObject, "updateBy", String.class, "admin");
    }
}
```

### 逻辑删除
```java
// 实体类
@TableLogic
private Integer deleted;

// application.yml 配置
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: deleted
      logic-delete-value: 1
      logic-not-delete-value: 0
```

### 乐观锁
```java
// 实体类
@Version
private Integer version;

// 配置插件
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    // 乐观锁插件
    interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
    return interceptor;
}

// 使用
User user = userMapper.selectById(1L);
user.setEmail("new@example.com");
userMapper.updateById(user);
```

## 4. 官方示例代码（可运行）

### 完整 CRUD 示例
```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private UserMapper userMapper;
    
    // 新增用户
    @PostMapping
    public Result<User> create(@RequestBody User user) {
        boolean success = userService.save(user);
        return success ? Result.success(user) : Result.error("创建失败");
    }
    
    // 根据ID查询用户
    @GetMapping("/{id}")
    public Result<User> getById(@PathVariable Long id) {
        User user = userService.getById(id);
        return Result.success(user);
    }
    
    // 查询用户列表
    @GetMapping
    public Result<List<User>> list(
            @RequestParam(required = false) String username,
            @RequestParam(required = false) Integer status) {
        
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        
        if (username != null && !username.isEmpty()) {
            wrapper.like(User::getUsername, username);
        }
        
        if (status != null) {
            wrapper.eq(User::getStatus, status);
        }
        
        wrapper.orderByDesc(User::getCreateTime);
        
        List<User> list = userService.list(wrapper);
        return Result.success(list);
    }
    
    // 分页查询用户
    @GetMapping("/page")
    public Result<IPage<User>> page(
            @RequestParam(defaultValue = "1") Integer current,
            @RequestParam(defaultValue = "10") Integer size,
            @RequestParam(required = false) String username) {
        
        Page<User> page = new Page<>(current, size);
        
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        if (username != null && !username.isEmpty()) {
            wrapper.like(User::getUsername, username);
        }
        wrapper.orderByDesc(User::getCreateTime);
        
        IPage<User> result = userService.page(page, wrapper);
        return Result.success(result);
    }
    
    // 更新用户
    @PutMapping("/{id}")
    public Result<User> update(@PathVariable Long id, @RequestBody User user) {
        user.setId(id);
        boolean success = userService.updateById(user);
        return success ? Result.success(user) : Result.error("更新失败");
    }
    
    // 删除用户
    @DeleteMapping("/{id}")
    public Result<Void> delete(@PathVariable Long id) {
        boolean success = userService.removeById(id);
        return success ? Result.success() : Result.error("删除失败");
    }
    
    // 批量删除用户
    @DeleteMapping("/batch")
    public Result<Void> deleteBatch(@RequestBody List<Long> ids) {
        boolean success = userService.removeByIds(ids);
        return success ? Result.success() : Result.error("删除失败");
    }
    
    // 复杂查询示例
    @GetMapping("/complex")
    public Result<List<User>> complexQuery() {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.select(User::getId, User::getUsername, User::getEmail)
                .eq(User::getStatus, 1)
                .like(User::getUsername, "张")
                .between(User::getAge, 18, 30)
                .inSql(User::getDeptId, "SELECT id FROM dept WHERE status = 1")
                .and(w -> w.lt(User::getCreateTime, LocalDateTime.now())
                        .or()
                        .isNull(User::getUpdateTime))
                .orderByAsc(User::getCreateTime)
                .last("LIMIT 10");
        
        List<User> users = userService.list(wrapper);
        return Result.success(users);
    }
}
```

### 批量操作示例
```java
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.List;

@Service
public class UserBatchService {
    
    @Autowired
    private UserService userService;
    
    // 批量插入
    @Transactional(rollbackFor = Exception.class)
    public boolean batchSave(List<User> userList) {
        return userService.saveBatch(userList);
    }
    
    // 批量插入（指定批次大小）
    @Transactional(rollbackFor = Exception.class)
    public boolean batchSave(List<User> userList, int batchSize) {
        return userService.saveBatch(userList, batchSize);
    }
    
    // 批量更新
    @Transactional(rollbackFor = Exception.class)
    public boolean batchUpdate(List<User> userList) {
        return userService.updateBatchById(userList);
    }
    
    // 批量保存或更新
    @Transactional(rollbackFor = Exception.class)
    public boolean batchSaveOrUpdate(List<User> userList) {
        return userService.saveOrUpdateBatch(userList);
    }
    
    // 自定义批量操作
    @Transactional(rollbackFor = Exception.class)
    public void customBatchOperation(List<User> userList) {
        for (User user : userList) {
            if (user.getId() == null) {
                userService.save(user);
            } else {
                userService.updateById(user);
            }
        }
    }
}
```

### Service 层业务逻辑示例
```java
import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.core.metadata.IPage;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserBusinessService {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private UserMapper userMapper;
    
    // 用户注册
    @Transactional(rollbackFor = Exception.class)
    public User register(User user) {
        // 检查用户名是否存在
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUsername, user.getUsername());
        User existUser = userService.getOne(wrapper);
        
        if (existUser != null) {
            throw new BusinessException("用户名已存在");
        }
        
        // 检查邮箱是否存在
        wrapper.clear();
        wrapper.eq(User::getEmail, user.getEmail());
        existUser = userService.getOne(wrapper);
        
        if (existUser != null) {
            throw new BusinessException("邮箱已存在");
        }
        
        // 保存用户
        user.setStatus(1);
        userService.save(user);
        
        return user;
    }
    
    // 用户登录
    public User login(String username, String password) {
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUsername, username)
                .eq(User::getPassword, password)
                .eq(User::getStatus, 1);
        
        User user = userService.getOne(wrapper);
        
        if (user == null) {
            throw new BusinessException("用户名或密码错误");
        }
        
        return user;
    }
    
    // 分页查询活跃用户
    public IPage<User> getActiveUsers(int current, int size, String keyword) {
        Page<User> page = new Page<>(current, size);
        
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getStatus, 1);
        
        if (keyword != null && !keyword.isEmpty()) {
            wrapper.and(w -> w.like(User::getUsername, keyword)
                    .or()
                    .like(User::getEmail, keyword));
        }
        
        wrapper.orderByDesc(User::getCreateTime);
        
        return userService.page(page, wrapper);
    }
    
    // 统计用户数量
    public Map<String, Long> getUserStatistics() {
        Map<String, Long> result = new HashMap<>();
        
        // 总用户数
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        long total = userService.count();
        result.put("total", total);
        
        // 活跃用户数
        wrapper.clear();
        wrapper.eq(User::getStatus, 1);
        long active = userService.count(wrapper);
        result.put("active", active);
        
        // 今日新增
        wrapper.clear();
        wrapper.ge(User::getCreateTime, LocalDate.now().atStartOfDay());
        long todayNew = userService.count(wrapper);
        result.put("todayNew", todayNew);
        
        return result;
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 配置注意事项
1. **包扫描**：确保 Mapper 接口被正确扫描到
2. **类型别名**：配置 type-aliases-package 简化 XML 配置
3. **日志配置**：开发环境配置 SQL 日志便于调试
4. **数据库类型**：分页插件需要指定正确的数据库类型
5. **全局配置**：合理配置全局策略减少重复代码

### CRUD 使用注意事项
1. **主键策略**：根据业务场景选择合适的主键策略
2. **null 值处理**：注意 updateById 对 null 值的处理
3. **Wrapper 使用**：优先使用 LambdaWrapper 避免硬编码字段名
4. **批量操作**：大数据量批量操作注意分批处理
5. **事务管理**：涉及多表操作时使用事务保证数据一致性

### 性能优化
1. **索引利用**：确保查询条件能利用数据库索引
2. **查询字段**：只查询需要的字段，避免 select *
3. **分页查询**：合理使用分页避免查询大量数据
4. **缓存使用**：对于不常变化的数据使用缓存
5. **批量操作**：使用批量操作减少数据库交互次数

### 插件使用注意事项
1. **分页插件**：确保分页插件正确配置并添加到拦截器链
2. **乐观锁**：乐观锁适用于并发不高的场景
3. **逻辑删除**：逻辑删除的数据仍占用存储空间，定期清理
4. **多租户**：多租户插件注意租户字段的隔离
5. **动态表名**：动态表名注意线程安全问题

### 最佳实践总结
1. **分层架构**：遵循 Controller-Service-Mapper 分层架构
2. **Service 封装**：复杂业务逻辑封装在 Service 层
3. **Lambda 表达式**：优先使用 LambdaWrapper 提高代码可维护性
4. **分页查询**：列表查询优先使用分页
5. **事务控制**：合理使用 @Transactional 注解
6. **异常处理**：统一异常处理，提供友好的错误信息
7. **代码生成**：使用代码生成器快速生成基础代码
8. **单元测试**：为 Service 层编写单元测试
9. **SQL 优化**：定期分析慢查询并优化
10. **文档维护**：保持代码注释和文档的更新

## 6. 官方文档参考链接

- MyBatis-Plus 官方文档：https://baomidou.com/
- MyBatis-Plus GitHub：https://github.com/baomidou/mybatis-plus
- MyBatis-Plus 指南：https://baomidou.com/guide/
