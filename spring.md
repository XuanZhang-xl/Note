# Spring

- [1. Spring的概述](#1)  
- [2. Spring的核心](#2)  
- [3. 注解](#3)  
- [](#4)  
- [](#5)  

## <span id = '1'>1. Spring的概述</span>
- Spring是分层的, JavaSE/EE一站式(full-stack), 轻量级开源框架.
    - 一站式
        - Spring提供了JavaEE各层的解决方案:
        - 表现层: struts1, struts2, Spring MVC
        - 业务层: Ioc, AOP, 事务控制
        - 持久层: JdbcTemplate, HibernateTemplate, ORM框架(对象关系映射)整合

    - 轻量级: Spring的出现取代了EJB的臃肿,低效,繁琐复杂,脱离现实的情况. 而且使用spring编程是非侵入式的. 
- Spring框架是一个分层架构, 它包含一系列的功能要素并被分为大约20个模块, 这些模块分为Core, Container, Data Access/Integration, Web, AOP(Aspect Oriented Programming), Instrumentation和测试部分. 如图:![spring结构](pic/springstrucure.png)
- 核心容器(Core Container) 包括Core、Beans、Context、EL模块。
    - 1：Core和Beans模块提供了Spring最基础的功能，提供IoC和依赖注入特性。这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要，并真正地允许你从程序逻辑中分离出依赖关系和配置。
    - 2：Context模块基于Core和Beans来构建，它提供了用一种框架风格的方式来访问对象，有些像JNDI注册表。Context封装包继承了beans包的功能，还增加了国际化（I18N）,事件传播，资源装载，以及透明创建上下文，例如通过servlet容器，以及对大量JavaEE特性的支持，如EJB、JMX。核心接口是ApplicationContext。
    - 3：Expression Language，表达式语言模块，提供了在运行期间查询和操作对象图的强大能力。支持访问和修改属性值，方法调用，支持访问及修改数组、容器和索引器，命名变量，支持算数和逻辑运算，支持从Spring 容器获取Bean，它也支持列表投影、选择和一般的列表聚合等。
- 数据访问/集成部分(Data Access/Integration)
    - 1：JDBC模块，提供对JDBC的抽象，它可消除冗长的JDBC编码和解析数据库厂商特有的错误代码。
    - 2：ORM模块，提供了常用的"对象/关系"映射APIs的集成层。 其中包括JPA、JDO、Hibernate 和 iBatis 。利用ORM封装包，可以混合使用所有Spring提供的特性进行"对象/关系"映射，如简单声明事务管理 。
    - 3：OXM模块，提供一个支持Object和XML进行映射的抽象层，其中包括JAXB、Castor、XMLBeans、JiBX和XStream。
    - 4：JMS模块，提供一套"消息生产者、消费者"模板用于更加简单的使用JMS，JMS用于用于在两个应用程序之间，或分布式系统中发送消息，进行异步通信。
    - 5：Transaction模块，支持程序通过简单声明性 事务管理，只要是Spring管理对象都能得到Spring管理事务的好处，即使是POJO，也可以为他们提供事务。
- Web
    - 1：Web模块，提供了基础的web功能。例如多文件上传、集成IoC容器、远程过程访问、以及Web Service支持，并提供一个RestTemplate类来提供方便的Restful services访问
    - 2：Web-Servlet模块，提供了Web应用的Model-View-Controller（MVC）实现。Spring MVC框架提供了基于注解的请求资源注入、更简单的数据绑定、数据验证等及一套非常易用的JSP标签，完全无缝与Spring其他技术协作。
    - 3：Web-Struts模块， 提供了对Struts集成的支持，这个功能在Spring3.0里面已经不推荐了，建议你迁移应用到使用Struts2.0或Spring的MVC。
    - 4：Web-Portlet模块，提供了在Portlet环境下的MVC实现
- AOP
    - 1：AOP模块，提供了符合AOP 联盟规范的面向方面的编程实现，让你可以定义如方法拦截器和切入点，从逻辑上讲，可以减弱代码的功能耦合，清晰的被分离开。而且，利用源码级的元数据功能，还可以将各种行为信息合并到你的代码中 。
    - 2：Aspects模块，提供了对AspectJ的集成。
    - 3：Instrumentation模块， 提供一些类级的工具支持和ClassLoader级的实现，可以在一些特定的应用服务器中使用。
- Test
    - 1：Test模块，提供对使用JUnit和TestNG来测试Spring组件的支持，它提供一致的ApplicationContexts并缓存这些上下文，它还能提供一些mock对象，使得你可以独立的测试代码。


## <span id = '2'>2. Spring的核心</span>
- IoC(Inverse of Control 控制反转): 将对象创建权利交给Spring工厂进行管理.比如说: 
 Book book = Spring工厂.getBook()
- AOP(Aspect Oriented Programming 面向切面编程), 基于动态代理的功能增强方式. 















































































































