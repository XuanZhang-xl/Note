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
----
    //工厂类中的方法:
	public Object getBean(){
		Object bean = null;
		try {
			//传入类字符串,生产对象实例
			bean = Class.forName("xl.idea.pojo.Book").newInstance();
		} catch (Exception e) {
			e.printStackTrace();
		}
		//返回具体类型的对象类型实例
		return bean;
	}
    //那么只要通过工厂就可以获取对象
    Book book = (Book)Spring工厂.getBean()

- DI: Dependency Injection 依赖注入, 在Spring框架负责创建Bean对象时, 动态的将依赖对象注入到Bean组件(简单的说, 可以将另外一个bean对象动态的注入到另外一个bean中.)
- AOP(Aspect Oriented Programming 面向切面编程), 基于动态代理的功能增强方式. 

## <span id = '3'>3. Spring工厂</span>
- ApplicationContext直译为应用上下文, 是用来加载Spring框架配置文件, 来构建Spring的工厂对象, 它也称之为Spring容器的上下文对象, 也称之为Spring的容器. 
- ApplicationContext只是BeanFactory(Bean工厂, Bean就是一个java对象)一个子接口:
![Bean工厂](pic/springbeanfactory.png)
- 问:为什么不直接使用顶层接口对象来操作呢? 
- 答:ApplicationContext是对BeanFactory扩展, 提供了更多功能
    - 国际化处理
    - 事件传递
    - Bean自动装配
    - 各种不同应用层的Context实现
- 实例化Bean的四种方式
----
public class MyBean{
}
- 方式一:无参构造器(最常用)
----
    //applicationcontext.xml中:
    <!-- 默认构造器实例化对象 -->
	<bean id ="bean" class="xl.pojo.MyBean" />
    测试方法:
    public  void test(){
		// 创建spring工厂
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		// 默认构造器获取bean对象
		MyBean bean = (MyBean) ac.getBean("bean");
	}

- 方式二: 静态工厂方法
    // 创建静态工厂
    public class MyBeanFactory {
        // 静态方法，用来返回对象的实例
        public static MyBean getMyBean(){
            // 在做实例化的时候，可以做其他的事情，即可以在这里写初始化其他对象的代码
            // Connection conn....
            return new MyBean();
        }
    }
    <!-- 静态工厂获取实例化对象 -->
    <!-- class:直接指定到静态工厂类, factory-method: 指定生产实例的方法, spring容器在实例化工厂类的时候会自动调用该方法并返回实例对象 -->
    <bean id = "myBean" class="xl.pojo.MyBean" factory-method="getMyBean" />

    测试方法:
    public  void test(){
		// 创建spring工厂
		ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
		// 默认构造器获取bean对象
		MyBean bean = (MyBean) ac.getBean("bean");
	}

- 方式三: 实例工厂方法
----
    // 实例工厂: 必须new工厂 --> bean
    public class MyBeanFactory {
        // 普通的方法，非静态方法
        public MyBean getMyBean(){
            //初始化实例对象返回
            return new MyBean();
        }
    }

    <!-- 实例工厂的方式实例化bean -->
	<bean id="MyBeanFactory" class="xl.pojo.MyBean"/>
	<!-- factory-bean相当于ref：引用一个bean对象 -->
	<bean id="myBean" factory-bean="MyBeanFactory" factory-method="getMyBean"/>

- 方式四:FactoryBean方式
    // 实现FactoryBean接口的方式
    //泛型: 你要返回什么类型的对象, 泛型就是什么
    public class MyBeanFactoryBean implements FactoryBean<MyBean>{
        // 提供getObject方法, 返回目标类型对象
        public MyBean getObject() throws Exception {
            //写一些初始化数据库连接等等其他代码
            return new MyBean();
        }
        //这里获取泛型的实际类型的代码有待完善
        public Class<?> getObjectType() {
            return null;
        }
        //是否为单例
        public boolean isSingleton() {
            return false;
        }
    }

    <!-- 实现FactoryBean接口实例化对象 -->
	<!-- spring在实例化MyBeanFactoryBean的时候会判断是否实现了FactoryBean接口,如果实现了就调用getObject方法返回实例 -->
	<bean id="myBean" class="xl.pojo.MyBean" />
