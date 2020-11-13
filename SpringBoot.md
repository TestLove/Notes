# 配置

## yaml语法

> 一般格式

```yaml
# '#'这个符号是注释
name: zqq #:后要有空格
# 可以存对象
## 空格
student:
 name: zqq
 age: 3
##行内
student: {name: zqq,age: 3}
# 数组
## 空格
pets:
 - cat
 - dog
pets: [cat,dog]
#& 用来建立锚点（defaults），<< 表示合并到当前数据，* 用来引用锚点
defaults: &defaults
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  <<: *defaults
 ################################################
 defaults:
  adapter:  postgres
  host:     localhost

development:
  database: myapp_development
  adapter:  postgres
  host:     localhost
```

> 具体实例

```java
@configurationProperties(prefix='person')//用于查找注入
public class person{}
```

```yaml
person: 
	name: zqq
	age: 11
	handsome: true
	birth: 2020/20/20
	maps: {k1: v1, k2: v2}
	lists:
		- code
		- music
		- girl
	dog:
		name: hgh
		age: 22
```

# controller层获取前端参数

> ![image-20201024112244640](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024112244640.png)

## @RequestParam

> 这里的@RequestBody用于读取Http请求的body部分数据——就是我们的请求数据。比如json或者xml。然后把数据绑定到 controller中方法的参数上

## @PathVariable

## @PathParam

> 就是从地址栏取参数值，采用的是传统的?name=唐&sex=男。@PathVariab在没有对应属性时会是一个null值，不会报错。

## @QueryParam

## @ResponseBody

> 放在controller层的方法上，将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。
> 使用时机：
> 当我们想让页面知道我们返回的数据不是按照html标签的页面来解析，而是其他某种格式的数据解析时（如json、xml等）

# Springboot流程

## Controller

> 与前端联系的接口

### 常用注解

#### @RestController

#### @Autowired

用于自定义类的属性注入

#### @Postmapping  or @GetMapping

用于接收前端的页面请求

## 启动类

### 常用注解

@Resource和@Autowired都是做bean的注入时使用，其实@Resource并不是Spring的注解，它的包是javax.annotation.Resource，需要导入，但是Spring支持该注解的注入

@SpringBootApplication

### 一般写法

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

## Entity

> 每一个类对应于数据库中的一张表properties与column相对应

### 常用注解

#### [@Value](https://blog.csdn.net/hry2015/article/details/72353994)



#### [@Resource与@AutoWired](https://www.cnblogs.com/think-in-java/p/5474740.html)

- 共同点:
  - 做bean的注入
  - 两者都可以写在字段和setter方法上。两者如果都写在字段上，那么就不需要再写setter方法
- 不同点
  - @AutoWired是spring自带,@Resource是jdk
  - @Autowired注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它的required属性为false。如果我们想使用按照名称（byName）来装配，可以结合==@Qualifier==注解一起使用。
  - @Resource默认按照ByName自动注入，由J2EE提供，需要导入包javax.annotation.Resource。@Resource有两个重要的属性：name和type，而Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以，如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不制定name也不制定type属性，这时将通过反射机制使用byName自动注入策略。

## Service

> 业务实现层,代码主要功能区,由接口与接口实现类构成

### 常用注解

@Service

## Dao/Mapper

> 持久层: 与数据库建立联系,进行增删改查



# 注意

1. 需要与前端约定传返回值得格式
2. 除搜索外按格式传值,如果是搜索,则直接传结果

# 错误

1. 程序跑通,但网页传参说500,报空指针,没有加@Autowired注解
2. sql类不存在,要写全限定名或者起别名
3. 类的属性与表的字段类型要一致
4. @RequiredArgsConstructor为常量和标注@NonNull的量提供构造函数

# Swagger2

> 接口文档生成框架

| <img src="https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090121733.png" alt="image-20201024090121733" style="zoom: 67%;" /> | ![image-20201024090205728](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090205728.png) |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 用于指定所需生成接口文档的接口位置                           | ![image-20201024090426704](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090426704.png) |
| ![image-20201024090552679](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090552679.png) | ![image-20201024090631218](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090631218.png) |
| 指定文档框架格式与版本                                       | ![image-20201024090917828](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090917828.png)![image-20201024090959453](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024090959453.png) |
| 比较完整的实例                                               | ![image-20201024091109452](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091109452.png) |
| ![image-20201024091430221](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091430221.png) | ![image-20201024091407586](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091407586.png) |
| ![image-20201024091606731](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091606731.png) | ![image-20201024091622232](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091622232.png) |
| ![image-20201024091933320](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091933320.png) | ![image-20201024091729360](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024091729360.png)单个参数 |
| ![image-20201024092253781](https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024092253781.png) | <img src="https://raw.githubusercontent.com/TestLove/Pictures/main/img/image-20201024092142163.png" alt="image-20201024092142163" style="zoom:150%;" />多个参数 |

# AOP

### 静态代理

#### 流程

1. 创建目标类或目标接口

   1. ```java
      public interface Greeting {
          void sayHello(String name);
      }
      ```

      
      

2. 代理类继承目标类或实现目标接口

   1. ```java
      public class GreetingProxy implements Greeting {
          private GreetingImpl greetingImpl;
          public GreetingProxy(GreetingImpl greetingImpl) {
              this.greetingImpl = greetingImpl;
          }
          @Override
          public void sayHello(String name) {
              before();
              greetingImpl.sayHello(name);
              after();
          }
          private void before() {
              System.out.println("Before");
          }
          private void after() {
              System.out.println("After");
          }
      }
      ```

      
      

3. 利用多态调用

   1. ```java
      public class Client {
          public static void main(String[] args) {
              Greeting greetingProxy = new GreetingProxy(new GreetingImpl());
              greetingProxy.sayHello("Jack");
          }
      }
      ```

   #### 缺点

   1、 当需要代理多个类的时候，由于代理对象要实现与目标对象一致的接口，有两种方式：

   - 只维护一个代理类，由这个代理类实现多个接口，但是这样就导致**代理类过于庞大**
   - 新建多个代理类，每个目标对象对应一个代理类，但是这样会**产生过多的代理类**

   2、 当接口需要增加、删除、修改方法的时候，目标对象与代理类都要同时修改，**不易维护**。

### 动态代理

#### jdk动态代理

#### cglib动态代理

#### Spring+AOP接口

#### Spring+Aspectj

```
Signature
```

