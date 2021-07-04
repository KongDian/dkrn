# 第1章　搭建MyBatis源码环境

## 1.1 MyBatis 3简介

*MyBatis 能够流行起来的主要原因*

> 1. 消除了大量的 JDBC 冗余代码，包括参数配置，结果集封装等。
> 2. SQL语句可控制，方便查询优化，使用更加灵活。
> 3. 学习成本较低，对于新用户能够快速学习使用。
> 4. 提供了与主流IoC框架Spring的集成支持。
> 5. 引入缓存机制，提供了与第三方缓存类库的集成支持。

## 1.5 HSQLDB数据库简介

MyBatis源码项目中使用HSQLDB的内存模式作为单元测试数据库。

HSQLDB是纯Java语言编写的关系型数据库，支持大部分SQL-92、SQL:2008、SQL:2011规范。

支持Server模式和内存运行模式。

> HSQLDB官方文档：http://hsqldb.org/doc/2.0/guide/index.html。
>
> SQL-92官方文档：http://www.contrib.andrew.cmu.edu/~shadow/sql/sql1992.txt
>
> JDBC 4.2规范文档：https://download.oracle.com/otndocs/jcp/jdbc-4_2-mrel2-spec/index.html