- 总结
    - 第一种最常用, 第二第三种一些框架初始化的时候用的多, 第四种Spring底层用的多。
    - BeanFactory和FactoryBean的区别？ 
    - BeanFactory: 是一个工厂(其实是构建了一个spring上下文的环境, 容器), 用来管理和获取很多Bean对象. 
    - FactoryBean, 是一个Bean生成工具, 是用来获取一种类型对象的Bean, 它是构造Bean实例的一种方式. 

## <span id = '4'>4. Bean的作用域</span>
- 项目开发中通常会使用: singleton 单例, prototype多例 
    - Singleton: 在一个spring容器中, 对象只有一个实例. (默认值)
    - Prototype: 在一个spring容器中, 存在多个实例, 每次getBean返回一个新的实例. 
- 单例是在Spring容器初始化的时候就初始化, 多例是在调用的时候初始化.
----
    <!-- 单例 -->
    <bean id="singletonBean" class="xl.pojo.SingletonBean"/>
    <!-- 多例 -->
    <bean id="prototypeBean" class="xl.pojo.PrototypeBean" scope="prototype"/>

![作用域](pic/springbeansingleton.png)

## <span id = '5'>5. Bean的初始化与销毁方法</span>
    // 步骤一: 创建LifeCycleBean，指定init方法, 和destroy方法。
    // 测试生命周期过程中的初始化和销毁bean
    public class LifeCycleBean {
        //定义构造方法
        public LifeCycleBean() {
            System.out.println("LifeCycleBean构造器调用了");		
        }
        //初始化后自动调用方法：方法名随意，但也不能太随便，一会要配置
        public void init(){
            System.out.println("LifeCycleBean-init初始化时调用");
        }
        //bean销毁时调用的方法
        public void destroy(){
            System.out.println("LifeCycleBean-destroy销毁时调用");
        }
    }

    //步骤二:配置Spring容器
    <!-- 生命周期调用的两个方法 
	init-method:初始化时（后）调用的，bean中的共有方法即可
	destroy-method:销毁时（前）被调用的。
	-->
	<bean id="lifeCycleBean" class="xl.spring.lifecycle.LifeCycleBean" init-method="init" destroy-method="destroy" />

    //步骤三: 测试代码:
    @Test
	public void test(){
		//先获取spring的容器，工厂，上下文
		ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
		//对于单例此时已经被初始化
		//获取bean
		LifeCycleBean lifeCycleBean=(LifeCycleBean) applicationContext.getBean("lifeCycleBean");
		System.out.println(lifeCycleBean);
		//为什么没有销毁方法调用。
		//原因是：使用debug模式jvm直接就关了，spring容器还没有来得及销毁对象。
		//解决：手动关闭销毁spring容器，自动销毁单例的对象
		((ClassPathXmlApplicationContext)applicationContext).close();
	}

## <span id = '6'>6. 后处理Bean: BeanPostProcessor接口</span>
- 后处理Bean也称之为Bean的后处理器, 作用是在Bean初始化的前后, 对Bean对象进行增强. 它既可以增强一个指定的Bean, 也可以增强所有的Bean, 底层很多功能(如AOP等)的实现都是基于它的, Spring可以在容器中直接识别调用. 


    // 步骤一: 创建MyBeanPostProcessor类, 实现接口BeanPostProcessor
    public class MyBeanPostProcessor implements BeanPostProcessor{
        // 初始化时（之前）调用的
        // 参数1：bean对象，参数2，bean的名字，id、name
        public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
    		System.out.println(beanName+"在初始化前开始增强了");
            // 如何只增强一个bean
            if(beanName.equals("lifeCycleBean")){
                System.out.println(beanName+"在初始化前开始增强了");
            }
            return bean;//放行
        }
        // 初始化时（之后）调用
        public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
    		System.out.println(beanName+"在初始化后开始增强了");
            if(beanName.equals("lifeCycleBean")){
                System.out.println(beanName+"在初始化后开始增强了");
            }
            return bean;
        }
    }

    //步骤二: 在Spring容器中注册
    <!-- 后处理bean: spring在初始化MyBeanPostProcessor的时候，判断是否实现了BeanPostProcessor，如果实现了，就采用动态代理的方式，对所有的bean对象增强 -->
	<bean class="xl.spring.BeanPostProcessor.MyBeanPostProcessor"/>


## <span id = '6'>6. Bean的依赖注入</span>

















































































































