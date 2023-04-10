# springboot Tips

[TOC]

## 文档

- [Spring Boot参考指南](https://www.springcloud.cc/spring-boot.html#using-boot-maven-parent-pom)

  > 官方中文文档，很全

## 快速开始

- [IntelliJ IDEA创建完整的Spring Boot项目](https://segmentfault.com/a/1190000020983772)
- [idea下springboot打包成jar包和war包，并且可以在外部tomcat下运行访问到](https://www.cnblogs.com/sxdcgaq8080/p/7727249.html)
- [linux jar 包运行与关闭](https://blog.csdn.net/qq_39507276/article/details/82227416)

## springboot升级



## web服务器

### Undertow

> 问题：用MultipartFile上传文件的时候，缓存路径不能设置为相对路径。详细见下面MultipartFile

- [SpringBoot2使用Undertow来提高应用性能（spring-boot-starter-undertow）](https://my.oschina.net/u/3204029/blog/4330800)

- [Spring boot start with Undertow](https://www.jianshu.com/p/9710585258fb)

  > 我的代码
  >
  > ```xml
  > <dependency>
  >  <groupId>org.springframework.boot</groupId>
  >  <artifactId>spring-boot-starter-web</artifactId>
  >  <exclusions>
  >      <exclusion>
  >          <groupId>org.springframework.boot</groupId>
  >          <artifactId>spring-boot-starter-tomcat</artifactId>
  >      </exclusion>
  >  </exclusions>
  > </dependency>
  > <dependency>
  >  <groupId>org.springframework.boot</groupId>
  >  <artifactId>spring-boot-starter-undertow</artifactId>
  > </dependency>
  > ```

### tomcat

- [如何解决tomcat中的应用报java.io.IOException: 您的主机中的软件中止了一个已建立的连接。](https://blog.csdn.net/shiyong1949/article/details/72845634)

  > 我的方法：排除tomcat，用Undertow

## 参数

- [SpringBoot Controller接收参数的几种常用方式](https://blog.csdn.net/suki_rong/article/details/80445880)

- [SpringMVC Controller接收参数总结](https://www.jianshu.com/p/ed44e89a6f79)

- validation校验

  - [【参数校验框架】javax的validation使用](https://www.jianshu.com/p/93dd02d9ff32)

    - [SpringBoot 中使用 @Valid 注解 + Exception 全局处理器优雅处理参数验证](http://www.mydlq.club/article/49/)

      > 在 SpringBoot 2.3.x 以前 SpringBoot 包 默认引入 spring-boot-starter-validation 包，而自 SpringBoot 2.3.x 以后官方将其排除，需要单独引入

  - [SpringBoot如何优雅的校验参数](https://zhuanlan.zhihu.com/p/97555913)

    > 自定义注解和分组还没用过。有机会可以试试

  - [@Validated和@Valid校验参数、级联属性、List](https://cloud.tencent.com/developer/article/1525195)

    > 我用的 "方法2：使用@Validated @Valid" 。该方式对list<实体>有用，但是list<string>这种貌似没用。
    >
    > 1. 在controller类上面增加@Validated注解
    > 2. controller中，方法参数前加@Valid
    > 3. query参数直接加JSR-303注解
    >
    > ```
    > public BaseResult snapStatistics(@NotNull Integer snapType) {}
    > ```
    >
    > 4. body参数类型
    >
    >    1. 如果在参数里面包裹着list或者泛型BO，还需要在list或者泛型参数上面一行加@Valid
    >    2. 如果参数内还有参数对象包含list或者泛型BO，重复第三个操作
    >
    > ```java
    > //list
    > @Valid
    > @ApiModelProperty(name = "list", value = "xxxx", allowEmptyValue = true)
    > private List<SaveUserValidator> list;
    > //也适用于泛型
    > @Valid
    > private XX<AA> params;
    > ```

  - validation校验resteasy接口

  1. 在接口interface和实现类的要校验的BO前面加上 @Valid 
     2. 在接口实现类上面加上 @Valid

- string用@notblank

- date等用@notnull，不然要报错

- list等集合用@NotEmpty

- [springboot 关于controller层传递单个参数的校验](https://blog.csdn.net/qq_33996921/article/details/79568456)

  1. @Validated要在controller上标注

  2. 校验写在参数前面

     ```java
     public BaseResult test(@ApiParam(value = "成绩") @Max(value = 60,message = "得分必须小于60") @RequestParam int score, @ApiParam(value = "年龄") @Min(value = 3, message = "年龄大于3岁") @RequestParam int age, @ApiParam(value = "辅导作文类型") @NotNull(message = "辅导作文类型不能为空") @RequestParam(required = true) String name) {...}
     ```

- swagger参数

  - [Swagger注解-@ApiModel 和 @ApiModelProperty](https://blog.csdn.net/dejunyang/article/details/89527348)

    > `allowEmptyValue`：一定要传  
    > `required`：可以不传

  - 单个参数

    ```java
    public BaseResult poolInfos(@RequestParam("maxKeys") @ApiParam(value = "返回资源池信息的数量") @Min(value = 0, message = "参数checkNumber不能小于0") Integer maxKeys) {return sacClient.poolInfos(maxKeys);}
    ```

    

- [阿里巴巴fastjson @JSONField 注解说明](https://juejin.im/post/5c1e02b45188257a937f9fea)

- 时间

  - 传入时间

    - 实体中属性或者方法参数上加注解
      - `@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") Date startDate` 
    - 直接用Date接收

  - 返回时间

    - [java 后端与前端Date类型与String类型互相转换（使用注解）](https://blog.csdn.net/java_zhangshuai/article/details/95951400)

      > @JsonFormat(pattern = "yyyy年MM月dd日HH时mm分ss秒")

## 自动执行

- [如何在Spring Boot应用启动之后立刻执行一段逻辑](https://juejin.im/post/6844904178343673864)
  - CommandLineRunner
  - ApplicationRunner

## 定时任务

### 工具

- [在线Cron表达式生成器](http://cron.qqe2.com/)

- [java校验Cron表达式是否有效，正确](https://blog.csdn.net/qq_42717747/article/details/106788478)

  ```java
  CronExpression.isValidExpression(cronExpression);//返回一个布尔值代表一个给定的Cron表达式的有效性
  new CronExpression(cronExpression);//try catch ParseException。无效时返回表达式错误描述e.getMessage(),如果有效返回null
  new CronExpression(cronExpression).getNextValidTimeAfter(new Date(System.currentTimeMillis()));//返回下一个执行时间根据给定的Cron表达式
  ```

### 教程

- [通过配置开关 Spring Boot 中的 @Scheduled 定时任务](https://juejin.cn/post/6925447551499894791)

  > 技术比较全。可以参考他的。
  >
  > 如果类被 @Lazy 修饰导致 Spring Boot 在启动的时候没有实例化，那定时任务就不会开始执行。也就是说，被修饰的类不会执行定时任务。
  >
  > ```java
  > //注意corn1不能写成corn-1
  > @Scheduled(cron = "${cron.1:* * * * * ?}")
  > //properties中
  > cron.1=0/5 * * * * ?
  > ```

- [SpringBoot几种定时任务的实现方式](https://blog.csdn.net/wqh8522/article/details/79224290)

  > 关于fixedDelay说明有点问题，见我的文章[fixedDelay 和 fixedRate的区别](https://www.jianshu.com/p/59f7650d9860?v=1680509966522)。
  >
  > - fixedDelay：表示上一个任务执行结束后隔一段时间再开始下一次任务。即当任务执行完毕后，会等待一段时间，然后再执行下一次任务。如果上一个任务执行的时间很长，那么下一个任务会延迟执行。
  > - fixedRate：表示以固定的频率执行任务。即无论上一个任务执行的时间长短，下一次任务都会在固定的时间间隔后开始执行。如果上一个任务执行的时间很长，那么下一个任务也会按照固定的时间间隔开始执行，可能会导致并发执行多个任务。

- [spring boot项目中处理Schedule定时任务](https://www.rjkf.cn/springboot-schedule-cron/)

- [玩转SpringBoot之定时任务详解](https://www.cnblogs.com/mmzs/p/10161936.html)

  ```java
  //@Component注解用于对那些比较中立的类进行注释；
  //相对与在持久层、业务层和控制层分别采用 @Repository、@Service 和 @Controller 对分层中的类进行注释
  @Component
  @EnableScheduling   // 1.开启定时任务
  @EnableAsync        // 2.开启多线程
  public class MultithreadScheduleTask {
  
          //@Async					   //加了这个后，启动多线程，相当于任务直接完成，无视了程序执行时间。
          @Scheduled(fixedDelay = 1000)  //任务完成1秒后再次执行。如果加上@Async后就是任务开始1秒后再次执行，就变得和fixedRate一样了（自我感觉）？
      	//可以通过配置文件配置间隔时间
      	//@Scheduled(fixedDelayString = "${scheduled.ai-task.fixed:60000}")
          public void first() throws InterruptedException {
              System.out.println("第一个定时任务开始 : " + LocalDateTime.now().toLocalTime() + "\r\n线程 : " + Thread.currentThread().getName());
              System.out.println();
              Thread.sleep(1000 * 10);
          }
  
          @Async
          @Scheduled(fixedDelay = 2000)
          public void second() {
              System.out.println("第二个定时任务开始 : " + LocalDateTime.now().toLocalTime() + "\r\n线程 : " + Thread.currentThread().getName());
              System.out.println();
          }
      }
  ```

- [精通SpringBoot——第三篇：详解WebMvcConfigurer接口](https://yq.aliyun.com/articles/617307)

- [使用WebMvcConfigurer配置SpringMVC](https://www.jianshu.com/p/52f39b799fbb)

- 控制configuration是否生效

  - [springboot中@scheduled有没有开关机制](https://segmentfault.com/q/1010000011750690)

  - [@ConditionalOnProperty来控制Configuration是否生效](https://www.jianshu.com/p/68a75c093023)

  ```java
  @Component//或者@Configuration
  @EnableScheduling//只需要声明一次
  @ConditionalOnProperty(prefix = "scheduling", name = "enabled", havingValue = "true")
  //或者这样，配置文件没配置的话默认启动。实际测试配置类不生效，不知道为啥。@ConditionalOnProperty("${scheduling.enabled:true}")
  上面的注解加在配置类上配置文件上
  
  配置文件配置定时的总开关
  scheduling.enabled=true
  flase的话就不会执行
  ```

- [详解Springboot@ConditionalOnProperty注解](https://blog.csdn.net/u010142437/article/details/115732223)

  > 参数解释详细点

- [springboot定时任务线程阻塞踩坑](https://www.cnblogs.com/owenma/p/13080426.html)

  > 不能使用@Async，因为这个注解会无视fixedDelay

  - 方案一

    ```shell
    ## 配置可用线程数为10
    spring.task.scheduling.pool.size=10
    ```

  - 方案二：自定义taskschedule线程池

    ```shell
    @Configuration
    public class ScheduleConfig {
    
        /**
         * 此处方法名为Bean的名字，方法名无需固定因为是按TaskScheduler接口自动注入
         */
        @Bean
        public TaskScheduler taskScheduler(){
            // Spring提供的定时任务线程池类
            ThreadPoolTaskScheduler taskScheduler=new ThreadPoolTaskScheduler();
            //设定最大可用的线程数目
            taskScheduler.setPoolSize(10);
            return taskScheduler;
        }
    }
    ```

    

## 任务调度

### quartz

> 支持cron表达式或者简单模式（执行间隔，执行次数）。即SimpleScheduleBuilder和CronScheduleBuilder

#### 内存方式（需要自己维护数据库）

- [spring-boot-2.0.3之quartz集成，不是你想的那样哦！](https://www.cnblogs.com/youzhibing/p/10024558.html)

  > [他的最新源码地址](https://gitee.com/youzhibing/spring-boot-2.0.3/tree/master/spring-boot-quartz-plus)
  >
  > 配置了mysql数据库就能用。测试了效果可以。注意在修改sql那里要加上事务处理
  >
  > ```java
  > @Modifying(clearAutomatically = true)
  > @Transactional(rollbackFor = Exception.class)
  > @Query(value = "update quartz_job set trigger_state = ?3 where job_group = ?2 and job_name = ?1", nativeQuery = true)
  > void updateStatus(String jobName, String jobGroup, String status);
  > ```
  >
  > 1. JobStore选择RAMJobStore；
  > 2. 持久化我们自定义的job，应用启动的时候调用loadJobToQuartz加载给quartz，作为quartz job的初始值
  > 3. quartz job无需注入到spring，但是job中能注入spring的常规bean
  > 4. 修改任务他是直接删除，然后添加。所以cron和groupName之类的都可以修改。

- [Spring Boot集成Quartz-动态任务管理](https://cloud.tencent.com/developer/article/1482819)

  > 比较全面。比上面的多了。看之前先看下Quartz介绍[Spring Boot 定时任务之Quartz](https://blog.csdn.net/rickyit/article/details/77543418)
  >
  > 1. 使用QuartzConfig配置类来注入quartzJobFactory
  >
  >    参考**坑二：Spring Bean不能注入Job的实现类中**的代码
  >
  >    不过我没有添加下面这个代码，至于加不加，需要后续验证
  >
  > ```java
  > protected void springBeanAutowiringSupport() {
  >     SpringBeanAutowiringSupport.processInjectionBasedOnCurrentContext(this);
  > }
  > ```
  >
  > 2. 使用QuartzConfig配置类来注入scheduler，好处时可以添加监听器。和**上面**写在同一个文件里面。
  >
  >    参考**2.SchedulerFactory获取Schedule**的代码
  >
  > 3. 添加监听器JobListenerSupport。如果有问题参考**坑四：Job监听器中注入业务Bean**
  >
  >    1. 实现JobListener
  >    2. getName方法写上监听器名字。
  >    3. 重写jobWasExecuted方法
  >
  > 4. 上一个任务还未完成，不开始执行新的任务。在job的class上加注解 @DisallowConcurrentExecution

#### 数据持久化方式（需要配置多数据源）

- [《SpringBoot2.0 实战》系列-集成Quartz定时任务（持久化到数据库）](https://blog.csdn.net/HXNLYW/article/details/95055601)

  > 还未看

## MongoDB

- [Java操作MongoDB采用MongoRepository仓库进行条件查询](https://blog.csdn.net/qq_38288606/article/details/78673528)

- [SpringBoot中MongoDB注解概念及使用](https://www.cnblogs.com/liuchuanfeng/p/7772421.html)

- [spring boot 启动时候报错 MongoSocketOpenException](https://www.jianshu.com/p/20278ce81d79)

- springboot mongo findby*_*  当字段含有下划线，findby报错解决方法：

  ```java
  @Field(value="item_id")
      private String itemId;
  ```

## Redis

- [如何使用RedisTemplate访问Redis数据结构](https://www.jianshu.com/p/7bf5dc61ca06)

- [springboot redis key 乱码 key not found](https://www.jianshu.com/p/babd0ba1efab)

- [JAVA缓存-Redis入门级使用](https://juejin.im/post/5b8e251df265da4326074ae2)

  > 包含 Redisson 和 redisTemplate 

- Redisson

  - [Redisson官方文档 - 4. 数据序列化](https://blog.csdn.net/weixin_33756418/article/details/89781241)
  - [从头开始学Redisson--------话题（订阅分发）](https://blog.csdn.net/yanluandai1985/article/details/104824045)

## 数据操作

### 事物

- 大佬的总结：事务传播 - Propagation

  - **REQUIRED**

    使用当前的事务，如果当前没有事务，则自己新建一个事务，子方法是必须运行在一个事务中的；如果当前存在事务，则加入这个事务，成为一个整体。
    举例：领导没饭吃，我有钱，我会自己买了自己吃；领导有的吃，会分给你一起吃。

  - **SUPPORTS**

    如果当前有事务，则使用事务；如果当前没有事务，则不使用事务。
    举例：领导没饭吃，我也没饭吃；领导有饭吃，我也有饭吃。

  - **MANDATORY**

    该传播属性强制必须存在一个事务，如果不存在，则抛出异常
    举例：领导必须管饭，不管饭没饭吃，我就不乐意了，就不干了（抛出异常）

  - **REQUIRES_NEW**

    如果当前有事务，则挂起该事务，并且自己创建一个新的事务给自己使用；
    如果当前没有事务，则同 REQUIRED
    举例：领导有饭吃，我偏不要，我自己买了自己吃

  - **NOT_SUPPORTED**

    如果当前有事务，则把事务挂起，自己不适用事务去运行数据库操作
    举例：领导有饭吃，分一点给你，我太忙了，放一边，我不吃

  - **NEVER**

    如果当前有事务存在，则抛出异常
    举例：领导有饭给你吃，我不想吃，我热爱工作，我抛出异常

  - **NESTED**

    如果当前没有事务，则同 REQUIRED。
    但是如果主事务提交，则会携带子事务一起提交。
    如果主事务回滚，则子事务会一起回滚。相反，子事务异常，则父事务可以回滚或不回滚。
    举例：领导决策不对，老板怪罪，领导带着小弟一同受罪。小弟出了差错，领导可以推卸责任。

- [Spring 中的事务传播](https://segmentfault.com/a/1190000015794446)

  > 详细写了七种事务类型。有空一定看

- [对事务及其注解@Transactional的解读](https://www.cnblogs.com/myitnews/p/12363980.html)

  > 对于七种事务传播类型，写的不详细。

- [JPA中事务回滚的问题](https://www.cnblogs.com/Andrew520/p/12144362.html)

  > 默认回滚runtimeException。可以指定Exception.class

- [SpringBoot2异常处理回滚事务详解(自动回滚/手动回滚/部分回滚）](https://blog.csdn.net/zzhongcy/article/details/102893309)

  > 详细，有空一定要好好看


#### postgresql

- 和上面mysql，除了数据库连接换成pg的，语法设置成pg的，其他貌似没区别。

  ```xml
  <dependency>
      <groupId>org.postgresql</groupId>
      <artifactId>postgresql</artifactId>
  </dependency>
  ```

  ```java
  interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.POSTGRE_SQL));
  ```

- PSQLException: ERROR: LIMIT #,# syntax is not supported报错处理

  > 我的报错是因为设置成了mysql，导致mybatisplus使用成了mysql的语法（DbType.MYSQL），需要改成pg的。
  >
  > interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.POSTGRE_SQL));

### 多数据源

- [搞定SpringBoot多数据源(1)：多套源策略](https://segmentfault.com/a/1190000021573511)

  > 三部曲

- mybatis-plus+dynamic

  - [使用dynamic-datasource-spring-boot-starter做多数据源及源码分析](https://blog.csdn.net/w57685321/article/details/106823660)
  - mybatis-plus官网也有教程

  > 注意：最好是把每个数据源的jdbctemplate封装到单独的service中，我试过直接返回jdbctemplate会变成全局数据源。所以，每个数据源的service只返回数据，别返回jdbctemplate。
  >
  > ```java
  > @Service
  > @DS("second")
  > public class DcrService {
  >  @Value("${third.database.second.sql.statisticsPeopleGroupByCommunities}")
  >  private String statisticsPeopleGroupByCommunitiesSql;
  >  @Autowired
  >  private JdbcTemplate dcrJdbcTemplate;
  > 
  >  public List<Map<String, Object>> statisticsCommunities() {
  >      return dcrJdbcTemplate.queryForList(statisticsPeopleGroupByCommunitiesSql);
  >  }
  > }
  > ```

### 分库分表

#### Sharding-JDBC

> 1. 这个框架无法自动创建表，所以要手动建表
>
> 2. 4.0.0-RC1、RC2、RC3这三个版本可以不填actual-data-nodes参数(向按天分表这种不能填，因为每月天数不固定)，4.0.1、4.1.1都要填才能启动。
>
> 3. 4.1.1、4.0.0.RC3要加这个      management.health.db.enabled= false    才不会报错。
>
> 4. pg分页查询有问题，mysql没测过。
> 5. 因为项目用的pg，所以我最终用的RC3版本。

- [SpringBoot结合Sharding-JDBC实现分库分表](https://segmentfault.com/a/1190000022983968)

  > 上层框架用的mybatis，不知道能不能用mybatis-plus，有时间测试下。
  >
  > [他的github地址](https://github.com/leishen6/SpringBoot_shardDB_shardTable)。[我fork了一下](https://github.com/cqboyone/SpringBoot_shardDB_shardTable)

- [分库分表(4) ---SpringBoot + ShardingSphere 实现分表](https://www.cnblogs.com/qdhxhz/p/11651163.html)

  > 他这个系列有分库分表读写分离，但是**没有自定义分表规则**。

- [shardingJDBC按月分表，能否实现动态创建表，然后进行分表？](https://blog.csdn.net/qq_20417499/article/details/99748745)

  > 我获取datasource为null，后面看到作者说他没成功，我就没尝试了。

- [springboot结合MybatisPlus使用sharding-jdbc分库分表](https://blog.csdn.net/leilei1366615/article/details/114363369)

  > 这个**能自定义分表规则**，比较全。我主要参考的这个（包括版本）。[git地址](https://github.com/leilei0220/springboot-learn/tree/master/springboot-sharding-jdbc) [fork了一下](https://github.com/cqboyone/springboot-learn/tree/master/springboot-sharding-jdbc)

## 性能优化

- [spring boot--使用异步请求，提高系统的吞吐量](https://blog.csdn.net/liuchuanhong1/article/details/78744138)
- [SpringBoot学习--异步接口的配置和调用](https://blog.csdn.net/flygoa/article/details/83273081)

## 扩展

- [Spring Boot中使用LDAP来统一管理用户信息](http://blog.didispace.com/spring-boot-ldap-user/)

## 问题

- [springboot配置文件 application.yml注意事项（Failed to load property source from location 'classpath:/applica）](https://blog.csdn.net/linjy520/article/details/79455842)

- [springboot 启动报错 java.lang.IllegalStateException: Failed to introspect annotated methods on class org](https://www.cnblogs.com/jpfss/p/11089078.html)

- [使用thymeleaf 模板引擎时报错org.xml.sax.SAXParseException: 元素类型 “meta” 必须由匹配的结束标记 “” 终止](https://blog.csdn.net/a909422229/article/details/82967740)

- [详解SpringBoot应用跨域访问解决方案](https://juejin.im/post/6844903991558537223)

  > 我采用的 3.1.使用CorsFilter进行全局跨域配置
  >
  > 3.2也可以，如
  >
  > ```java
  > @Configuration
  > public class CustomWebMvcConfig implements WebMvcConfigurer {
  >  @Override
  >  public void addCorsMappings(CorsRegistry registry) {
  >      String[] allMethod = Arrays.stream(HttpMethod.values()).map(Enum::name).toArray(String[]::new);
  >      registry.addMapping("/**")
  >              .allowedOriginPatterns("*")
  >              .allowCredentials(true)
  >              .allowedMethods(allMethod)
  >              .maxAge(86400);
  >  }
  > }
  > ```

- [java 服务端设置跨域](https://www.jianshu.com/p/0c67823550d6)

  > 有效
  >
  > ```java
  > @Component
  > public class CORSInterceptor extends HandlerInterceptorAdapter {
  >  @Override
  >  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
  >      //添加跨域CORS
  >      response.setHeader("Access-Control-Allow-Origin", "*");
  >      response.setHeader("Access-Control-Allow-Headers", "X-Requested-With,content-type,token");
  >      response.setHeader("Access-Control-Allow-Methods", "GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS, PATCH");
  >      return true;
  >  }
  > }
  > 
  > public class WebConfigTest extends WebMvcConfigurerAdapter {
  >  @Autowired
  >  private CORSInterceptor corsInterceptor;
  > 
  >  @Override
  >  public void addInterceptors(InterceptorRegistry registry) {
  >      registry.addInterceptor(corsInterceptor);
  >  }
  > }
  > ```

- 

## 配置文件

- [@ConfigurationProperties 注解使用姿势，这一篇就够了](https://www.cnblogs.com/FraserYu/p/11261916.html)

  > 还不错。优雅配置文件首选。注意要加@Getter @Setter；然后prefix貌似要精确到最后一个单词前
  >
  > ```java
  > //sql.third.database.dcr-statistics-people-group-by-communities=select * from xxx
  > @Data
  > @Configuration
  > @PropertySource(value = "classpath:sql.properties", encoding = "UTF-8")
  > @ConfigurationProperties(prefix = "sql.third.database")
  > public class SqlProperties {
  >  private String dcrStatisticsPeopleGroupByCommunities;
  > }
  > ```

### @Value

- [Spring MVC 通过@Value注解读取.properties配置内容](https://www.cnblogs.com/xianan87/p/3942010.html)

  > 1. 如果是整型，使用Integer可以避免配置文件不填时报空指针。
  >
  > 2. @Value("${merge.time.offset:0}") 在properties里面没有该配置的时候赋值，但是如果有配置即使没有填也会替换成空
  >
  > 3. 直接写=可以指定默认值
  >
  > ```java
  > @Value("${merge.time.offset}")
  > Integer mergeTimeOffset = 0;
  > ```
  >
  > 4. 获取数组
  >
  > ```java
  > @Value("#{'${my.config.values:0,0.0,-1,-1.0}'.split(',')}")
  > private String[] illegalLL;
  > ```

- [Spring @Value 设置默认值](https://segmentfault.com/a/1190000023962377)

- [spring拾遗(一)——@Value注入static属性](https://www.cnblogs.com/toSeeMyDream/p/8177690.html)

- [spring.profiles.active和spring.profiles.include的使用与区别](https://blog.csdn.net/wysghmbb/article/details/107175416)

  - spring.profiles.active

    命名规则遵循application-${profile}.properties

  - spring.profiles.include



##### 扩展

- [@ConfigurationProperties绑定配置信息至Array、List、Map、Bean](https://blog.csdn.net/justry_deng/article/details/90758250)

  > [git地址](https://github.com/JustryDeng/CommonRepository/tree/master/Abc_ConfigurationProperties_Demo) 比较全，不错

- [SpringBoot @Value中文乱码解决](https://cloud.tencent.com/developer/article/1452211)

  1. 设置成utf-8

     <img src="../../../personalNote/notes/Java/springboot tips.assets/image-20210305095529150.png" alt="image-20210305095529150" style="zoom: 50%;" />

  2. 在使用配置文件的地方，设置encoding="utf-8"

     > ```java
     > @PropertySource(value="classpath:application.properties",encoding = "utf-8")
     > ```

  3. 在idea右下角，CRLF修改为CR然后在访问，就不乱码了。再次切换为CRLF即可了。


## 流&文件

- [SpringBoot项目jar发布获取jar包所在目录路径](https://blog.csdn.net/liangcha007/article/details/88526181)

- [解决springboot读取jar包中文件的问题](https://www.cnblogs.com/songxiaotong/p/11411123.html)

- [Springboot下jar如何读取静态配置JSON文件](https://my.oschina.net/boonya/blog/3171765)

  ```java
  public String getFileJson(String filePath) {
      try {
          ClassPathResource classPathResource = new ClassPathResource(filePath);
          byte[] bytes = FileCopyUtils.copyToByteArray(classPathResource.getInputStream());
          return new String(bytes);
      } catch (IOException e) {
          return null;
      }
  }
  ```

- [springboot上传大文件时内存溢出的可能解决办法](https://blog.csdn.net/huweijian5/article/details/85067412)

  > 这个提供了四种方法，我推荐用配置的方式，最简单。[Linux下 Spring Boot 上传找不到临时目录, 出现500错误](https://blog.csdn.net/lsvtogergo/article/details/105778648)
  >
  > 当然也可也用下面的代码解决（个人推荐），但是**注意如果web容器是Undertow，相对路径无法使用，需要配置为绝对路径。**
  >
  > ```java
  > //tomcat和Undertow做容器可以用
  > /**
  >  * 自定义临时文件目录，确保目录不会被系统删除导致抛异常
  > */
  > @Bean
  > public MultipartConfigElement multipartConfigElement() {
  >  MultipartConfigFactory factory = new MultipartConfigFactory();
  >  String location = fileProperties.getFileSpace();
  >  log.info("location={}", location);
  >  File tmpDir = new File(location);
  >  if (!tmpDir.exists()) {
  >      tmpDir.mkdirs();
  >  }
  >  factory.setLocation(tmpDir.getAbsolutePath());//这里设置绝对路径是因为Undertow需要绝对路径。
  >  return factory.createMultipartConfig();
  > }
  > ```

- 读取配置文件

  - URL url = this.getClass().getClassLoader().getResource("subscribe/kafka.json");

    > 这种方式比用线程获取好的地方是，单元测试不会出错。
    >
    > 线程获取不推荐：Thread.currentThread().getContextClassLoader().getResource("").getPath()
    >                         + File.separator + "subscribe/kafka.json")

### Excel

#### Apache poi

>耗内存，代码量大

#### EasyExcel

> 不会内存溢出。实际使用还可以

- [github项目](https://github.com/alibaba/easyexcel)

- [官方文档](https://www.yuque.com/easyexcel/doc/easyexcel)

- [JAVA解析Excel工具EasyExcel](https://github.com/alibaba/easyexcel)

  > 阿里巴巴出品，挺好用。[官方文档](https://www.yuque.com/easyexcel/doc/quickstart)

#### hutool

> 没测试



## 模板

- [第7章 Spring Boot集成模板引擎](https://www.jianshu.com/p/3f6a32b2a86d)

- [2.3 集成Freemarker](https://www.kancloud.cn/jaune162/springboot/212934)

  > 没有测试，先mark

## 规范

- [【项目实践】SpringBoot三招组合拳，手把手教你打出优雅的后端接口](https://www.jianshu.com/p/b5b8613769db)

## 代码优化

- [优化 if-else 代码的八种方案](https://mp.weixin.qq.com/s/ilhEcuSwGAa2e0diAp51LQ)

## 序列化

- [优雅的转义小项目接口中的字典值](https://juejin.im/post/5dba6a5ef265da4cef190537)

- [SpringBoot系列——Jackson序列化](https://www.cnblogs.com/huanzi-qch/p/11301453.html)

  ```java
  //序列化、反序列化时，格式化时间
  @JsonFormat(locale = "zh", timezone = "GMT+8", pattern = "yyyy-MM-dd HH:mm:ss")
  private Date createDate;
  ```

- 返回参数给前端出现Long精度丢失问题。PS：前端传入到后端不会丢失精度。

  - [修复Long类型太长，而Java序列化JSON丢失精度问题的方法](https://www.codepeople.cn/2020/04/17/SpringBoot2.x-Long2String/)

    > 在Long参数上加一行注解@JsonSerialize(using = ToStringSerializer.class)

  - [Spring Boot返回前端Long型丢失精度](https://www.jianshu.com/p/f46699ea331a)

    > 这个写的很详细，还有解析源码。有空看一下。
    >
    > 他的解决方法之一也有@JsonSerialize(using = ToStringSerializer.class)


## udp

- [Springboot启动Udp监听服务，创建socket,接收数据包](https://blog.csdn.net/ldddd_/article/details/105855414)

  > 使用[springboot线程池的使用和扩展](https://blog.csdn.net/boling_cavalry/article/details/79120268)的线程池来做线程池。  
  > 在contextInitialized方法中，我直接`@Autowired`的上面的线程池来运行（`.execute`）启动udp服务的方法。

- [SpringBoot开启UDP服务](https://github.com/dolyw/ProjectStudy/tree/master/SpringBoot/AsyncDemo#springboot%E5%BC%80%E5%90%AFudp%E6%9C%8D%E5%8A%A1)

  > 这个看起来更规范。我[fork](https://github.com/cqboyone/ProjectStudy)了

## Websocket

- [SpringBoot Websocket 实战](https://segmentfault.com/a/1190000023194240)

  > 有针对多服务推送消息方案。如果用户不在线，推送到redis。
  >
  > [源码路径](https://github.com/TavenYin/taven-springboot-learning/tree/master/sp-websocket)

- [SpringBoot 实现 Websocket 通信详解](http://www.mydlq.club/article/86/)



## 接口调用

### Fegin

- [Feign脱离 springcloud 在springboot项目中独立使用小结](https://blog.csdn.net/K1215400017/article/details/103314838)

  > 使用的 com.netflix.feign feign-core 调用接口

- [SpringCloud使用Feign对接第三方http接口](https://blog.csdn.net/u014131617/article/details/89002310)

  > 待尝试

- [为 springcloud feign 添加自定义headers](https://bishion.github.io/2019/05/29/spring-feign-headers/#%E4%B8%BA-springcloud-feign-%E6%B7%BB%E5%8A%A0%E8%87%AA%E5%AE%9A%E4%B9%89headers)

- [Spring Cloud中如何优雅的使用Feign调用接口](https://segmentfault.com/a/1190000012496398)

  > 备用

- [Feign调用时读取超时（Read timed out executing GET）解决方法](https://blog.csdn.net/hkl_Forever/article/details/116773655)

  ```properties
  # feign调用超时时间配置
  feign.client.config.default.connectTimeout=10000
  feign.client.config.default.readTimeout=60000
  ```

  

### RestTemplate

- [Springboot — 用更优雅的方式发HTTP请求(RestTemplate详解)](https://www.cnblogs.com/javazhiyin/p/9851775.html)

- 拦截器

  - [RestTemplate 微信接口 text/plain HttpMessageConverter](https://blog.csdn.net/kinginblue/article/details/52706155)

    我这样写的，报什么错就加什么解析。代码分别是在两个class里面。

    ```java
    public class CustomMappingJackson2HttpMessageConverter extends MappingJackson2HttpMessageConverter {
        public CustomMappingJackson2HttpMessageConverter(){
            List<MediaType> mediaTypes = new ArrayList<>();
            mediaTypes.add(MediaType.parseMediaType("text/json;charset=utf-8"));
            setSupportedMediaTypes(mediaTypes);
        }
    }
    @Bean
    RestTemplate defaultRestTemplate(RestTemplateTokenInterceptor restTemplateTokenInterceptor) {
        RestTemplate restTemplate = new RestTemplate();
        restTemplate.getMessageConverters().add(new CustomMappingJackson2HttpMessageConverter());
        return restTemplate;
    }
    ```


- 自定义异常处理


  - [自定义 RestTemplate 异常处理](https://ethendev.github.io/2018/11/06/RestTemplate-error-handler/)

    ```java
    public class CustomErrorHandler implements ResponseErrorHandler {
        @Override
        // 标示 Response 是否存在任何错误。实现类通常会检查 Response 的 HttpStatus。
        public boolean hasError(ClientHttpResponse response) throws IOException {
            return true;
        }
        @Override
        // 处理 Response 中的错误, 当 HasError 返回 true 时才调用此方法。用空方法替换掉默认的抛出异常的方法。这样就不会让resttemplate报错了
        public void handleError(ClientHttpResponse response) throws IOException {
        }
    }
    
    restTemplate.setErrorHandler(new CustomErrorHandler());
    ```

- restTemplate 转换错误。有的返回参数首字母为大写，但是dto会被自动转化为首字母小写。需要加上 @JsonProperty("DeviceInfo")

- 忽略ssl


  - [springboot restTemplate https请求 忽略ssl证书](https://cloud.tencent.com/developer/article/1825409)

  - [解决远程调用三方接口：javax.net.ssl.SSLHandshakeException报错](https://www.cnblogs.com/dongl961230/p/15594627.html)

    > 可用

## 服务

### resteasy

- [RestEasy 3.x 系列：使用Hibernate_Validator进行数据校验](https://blog.csdn.net/lazycheerup/article/details/84783620)

## 单元测试

- [SpringBoot Test及注解详解 ](https://www.cnblogs.com/myitnews/p/12330297.html)

- [springboot使用@SpringBootTest注解进行单元测试](https://blog.csdn.net/weixin_39220472/article/details/87714756)

- 关于springboot版本和spring-boot-starter-test兼容说明

  - spring-boot-starter-parent 2.3.10.RELEASE

    - 使用的是 org.junit.Test;

    - 使用spring容器需要引入

      ```java
      @RunWith(SpringRunner.class)
      @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
      ```

  - spring-boot-starter-parent 2.6.12.RELEASE

    - 使用的是 org.junit.jupiter.api.Test

    - 使用spring容器仅需要引入

      ```java
      @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
      ```

  - spring-boot-starter-parent 1.5.22.RELEASE

    - 使用的是 org.junit.Test;

    - 需要引入

      ```java
      @RunWith(SpringRunner.class)
      @SpringBootTest(classes = Main.class)
      ```

  

## 切面

### 日志处理

- [Spring boot中使用aop详解](https://www.cnblogs.com/bigben0123/p/7779357.html)

  > 讲解较详细，案例是操作日志

- [使用SpringBoot AOP 记录操作日志、异常日志](https://www.cnblogs.com/wm-dv/p/11735828.html)

  > 功能较全
  >
  > 里面的获取参数的方法不管用。我用的方法如下，可以获取get和post的参数。但是无法获取参数的name
  >
  > ```java
  > //若是post方法，获取的参数格式如：[{"pageNumber":1,"name":"1","pageSize":20}]
  > //若是get方法，获取的参数格式如：["a","d"]，缺少a和b对应的name
  > Object[] args = joinPoint.getArgs();
  > ```
  >
  > 

- 切面表达式

  execution 代表所要执行的表达式主体。如：`@Pointcut("execution(* com.xx.service.impl..*.*(..))")`

  - 第一处 * 代表方法返回类型 *代表所有类型
  - 第二处 包名代表aop监控的类所在的包
  - 第三处 .. 代表该包以及其子包下的所有类方法
  - 第四处 * 代表类名，*代表所有类
  - 第五处 *(..) *代表类中的方法名，(..)表示方法中的任何参数

## 部署

### 多环境

1. application.yml增加

   ```yaml
   spring:
     profiles:
       active: name(dev/prod)
   ```

2. 增加对应的application-{name}.yml文件



### 构建

#### Assembly

> 1. 如果本地没有maven插件，则plugins里面报红，可以先引用依赖，然后下载到本地后在删除引用
>
> ```xml
> <!-- 打包，下载到本地后就删除吧 -->
> <dependency>
>     <groupId>org.apache.maven.plugins</groupId>
>     <artifactId>maven-jar-plugin</artifactId>
>     <version>2.6</version>
> </dependency>
> <dependency>
>     <groupId>org.apache.maven.plugins</groupId>
>     <artifactId>maven-assembly-plugin</artifactId>
>     <version>3.1.1</version>
> </dependency>
> ```
>
> 2. 可以把程序的jar包排除掉，因为jar带了版本号，至少我不喜欢。因为启动jar包的脚本需要随着项目版本升级而改变。
>
> ```xml
> <dependencySets><dependencySet><excludes><exclude>${groupId}:${artifactId}</exclude></excludes></dependencySet></dependencySets>
> ```
>
> 3. 上面的jar排除掉了，可以把自定义名字的程序jar放进去lib文件夹或者放到任意位置
>
> 可以在<fileSets>里面添加；
>
> ```xml
> <!--最终生成lib文件夹，用于存放项目所需的Jar包-->
> <fileSet>
>     <directory>${project.build.directory}</directory>
>     <outputDirectory>lib</outputDirectory>
>     <includes>
>         <include>${project.artifactId}.jar</include>
>     </includes>
>     <fileMode>0755</fileMode>
> </fileSet>
> ```
>
> 也可以在<files>添加。不过我没测试过。
>
> ```xml
> <files>
>     <file>
>         <source>target/${project.artifactId}-${project.version}.jar</source>
>         <outputDirectory>boot/</outputDirectory>
>         <destName>${project.artifactId}-${project.version}.jar</destName>
>     </file>
> </files>
> ```

##### 多模块

- [Maven Assembly自定义打包插件](maven/Maven Assembly自定义打包插件)

  > 这个不错，已复制到本地。多模块项目我参考的这个。

##### 单模块

- [重剑无锋,大巧不工 SpringBoot --- 自定义打包部署，暴露配置文件和静态资源文件](maven/重剑无锋,大巧不工 SpringBoot --- 自定义打包部署，暴露配置文件和静态资源文件)

  > 单模块项目我参考的这个，已复制到本地。[原文](http://blog.joylau.cn/2017/12/12/SpringBoot-Assembly-Package/)

##### 参数详解

- [maven插件之maven-assembly-plugin的使用](https://blog.csdn.net/lucky_ly/article/details/102531465)

#### 问题

- 没有主属性清单

  - [Spring Boot:jar中没有主清单属性](https://blog.csdn.net/u010429286/article/details/79085212)

  - [jar 中没有主清单属性](https://blog.csdn.net/jiankunking/article/details/74498880)

    ```xml
    <!--修复assembly打包 没有主清单属性的时候，在configuration标签下加入-->
    <archive>
        <manifest>
        	<mainClass>com.XX.XX.XX.AAA</mainClass>
        </manifest>
    </archive>
    ```

## 文件服务

### 静态资源

- [Spring Boot静态资源访问和配置全解析(看不懂你打我)](https://blog.csdn.net/u010358168/article/details/81205116)

- [spring boot 2.x静态资源会被HandlerInterceptor拦截的原因和解决方法](https://my.oschina.net/dengfuwei/blog/1795346)

  > 有空看下源码。
  >
  > springboot2.x使用spring 5.x，静态资源也会执行自定义的拦截器，因此在配置拦截器的时候需要指定排除静态资源的访问路径

### 图片服务

- [springBoot优雅返回图片/网页到浏览器](https://www.cnblogs.com/myf008/p/10881780.html)

### 上传文件

- [java常见3种文件上传方式](https://blog.csdn.net/weixin_39640122/article/details/80244527)