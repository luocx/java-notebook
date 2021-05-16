# Spring 基础框架

[ioc]: #ioc 
[aop]: #aop 
[transaction]: #transaction 
## 目录
* [Spring IOC (Inversion of Control)][ioc]
* [Spring AOP (Aspect-oriented Programming)][aop]
* [Spring的事物隔离级别与传播特性][transaction]


## Spring IOC (Inversion of Control)
### IOC概念
控制反转也称作依赖注入，在面向对象编程语言中，原始的对象的创建方式里，对象何时创建、销毁以及对象的依赖属性的创建需要人为的去控制。
现在只要人为定义对象的描述信息，由框架去解析描述信息并生成对象以及注入其依赖项，开发者不需要干涉对象创建的过程，将对象的生命周期交给程序控制，这一过程称作**控制反转**。

### IOC容器
容器是IOC中的核心，用于实例化、配置和组装对象，`org.springframework.context.ApplicationContext`接口代表了spring IoC容器。  

### Bean
所有交由Spring IOC容器管理的对象都可以称为Bean，Bean是由容器通过读取配置文件、扫描注解和执行代码的方式获取到对象的元数据来创建的。

Bean包含以下属性：  

属性|描述
---|---
Class|实例化的类路径
Name|Bean 名称，可以指定多个用逗号分隔
Scope|作用域： singleton 单例（默认），prototype 多实例，request Http请求, session 会话， application 应用
Constructor arguments|构造参数对象，可以依赖注入Bean
Properties|对象属性，可以依赖注入Bean
Lazy initialization mode| 懒加载模式，默认不开启，单例的Bean被容器加载后立即被实例化，若开启则被使用时才实例化

### Bean的生命周期


## Spring AOP (Aspect-oriented Programming)
### AOP概念
面向方面编程（AOP）是对面向对象编程（OOP）的补充，它提供了另一种思考程序结构的方式。在OOP中，模块化的关键单位是类，而在AOP中，模块化的单位是方面。方面可以实现跨越多种类型和对象的关注点（如事务管理）的模块化。(在AOP文献中，这种关注点通常被称为 "交叉 "关注点)。  



## Spring的事物隔离级别与传播特性
### 事物的四大特性ACID
**原子性**  
**一致性**  
**隔离性**  
**持久化**  

---


**脏读** ：读取到其他事物未提交的修改，若事物发生回滚，则数据变成无效数据（脏数据）  
  
**不可重复读**：当前事物第一次读取到数据，然后数据被其他事物修改，当前事物再一次读取时跟第一次读取的不一致  
  
**幻读**：  第一个事物读取数据后，第二个事物插入了满足第一个事物查询条件的数据，第一个事物以相同条件再次读取，检索到额外的‘幻读’行。


### Spring五种事物隔离级别
 -|级别|说明
---|---|---
DEFAULT | 默认级别 | 使用底层的数据库默认隔离级别
READ_UNCOMMITTED | 可读未提交，该级别的事物，未提交的修改可以被另一个事物读取，可能出现脏读、不可重复读和幻读。
READ_COMMITTED | 可读已提交，该级别的事物禁止未提交的修改被读取，防止脏读，仍可能出现不可重复读和幻读。  
REPEATABLE_READ | 可重复读，防止脏读和不可重复读  
SERIALIZABLE | 序列化，防止脏读、不可重复读和幻读
  
### Spring七种事物传播特性
 特性| 说明
---|---
REQUIRED| **支持当前事物，如果当前没事物则开启新事物** ，一般都用做默认设置。
SUPPORTS| **支持当前事物，如果当前没事物则以非事物状态执行**
MANDATORY| **支持当前事物，如果当前没事物则抛出异常**
REQUIRES_NEW| **开启新的事物，如果当前有事物则将当前事物挂起**
NOT_SUPPORTED| **不支持当前事物，始终以非事物方式运行**
NEVER| **不支持当前事物，如果当前存在事物，则抛出异常**
NESTED| **如果当前存在事物，则在内嵌事物中执行**

## Spring IoC容器初始化过程

## Spring中的BeanFactory处理器
**BeanFactoryProcessor**: 用于配置BeanFactory  
**BeanDefinitionRegistryProcessor**: 用于处理注册Bean  
    *. ConfigurationClassPostProcessor 配置类处理器，对@Import、@ComponentScans、@ImportResource、@Bean 解析   

## Spring中的Bean处理器
**BeanPostProcessor**: 能够对bean做初始化前置处理和初始化后置处理  


## Spring 中的设计模式

### 工厂模式
BeanFactory  
PropertySourceFactory  