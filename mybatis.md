## 目录
- [Mybatis特点](#1)
- [MyBatis架构](#2)
    - [mybatis-config.xml](#2-1)
    - [XxxMapper.xml](#2-2)
    - [接口UserMapper](#2-3)
    - [测试UserMapperTest](#2-4)
    - [自己写Dao实现类](#2-5)
- [mybatis-config 全局配置详解](#3)
- [Mapper.xml映射文件详解](#4)
- [查看文件内容](#5)
- [编辑文本内容](#6)
- [组合命令](#7)
- [网络相关命令](#8)
- [下载与安装](#9)
- [系统相关](#10)
- [用户及组管理](#11)
- [权限管理](#12)
- [软件的安装卸载与配置](#13)


### <span id = '1'>Mybatis特点</span>
- 支持自定义SQL、存储过程、及高级映射
- 实现自动对SQL的参数设置
- 实现自动对结果集进行解析和封装
- 通过XML或者注解进行配置和映射
- 实现Java对象与数据库表的映射转换


### <span id = '2'>MyBatis架构</span>
- 配置文件：
    - mybatis-condig.xml, 是MyBatis的全局配置文件,包含全局配置信息,如数据库连接参数,插件等.整个框架中只需要一个即可.
    - XxxMapper.xml,是映射文件,里面配置要执行的SQL语句,每个SQL对应一个Statement,可以有多个XxxMapper.xml文件
- Mybatis会通过SqlSessionFactoryBuilder来加载配置文件，生成一个SqlSessionFactory
    - 加载mybatis-config.xml和mapper.xml
    - 加载mapper.xml的时候，顺便会对Sql进行编译，形成statement
- 通过SqlSessionFactory建立连接，获取SqlSession对象
- MyBatis获取要执行的statement，进行自动参数设置
- SqlSession底层会通过Executor（执行器）来执行编译好的Statement，获取结果
- SQL的输入参数类型：
    - POJO，普通Java对象
    - HashMap，其实是POJO的Map形式， 键值对就是对象字段名和值z``
    - 各种基本数据类型
- 查询结果的输出形式
    - POJO，普通Java对象
    - HashMap，其实是POJO的Map形式， 键值对就是对象字段名和值
    - 各种基本数据类型

### <span id = '2'>MyBatis执行流程与例子</span>
- 编写配置文件（全局配置文件mybatis-config.xml和所有的mapper.xml映射文件）
    - 简单来说：就是准备JDBC连接参数以及要用到的Sql语句
- 加载配置，创建SqlSessionFactory
    - 这里获取连接参数，获取Sql，对Sql进行预编译，形成statement
- 通过SqlSessionFactory创建SqlSession
    - 这里就是调用了连接参数，连接数据库，形成会话
- 通过SqlSession执行statement，实现CRUD
    - 给前面编译好的statement设置Sql参数，然后执行
- 通过SqlSession提交事务
- 通过SqlSession关闭会话
- **注意事项**
    - 每个dao接口对应一个XxxMapper.xml文件,命名为XxxMapper是一种规范,并不是强制
    - XxxMapper.xml的namespace必须是接口的全类名
    - XxxMapper.xml中的每个statement的id必须是接口中对应的方法名
    - statement中定义的resultType必须和接口中对应方法的返回值一致

#### <span id = '2-1'>mybatis-config.xml</span>
```
<configuration>
    <!--加载外部配置属性,写有驱动,url,用户名,密码-->
    <properties resource="jdbc.properties"/>
    <!--运行环境,可以配置多个,default指向的是一个叫development的环境-->
    <environments default="development">
        <!--环境参数,id:环境的唯一标识符-->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!--从jdbc.properties中取值-->
                <property name="driver" value="${jdbc.mysql.driver}"/>
                <property name="url" value="${jdbc.mysql.url}"/>
                <property name="username" value="${jdbc.mysql.username}"/>
                <property name="password" value="${jdbc.mysql.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--指定所有的Mapper映射文件-->
    <mappers>
        <!--resource:在项目的resource目录下寻找-->
        <mapper resource="XxxMapper.xml"/>
    </mappers>
</configuration>
```


#### <span id = '2-2'>XxxMapper.xml</span>
```
<!--
定义所有要用到的sql语句和映射关系
namespace:当前这个Mapper文件的唯一标识,不能与其他Mapper文件重复
-->
<mapper namespace="xl.idea.mybatis.mapper.UserMapper">
    <!--定义一条sql,其实就是一个statement-->
    <!--
        select:表示这是一条查询语句,还有insert,delect,update
        id:唯一标识
        parameterType:sql的实际参数类型,写全类名
        resultType:返回结果集的数据类型,写全类名
    -->
    <select id="queryUserById" parameterType="java.lang.Long" resultType="xl.idea.mybatis.pojo.User">
        <!--
            这里写具体的sql语句,使用#{}作为参数的占位符,编译时,会替换为?然后注入真实参数
        -->
        SELECT *, user_name AS userName FROM tb_user WHERE id = #{id}
    </select>

    <!--
        查询全部用户,
        resultType:返回的结果虽然是一个List,这里依然写List中的元素类型,Mybatis会自动判断返回值个数并做封装
    -->
    <select id="queryAll" resultType="xl.idea.mybatis.pojo.User">
        SELECT *,user_name AS userName FROM tb_user
    </select>

    <!--添加用户信息-->
    <insert id="insertUser" parameterType="xl.idea.mybatis.pojo.User">
        <!-- 注意，这里用#{字段名}，mybatis会自动根据字段名，去User参数中找字段值，设置到Sql中 -->
        INSERT INTO  tb_user(
            id,
            user_name,
            password,
            name,
            age,
            sex,
            birthday,
            created,
            updated
        ) VALUES (
            NULL,
            #{userName},
            #{password},
            #{name},
            #{age},
            #{sex},
            #{birthday},
            NOW(),
            NOW()
        );
    </insert>

    <!--修改用户-->
    <update id="updateUser" parameterType="xl.idea.mybatis.pojo.User">
        UPDATE tb_user SET
            user_name = #{userName},
            password = #{password},
            name = #{name},
            age = #{age},
            sex = #{sex},
            birthday = #{birthday},
            updated = NOW()
        WHERE
            id = #{id};

    </update>

    <!--删除用户-->
    <delete id="deleteUserById" parameterType="java.lang.Long">
        DELETE from tb_user WHERE id = #{id}
    </delete>
</mapper>
```

#### <span id = '2-3'>接口UserMapper</span>
```
public interface UserMapper {
    /**根据ID来查询用户*/
    User queryUserById(Long id);
    /**查询所有*/
    List<User> queryAll();
    /**添加用户*/
    void insertUser(User user);
    /**修改用户信息*/
    void updateUser(User user);
    /**根据ID删除用户*/
    void deleteUserById(Long id);
}
```

#### <span id = '2-4'>测试UserMapperTest</span>
```
public class UserMapperTest {

    //使用sqlSeesion.getMapper获取到的dao的动态实现类
    private UserMapper userMapper;

    @Before
    public void setUp() throws Exception {
        // 设置资源路径
        String resource = "mybatis-config.xml";
        // 获取输入流，关联配置文件
        InputStream inputStream = Resources.getResourceAsStream(resource);
        // 读取配置，构建session工厂
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        // 获取session,同时打开事务的自动提交，每一次操作都是个独立事务
        SqlSession sqlSession = sqlSessionFactory.openSession(true);

        /*动态实现代理dao,也就是说dao只需要写接口,不需要写实现类了
        * 要求:
        * 1.每个dao接口对应一个XxxMapper.xml文件
        * 2.XxxMapper.xml的namespace必须是接口的全类名
        * 3.XxxMapper.xml中的每个statement的id必须是接口中对应的方法名
        * 4.statement中定义的resultType必须和接口中对应方法的返回值一致
        * */
        this.userMapper = sqlSession.getMapper(UserMapper.class);
    }

    @Test
    public void testQueryUserById() {
        // 测试根据id查询
        User user = userMapper.queryUserById(1L);
        System.out.println(user);
    }

    //...其他方法
}
```
#### <span id = '2-5'>自己写Dao实现类</span>
![](pic/mybatiswritedao.png)


### <span id = '3'>mybatis-config 全局配置详解</span>
**---------->这些配置在文件中的顺序很重要,必须严格按照以下顺序定义<--------------**
- properties属性
    - 用于引用外部属性文件,如jdbc.properties
    - 使用${}来获取外部引用文件中的属性
- settings设置,会改变 MyBatis 的运行时行为
    - cacheEnable 所有映射器重配置的缓存的全局开关,默认true.
    - lazyLodingEnable 延迟加载开关,当开启时,所有关联对象都会延迟加载。特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态。默认false.
    - aggressiveLazyLoading 当启用时,带有延迟加载属性的对象的加载与否完全取决于对任意延迟属性的调用,反之,每种属性将会按需加载。默认true.
    - multipleResultSetsEnabled 是否允许单一语句返回多结果集（需要兼容驱动）默认true
    - useColumnLabel 使用列标签代替列名。不同的驱动在这方面会有不同的表现，具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。默认true
    - mapUnderscoreToCamelCase 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。默认false
- typeAliases 类型命名
    ```
    <typeAliases>
        <!--方式一:给User类起一个别名,以后在配置中使用User,就代表xl.idea.mybatis.pojo.User-->
        <typeAlias type="xl.idea.mybatis.pojo.User" alias="User"/>
        <!--方式二:指定一个包,Mybatis会自动扫描包下所有的配,并给这些类设置默认的别名:就是类的名字-->
        <package name="xl.idea.mybatis.pojo"/>
    </typeAliases>
    ```
    - MyBatis中已经定义好的别名
![基本类型别名](pic/mybatisaliases.png)
- typeHandlers 类型处理器
    - 无论是在预处理语句(PreparedStatement)中设置一个参数时,还是从结果集中取出一个值时,Mybatis都会用类型处理器将获取的值以合适的方式转换成Java类型,而MyBatis中已经定义了很多的类型处理器,来解决Java类型与数据库类型的转换问题.所以,一般我们是不需要自定义的.
![类型处理器](pic/mybatistypehandlers.png)
- objectFactory 对象工厂
- plugins 插件
    - 类似于拦截器的效果，可以实现在MyBatis的整个运行流程中的 某些指定位置进行拦截：
        - Executor：对执行器进行拦截，上图括号内是可以拦截的方法
        - ParameterHandler：参数处理时进行拦截
        - ResultSetHandler：处理结果集，封装Java对象时进行拦截
        - StatementHandler：编译statement时进行拦截

    ![plugins](/pic/mybatisplugins.png)

- environments环境 default属性表明默认使用的环境,在获得session工厂时可以指定环境
    - environment 环境变量,可以配置多个环境
        - transactionManager 事务管理器
        - dataSource 数据源
    
    ![environment](pic/mybatisenvironment.png)

- databaseIdProvider 数据库厂商标识
- mapper 映射器
    ```
    <mappers>
        <!--resource:在项目的resource目录下寻找-->
        <mapper resource="UserMapper.xml"/>

        <!--使用本地/远程url,几乎不用-->
        <mapper url="file:///var/mappers/UserMapper.xml"/>

        <!--
            使用接口全类名
            这种方式需要把接口与Mapper文件放在一个包下吗?
        -->
        <mapper class = "xl.idea.mybatis.mapper.UseMapper"/>

        <!--
            配置Mapper接口所在包的名称,Mybatis会自动扫描接口及借口对应的Mapper文件
            这种方式需要把接口与Mapper文件放在一个包下,并且文件名与接口名一样,几乎不用
        -->
        <package name = "xl.idea.mybatis.mapper"/>
        
    </mappers>
    ```

### <span id = '4'>Mapper.xml映射文件详解</span>
![sql语句](pic/mybatissql.png)

- CRUD操作:insert/update/delete/select

- sql片段

- resultMap结果集映射

- cache/cache-ref 缓存
























