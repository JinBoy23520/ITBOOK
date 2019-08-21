#  一丶简介
MyBatis 本是[apache](https://baike.baidu.com/item/apache/6265)的一个开源项目[iBatis](https://baike.baidu.com/item/iBatis), 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github。

iBATIS一词来源于“internet”和“abatis”的组合，是一个基于Java的[持久层](https://baike.baidu.com/item/%E6%8C%81%E4%B9%85%E5%B1%82/3584971)框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAOs）

当前，最新版本是MyBatis 3.5.2 ，其发布时间是2019年7月15日。
# 二丶基本信息
MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生信息，将接口和 Java 的 POJOs(Plain Ordinary Java Object,普通的 Java对象)映射成数据库中的记录。
#　三丶 背景介绍
MyBatis 是支持普通 SQL查询，[存储过程](https://baike.baidu.com/item/%E5%AD%98%E5%82%A8%E8%BF%87%E7%A8%8B)和高级映射的优秀[持久层](https://baike.baidu.com/item/%E6%8C%81%E4%B9%85%E5%B1%82)框架。MyBatis 消除了几乎所有的[JDBC](https://baike.baidu.com/item/JDBC)代码和参数的手工设置以及[结果集](https://baike.baidu.com/item/%E7%BB%93%E6%9E%9C%E9%9B%86)的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的`POJOs`（Plain Ordinary Java Objects，普通的 Java对象）映射成数据库中的记录。

每个MyBatis应用程序主要都是使用`SqlSessionFactory`实例的，一个`SqlSessionFactory`实例可以通过`SqlSessionFactoryBuilder`获得。`SqlSessionFactoryBuilder`可以从一个xml配置文件或者一个预定义的配置类的实例获得。

用xml文件构建SqlSessionFactory实例是非常简单的事情。推荐在这个配置中使用类路径资源（classpath resource)，但你可以使用任何Reader实例，包括用文件路径或file://开头的url创建的实例。MyBatis有一个实用类----Resources，它有很多方法，可以方便地从类路径及其它位置加载资源。

#　四丶特点

![image.png](https://upload-images.jianshu.io/upload_images/5260759-5b31bd69948d0ff2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


*   简单易学：本身就很小且简单。没有任何第三方依赖，最简单安装只要两个jar文件+配置几个sql映射文件易于学习，易于使用，通过文档和源代码，可以比较完全的掌握它的设计思路和实现。

*   灵活：mybatis不会对应用程序或者数据库的现有设计强加任何影响。 sql写在xml里，便于统一管理和优化。通过sql语句可以满足操作数据库的所有需求。

*   解除sql与程序代码的耦合：通过提供DAO层，将业务逻辑和数据访问逻辑分离，使系统的设计更清晰，更易维护，更易单元测试。sql和代码的分离，提高了可维护性。

*   提供映射标签，支持对象与数据库的orm字段关系映射

*   提供对象关系映射标签，支持对象关系组建维护

*   提供xml标签，支持编写动态sql。

#　五丶总体流程

- (1)加载配置并初始化

触发条件：加载配置文件

处理过程：将SQL的配置信息加载成为一个个MappedStatement对象（包括了传入参数映射配置、执行的SQL语句、结果映射配置），存储在内存中。

- (2)接收调用请求

触发条件：调用Mybatis提供的API

传入参数：为SQL的ID和传入参数对象

处理过程：将请求传递给下层的请求处理层进行处理。

- (3)处理操作请求

触发条件：API接口层传递请求过来

传入参数：为SQL的ID和传入参数对象

处理过程：

(A)根据SQL的ID查找对应的MappedStatement对象。

(B)根据传入参数对象解析MappedStatement对象，得到最终要执行的SQL和执行传入参数。

(C)获取数据库连接，根据得到的最终[SQL](https://baike.baidu.com/item/SQL)语句和执行传入参数到数据库执行，并得到执行结果。

(D)根据MappedStatement对象中的结果映射配置对得到的执行结果进行转换处理，并得到最终的处理结果。

(E)释放连接资源。

(4)返回处理结果将最终的处理结果返回。

#　六丶 功能架构

[![MyBatis架构](https://upload-images.jianshu.io/upload_images/5260759-a16f59165d298e30.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://baike.baidu.com/pic/MyBatis/2824918/0/0b46f21fbe096b63ea0d41bf0c338744eaf8accc?fr=lemma&ct=single "MyBatis架构") MyBatis架构

我们把Mybatis的功能架构分为三层：

(1)API接口层：提供给外部使用的接口API，开发人员通过这些本地API来操纵数据库。接口层一接收到调用请求就会调用数据处理层来完成具体的数据处理。

(2)数据处理层：负责具体的SQL查找、SQL解析、SQL执行和执行结果映射处理等。它主要的目的是根据调用的请求完成一次数据库操作。

(3)基础支撑层：负责最基础的功能支撑，包括连接管理、事务管理、配置加载和缓存处理，这些都是共用的东西，将他们抽取出来作为最基础的组件。为上层的数据处理层提供最基础的支撑。

# 七丶框架架构

框架架构讲解：

(1)加载配置：配置来源于两个地方，一处是配置文件，一处是Java代码的注解，将SQL的配置信息加载成为一个

[![mybatis结构](https://upload-images.jianshu.io/upload_images/5260759-7dadfd0e634dc374.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://baike.baidu.com/pic/MyBatis/2824918/0/64380cd7912397dde0cd87eb5982b2b7d1a287ac?fr=lemma&ct=single "mybatis结构") mybatis结构

个MappedStatement对象（包括了传入参数映射配置、执行的SQL语句、结果映射配置），存储在内存中。

(2)SQL解析：当API接口层接收到调用请求时，会接收到传入SQL的ID和传入对象（可以是Map、JavaBean或者基本数据类型），Mybatis会根据SQL的ID找到对应的MappedStatement，然后根据传入参数对象对MappedStatement进行解析，解析后可以得到最终要执行的SQL语句和参数。

(3)SQL执行：将最终得到的SQL和参数拿到数据库进行执行，得到操作数据库的结果。

(4)结果映射：将操作数据库的结果按照映射的配置进行转换，可以转换成HashMap、JavaBean或者基本数据类型，并将最终结果返回。

# 八丶动态SQL


MyBatis 最强大的特性之一就是它的动态语句功能。如果您以前有使用JDBC或者类似框架的经历，您就会明白把SQL语句条件连接在一起是多么的痛苦，要确保不能忘记空格或者不要在columns列后面省略一个逗号等。动态语句能够完全解决掉这些痛苦。
　　尽管与动态SQL一起工作不是在开一个party，但是MyBatis确实能通过在任何映射SQL语句中使用强大的动态SQL来改进这些状况。动态SQL元素对于任何使用过JSTL或者类似于XML之类的文本处理器的人来说，都是非常熟悉的。在上一版本中，需要了解和学习非常多的元素，但在MyBatis 3 中有了许多的改进，现在只剩下差不多二分之一的元素。MyBatis使用了基于强大的OGNL表达式来消除了大部分元素。

# 八丶集成

略

单独使用mybatis是有很多限制的（比如无法实现跨越多个session的事务），而且很多业务系统本来就是使用spring来管理的事务，因此mybatis最好与spring集成起来使用。



