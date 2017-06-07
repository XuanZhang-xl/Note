## 切入点的表达式
----
    execution(void xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 无返回类型, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
  
- 表达式的写法
----
    execution(modifiers-pattern? (非必填项)--<访问修饰符>?
            ret-type-pattern (必填项)--<返回类型>
            declaring-type-pattern? (非必填项)
            name-pattern(param-pattern)(必填项)--<方法名>(<参数>)
            throws-pattern?(非必填项)<异常>?
                    )
    一共有5个参数
    其中的?表示非必填项

- 文档中写的: 
    - 除了返回类型模式(上面代码片断中的ret-type-pattern), 名字模式和参数模式以外,  所有的部分都是可选的. 
    - 返回类型模式决定了方法的返回类型必须依次匹配一个连接点.  你会使用的最频繁的返回类型模式是*, 它代表了匹配任意的返回类型. 
    - 一个全限定的类型名将只会匹配返回给定类型的方法. 名字模式匹配的是方法名.  你可以使用*通配符作为所有或者部分命名模式. 
    - 参数模式稍微有点复杂: ()匹配了一个不接受任何参数的方法,  而(..)匹配了一个接受任意数量参数的方法(零或者更多).  
    - 模式(`*`)匹配了一个接受一个任何类型的参数的方法.  模式`(*,String)`匹配了一个接受两个参数的方法, 第一个可以是任意类型,  第二个则必须是String类型. 更多的信息请参阅AspectJ编程指南中 语言语义的部分.  

- 例子:
----
    1: modifiers-pattern? (非必填项): 表示方法的修饰符
    execution(public void xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 共有方法, 无返回类型, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(private void xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 私有方法, 无返回类型, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    2: ret-type-pattern (必填项): 表示方法的返回类型
    execution(void xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 无返回类型, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
        execution(java.lang.String xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 返回类型String类型, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
        execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示: 返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    3: declaring-type-pattern? (非必填项): 表示包, 或者子包的, 或者类的修饰符
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* xl.e_xml.*.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml包中的所有子包, 包中UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* xl.e_xml.*.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml包中的所有类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* xl.e_xml..*.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml包中及其子包中的所有类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* *.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, 所有包中的所有类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, 所有包中的所有类, 类中的saveUser方法, 参数2个, 都是String类型
    4: name-pattern(param-pattern)(必填项): 方法的名称(方法的参数)
    (1)方法名称
        execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* xl.e_xml.a_before.UserServiceImpl.save*(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的以save开头的方法, 参数2个, 都是String类型
    execution(* xl.e_xml.a_before.UserServiceImpl.*(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的所有方法, 参数2个, 都是String类型
    
    
    (2)方法的参数
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.String))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 都是String类型
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,java.lang.Integer))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 参数1是String类型, 参数二是Integer
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(java.lang.String,*))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数2个, 参数1是String类型, 参数二是任意类型
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(*))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数1个, 参数是任意类型
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser())
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 没有参数
    execution(* xl.e_xml.a_before.UserServiceImpl.saveUser(..))
    * 表示返回类型任意, xl.e_xml.a_before包中的UserServiceImpl类, 类中的saveUser方法, 参数任意(可以是0个, 也可以多个)
    5: throws-pattern?(非必填项): 方法上抛出的异常

- 项目开发中表达式(最多用)
----
    1: execution(* xl.procject.service..*.*(..))
    * 返回类型任意, xl.procject.service包及其子包中所有类, 类中所有方法, 参数任意
    2: execution(* *..*.*(..))
    * 返回类型任意, 任意包中及其子包中所有类, 类中所有方法, 参数任意
    3: execution(* *(..))
    * 返回类型任意, 任意包中及其子包中所有类, 类中所有方法, 参数任意


- 下面给出一些通用切入点表达式的例子. 
----
    任意公共方法的执行: 
    execution(public * *(..))

    任何一个名字以“set”开始的方法的执行: 
    execution(* set*(..))

    AccountService接口定义的任意方法的执行: 
    execution(* com.xyz.service.AccountService.*(..))

    在service包中定义的任意方法的执行: 
    execution(* com.xyz.service.*.*(..))

    在service包或其子包中定义的任意方法的执行: 
    execution(* com.xyz.service..*.*(..))
