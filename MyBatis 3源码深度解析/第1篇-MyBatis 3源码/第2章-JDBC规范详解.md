# 第2章 JDBC规范详解

> JDBC 4.2规范文档：https://download.oracle.com/otndocs/jcp/jdbc-4_2-mrel2-spec/index.html

## 2.1 JDBC API 简介

使用JDBC操作数据源大致需要以下几个步骤：

1. 与数据源建立连接。
2. 执行SQL语句。
3. 检索SQL执行结果。
4. 关闭连接。

### 2.1.1 建立数据库连接

JDBC API 中定义了Connection接口，用来表示与数据源的连接。

JDBC应用程序可以使用以下两种方式获取Connection对象。

- DriverManager
- DataSource

