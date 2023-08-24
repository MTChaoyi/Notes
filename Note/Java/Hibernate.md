# Hibernate ORM 概览

- JDBC(Java Database Connectivity): 它是提供了一组 Java API 来访问关系数据库的 Java 程序。这些 Java APIs 可以使 Java 应用程序执行 SQL 语句，能够与任何符合 SQL 规范的数据库进行交互。

- | **JDBC 的优点**      |   **JDBC 的缺点**   |
  | :------------------- | :-----------------: |
  | 干净整洁的 SQL 处理  | 大项目中使用很复杂  |
  | 大数据下有良好的性能 |   很大的编程成本    |
  | 对于小应用非常好     |      没有封装       |
  | 易学的简易语法       | 难以实现 MVC 的概念 |
  |                      |  查询需要指定 DBMS  |

- ORM 表示 Object-Relational Mapping (ORM)，是一个方便在关系数据库和类似于 Java， C# 等面向对象的编程语言中转换数据的技术。一个 ORM 系统相比于普通的 JDBC 有以下的优点。
  - 使用业务代码访问对象而不是数据库中的表
  - 从面向对象逻辑中隐藏 SQL 查询的细节
  - 基于 JDBC 的 'under the hood'
  -   没有必要去处理数据库实现
  -   实体是基于业务的概念而不是数据库的结构
  - 事务管理和键的自动生成
  -   应用程序的快速开发

# Hibernate 架构

![](https://atts.w3cschool.cn/attachments/image/wk/hibernate/hibernate_architecture.jpg)

- 配置对象

  配置对象是你在任何 Hibernate 应用程序中创造的第一个 Hibernate 对象，并且经常只在应用程序初始化期间创造。它代表了 Hibernate 所需一个配置或属性文件。配置对象提供了两种基础组件。

  - 数据库连接：由 Hibernate 支持的一个或多个配置文件处理。这些文件是 **hibernate.properties** 和 **hibernate.cfg.xml**。
  - 类映射设置：这个组件创造了 Java 类和数据库表格之间的联系。

- SessionFactory 对象

  配置对象被用于创造一个 SessionFactory 对象，使用提供的配置文件为应用程序依次配置 Hibernate，并允许实例化一个会话对象。SessionFactory 是一个线程安全对象并由应用程序所有的线程所使用。

  SessionFactory 是一个重量级对象所以通常它都是在应用程序启动时创造然后留存为以后使用。每个数据库需要一个 SessionFactory 对象使用一个单独的配置文件。所以如果你使用多种数据库那么你要创造多种 SessionFactory 对象。

- Session 对象

  一个会话被用于与数据库的物理连接。Session 对象是轻量级的，并被设计为每次实例化都需要与数据库的交互。持久对象通过 Session 对象保存和检索。

  Session 对象不应该长时间保持开启状态因为它们通常情况下并非线程安全，并且它们应该按照所需创造和销毁。

- Transaction 对象

  一个事务代表了与数据库工作的一个单元并且大部分 RDBMS 支持事务功能。在 Hibernate 中事务由底层事务管理器和事务（来自 JDBC 或者 JTA）处理。

  这是一个选择性对象，Hibernate 应用程序可能不选择使用这个接口，而是在自己应用程序代码中管理事务。

- Query 对象

  Query 对象使用 SQL 或者 Hibernate 查询语言（HQL）字符串在数据库中来检索数据并创造对象。一个查询的实例被用于连结查询参数，限制由查询返回的结果数量，并最终执行查询。

- Criteria 对象

  Criteria 对象被用于创造和执行面向规则查询的对象来检索对象。

# Hibernate 配置

> Hibernate 同时支持 xml 格式和传统的 properties 文件配置方式。（配置文件名默认为 hibernate.cfg.xml 和 hibernate.properties）
>
> xml 配置文件提供了更易读的结构和更强的配置能力，可以直接对映射文件加以配置，而在 properties 文件中则无法配置，必须通过代码中的 Hard Coding 加载相应的映射文件。所以一般采用 xml 型配置文件。
>
> 配置文件应部署在 CLASSPATH 中，对于 Web 应用而言，配置文件应放置在在
> \WEB-INF\classes 目录下。

## 配置

- hibernate.cfg.xml

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!DOCTYPE hibernate-configuration 
  	PUBLIC "-//Hibernate/Hibernate Configuration DTD//EN"
  	"http://hibernate.sourceforge.net/hibernate-configuration-2.0.dtd">
  <hibernate-configuration>
  <!-- SessionFactory 配置 -->
      <session-factory>
          <!-- 数据库URL -->
          <property name="hibernate.connection.url">
              jdbc:mysql://localhost/sample
          </property>
  
          <!-- 数据库JDBC驱动 -->
          <property name="hibernate.connection.driver_class">
              org.gjt.mm.mysql.Driver
          </property>
  
          <!-- 数据库用户名 -->
          <property name="hibernate.connection.username">
              User
          </property>
  
          <!-- 数据库用户密码 -->
          <property name="hibernate.connection.password">
              Mypass
          </property>
  
          <!-- dialect ，每个数据库都有其对应的Dialet以匹配其平台特性 -->
          <property name="dialect">
              net.sf.hibernate.dialect.MySQLDialect
          </property>
  
          <!-- 是否将运行期生成的SQL输出到日志以供调试 -->
          <property name="hibernate.show_sql">
              True
          </property>
  
          <!-- 是否使用数据库外连接 -->
          <property name="hibernate.use_outer_join">
              True
          </property>
  
          <!-- 事务管理类型，这里我们使用JDBC Transaction -->
          <property name="hibernate.transaction.factory_class">
              net.sf.hibernate.transaction.JDBCTransactionFactory
          </property>
  
          <!-- 映射文件配置，注意配置文件名必须包含其相对于根的全路径 -->
          <mapping resource="net/xiaxin/xdoclet/TUser.hbm.xml"/>
          <mapping resource="net/xiaxin/xdoclet/TGroup.hbm.xml"/>
      </session-factory>
  </hibernate-configuration>
  ```

- hibernate.properties

  ```properties
  hibernate.dialect net.sf.hibernate.dialect.MySQLDialect
  hibernate.connection.driver_class org.gjt.mm.mysql.Driver
  hibernate.connection.driver_class com.mysql.jdbc.Driver
  hibernate.connection.url jdbc:mysql:///sample
  hibernate.connection.username user
  hibernate.connection.password mypass
  ```

## 属性

|                属性                 |                          描述                           |
| :---------------------------------: | :-----------------------------------------------------: |
|         `hibernate.dialect`         | 这个属性使 Hibernate 应用为北选择的数据可生成适当的 SQL |
| `hibernate.connection.driver_class` |                     JDBC 驱动程序类                     |
|     `hibernate.connection.url`      |                  数据库实例的 JDBC URL                  |
|   `hibernate.connection.username`   |                      数据库用户名                       |
|   `hibernate.connection.password`   |                       数据库密码                        |
|  `hibernate.connection.pool_size`   |      限制在 Hibernate 应用数据库连接池中连接的数量      |
|  `hibernate.connection.autocommit`  |           允许在 JDBC 连接中使用自动提交模式            |

- 使用 JNDI 时的属性

  |                属性                 |                             描述                             |
  | :---------------------------------: | :----------------------------------------------------------: |
  |  `hibernate.connection.datasource`  |       在应用程序服务器环境中正在使用的应用程序 JNDI 名       |
  |       `hibernate.jndi.class`        |                  JNDI 的 InitialContext 类                   |
  | `hibernate.jndi.<JNDIpropertyname>` | 在 JNDI 的 InitialContext 类中通过任何你想要的 Java 命名和目录接口属性 |
  |        `hibernate.jndi.url`         |                       为 JNDI 提供 URL                       |
  |   `hibernate.connection.username`   |                         数据库用户名                         |
  |   `hibernate.connection.password`   |                          数据库密码                          |


# 映射文件

> 一个对象/关系型映射一般定义在 XML 文件中。映射文件指示 Hibernate 如何将已经定义的类或类组与数据库中的表对应起来。
>
> 尽管有些 Hibernate 用户选择手写 XML 文件，但是有很多工具可以用来给先进的 Hibernate 用户生成映射文件。这样的工具包括 **XDoclet**, **Middlegen** 和 **AndroMDA**。

## POJO 类

```java
public class Employee {
    private int id;
    private String firstName;
    private String lastName;
    private int salary;
    
    public Employee() {}
    public Employee(String fname, String lname, int salary) {
        this.firstName = fname;
        this.lastName = lname;
        this.salary = salary;
    }
    public int getId() {
        return id;
    }
    public void setId( int id ) {
        this.id = id;
    }
    public String getFirstName() {
        return firstName;
    }
    public void setFirstName( String first_name ) {
        this.firstName = first_name;
    }
    public String getLastName() {
        return lastName;
    }
    public void setLastName( String last_name ) {
        this.lastName = last_name;
    }
    public int getSalary() {
        return salary;
    }
    public void setSalary( int salary ) {
        this.salary = salary;
    }
}
```

## RDBMS 表

```sql
create table EMPLOYEE (
	id INT NOT NULL auto_increment,
    first_name VARCHAR(20) default NULL,
    last_name VARCHAR(20) default NULL,
    salary INT default NULL,
    PRIMARY KEY (id)
);
```

## Hibernate 配置文件

- 使用 Hibernate 关联已定义类与数据库表

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <!DOCTYPE hibernate-mapping PUBLIC 
   "-//Hibernate/Hibernate Mapping DTD//EN"
   "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
  <hibernate-mapping>
      <class name="Employee" table="EMPLOYEE">
          <meta attribute="class-description">
              This class contains the employee detail
          </meta>
          <id name="id" type="int" column="id">
              <generator class="native"/>
          </id>
          <property name="firstName" column="first_name" type="string"/>
          <property name="lastName" column="last_name" type="string"/>
          <property name="salary" column="salary" type="int"/>
      </class>
  </hibernate-mapping>
  ```

  需要以格式 `<classname>.hbm.xml` 保存映射文件。

  映射文件中使用的一些标签：

  - 以 `<hibernate-mapping>` 为根元素，包含所有 `<class>` 标签
  - `<class>` 标签是用来定义一个 Java 类到数据库表的特定映射。Java 的类名使用 `name` 属性表示，数据库表名用 `table` 属性表示
  - `<meta>` 标签是一个可选元素，可以被用来修饰类
  - `<id>` 标签将类中独一无二的 ID 属性与数据库表中的主键关联起来。id 元素中的 `name` 属性引用类的性质，`column` 属性引用数据库表名的列。`type` 属性保存 Hibernate 映射的类型，这个类型会将从 Java 转换成 SQL 数据类型
  - 在 `<id>` 元素中的 `<generator>` 标签用来自动生成主键值。设置 generator 标签中的 `class` 属性可以设置 `native` 使 Hibernate 可以使用 `identity, sequence 或 hilo` 算法根据底层数据库的情况创建主键
  - `<property>` 标签用来将 Java 类属性与数据库表的列匹配。标签中 `name` 属性引用的是类的性质，`column` 属性引用的是数据库表的列。`type` 属性保存 Hibernate 映射的类型，这个类型会将从 Java 转换成 SQL 数据数据类型

# 映射类型

- 原始类型
    |  映射类型   |          Java 类型           |    ANSI SQL 类型     |
    | :---------: | :--------------------------: | :------------------: |
    |   integer   |   int 或 java.lang.Integer   |       INTEGER        |
    |    long     |    long 或 java.lang.Long    |        BIGINT        |
    |    short    |   short 或 java.lang.Short   |       SMALLINT       |
    |    float    |   float 或 java.lang.Float   |        FLOAT         |
    |   double    |  double 或 java.lang.Double  |        DOUBLE        |
    | big_decimal |     java.math.BigDecimal     |       NUMERIC        |
    |  character  |       java.lang.String       |       CHAR(1)        |
    |   string    |       java.lang.String       |       VARCHAR        |
    |    byte     |    byte 或 java.lang.Byte    |       TINYINT        |
    |   boolean   | boolean 或 java.lang.Boolean |         BIT          |
    |   yes/no    | boolean 或 java.lang.Boolean | CHAR(1) ('Y' or 'N') |
    | true/false  | boolean 或 java.lang.Boolean | CHAR(1) ('T' or 'F') |

- 日期和时间类型
    |     date      |   java.util.Date 或 java.sql.Date    |   DATE    |
    | :-----------: | :----------------------------------: | :-------: |
    |     time      |   java.util.Date 或 java.sql.Time    |   TIME    |
    |   timestamp   | java.util.Date 或 java.sql.Timestamp | TIMESTAMP |
    |   calendar    |          java.util.Calendar          | TIMESTAMP |
    | calendar_date |          java.util.Calendar          |   DATE    |

- 二进制和大型数据对象

    |    binary    |                       byte[]                        | VARBINARY (or BLOB) |
    | :----------: | :-------------------------------------------------: | :-----------------: |
    |     text     |                  java.lang.String                   |        CLOB         |
    | serializable | any Java class that implements java.io.Serializable | VARBINARY (or BLOB) |
    |     clob     |                    java.sql.Clob                    |        CLOB         |
    |     blob     |                    java.sql.Blob                    |        BLOB         |

- JDK 相关类型

  |  class   |  java.lang.Class   | VARCHAR |
  | :------: | :----------------: | :-----: |
  |  locale  |  java.util.Locale  | VARCHAR |
  | timezone | java.util.TimeZone | VARCHAR |
  | currency | java.util.Currency | VARCHAR |

# 案例

- [POJO 类](##POJO-类)

- [数据库表](##RDBMS-表)

- [映射配置文件](##Hibernate-配置文件)

- 应用程序类

  ```java
  import java.util.List; 
  import java.util.Date;
  import java.util.Iterator; 
  
  import org.hibernate.HibernateException; 
  import org.hibernate.Session; 
  import org.hibernate.Transaction;
  import org.hibernate.SessionFactory;
  import org.hibernate.cfg.Configuration;
  
  public class ManageEmployee {
      private static SessionFactory factory; 
      public static void main(String[] args) {
          try{
              factory = new Configuration().configure().buildSessionFactory();
          } catch (Throwable ex) { 
              System.err.println("Failed to create sessionFactory object." + ex);
  			throw new ExceptionInInitializerError(ex); 
          }
          ManageEmployee ME = new ManageEmployee();
  
          /* Add few employee records in database */
          Integer empID1 = ME.addEmployee("Zara", "Ali", 1000);
          Integer empID2 = ME.addEmployee("Daisy", "Das", 5000);
          Integer empID3 = ME.addEmployee("John", "Paul", 10000);
  
          /* List down all the employees */
          ME.listEmployees();
  
          /* Update employee's records */
          ME.updateEmployee(empID1, 5000);
  
          /* Delete an employee from the database */
          ME.deleteEmployee(empID2);
  
          /* List down new list of the employees */
          ME.listEmployees();
      }
      /* Method to CREATE an employee in the database */
      public Integer addEmployee(String fname, String lname, int salary){
          Session session = factory.openSession();
          Transaction tx = null;
          Integer employeeID = null;
          try{
              tx = session.beginTransaction();
              Employee employee = new Employee(fname, lname, salary);
              employeeID = (Integer) session.save(employee); 
              tx.commit();
          } catch (HibernateException e) {
              if (tx!=null) tx.rollback();
              e.printStackTrace(); 
          } finally {
              session.close(); 
          }
          return employeeID;
      }
      /* Method to  READ all the employees */
      public void listEmployees( ){
          Session session = factory.openSession();
          Transaction tx = null;
          try{
              tx = session.beginTransaction();
              List employees = session.createQuery("FROM Employee").list(); 
              for (Iterator iterator = 
                             employees.iterator(); iterator.hasNext();){
                  Employee employee = (Employee) iterator.next(); 
                  System.out.print("First Name: " + employee.getFirstName()); 
                  System.out.print("  Last Name: " + employee.getLastName()); 
                  System.out.println("  Salary: " + employee.getSalary()); 
              }
              tx.commit();
          }catch (HibernateException e) {
              if (tx!=null) tx.rollback();
              e.printStackTrace(); 
          }finally {
              session.close(); 
          }
      }
      /* Method to UPDATE salary for an employee */
      public void updateEmployee(Integer EmployeeID, int salary ){
          Session session = factory.openSession();
          Transaction tx = null;
          try{
              tx = session.beginTransaction();
              Employee employee = 
                      (Employee)session.get(Employee.class, EmployeeID); 
              employee.setSalary( salary );
              session.update(employee); 
              tx.commit();
          } catch (HibernateException e) {
              if (tx!=null) tx.rollback();
              e.printStackTrace(); 
          }finally {
              session.close(); 
          }
      }
      /* Method to DELETE an employee from the records */
      public void deleteEmployee(Integer EmployeeID){
          Session session = factory.openSession();
          Transaction tx = null;
          try{
              tx = session.beginTransaction();
              Employee employee = 
                     (Employee)session.get(Employee.class, EmployeeID); 
              session.delete(employee); 
              tx.commit();
          } catch (HibernateException e) {
              if (tx!=null) tx.rollback();
              e.printStackTrace(); 
          }finally {
              session.close(); 
          }
          }
  }
  ```

# Hibernate 注释

> 使用注释可以直接替代 XML 映射文件
>
> 需要 JDK 5.0 及以上才支持注释

- 数据库表

  ```sql
  create table EMPLOYEE (
      id INT NOT NULL auto_increment,
      first_name VARCHAR(20) default NULL,
      last_name  VARCHAR(20) default NULL,
      salary     INT  default NULL,
      PRIMARY KEY (id)
  );
  ```

- 带注释的 Employee 类

  ```java
  import javax.persistence.*;
  
  @Entity
  @Table(name = "EMPLOYEE")
  public class Employee {
      @Id @GeneratedValue
      @Column(name = "id")
      private int id;
  
      @Column(name = "first_name")
      private String firstName;
  
      @Column(name = "last_name")
      private String lastName;
  
      @Column(name = "salary")
      private int salary;  
  
      public Employee() {}
      public int getId() {
        return id;
      }
      public void setId( int id ) {
        this.id = id;
      }
      public String getFirstName() {
        return firstName;
      }
      public void setFirstName( String first_name ) {
        this.firstName = first_name;
      }
      public String getLastName() {
        return lastName;
      }
      public void setLastName( String last_name ) {
        this.lastName = last_name;
      }
      public int getSalary() {
        return salary;
      }
      public void setSalary( int salary ) {
        this.salary = salary;
      }
  }
  ```

  - @Entity 注释

    EJB 3 标准的注释包含在 **javax.persistence** 包，所以我们第一步需要导入这个包。第二步我们对 Employee 类使用 **@Entity 注释**，标志着这个类为一个实体 bean，所以它必须含有一个没有参数的构造函数并且在可保护范围是可见的。

  - @Table 注释

    @table 注释允许您明确表的详细信息保证实体在数据库中持续存在。

    @table 注释提供了四个属性，允许您覆盖的表的名称，目录及其模式,在表中可以对列制定独特的约束。现在我们使用的是表名为 EMPLOYEE。

  - @Id 和 @GeneratedValue 注释

    每一个实体 bean 都有一个主键，你在类中可以用 **@Id** 来进行注释。主键可以是一个字段或者是多个字段的组合，这取决于你的表的结构。

    默认情况下，@Id 注释将自动确定最合适的主键生成策略，但是你可以通过使用 **@GeneratedValue** 注释来覆盖掉它。**strategy** 和 **generator** 这两个参数我不打算在这里讨论，所以我们只使用默认键生成策略。让 Hibernate 确定使用哪些生成器类型来使代码移植于不同的数据库之间。

  - @Column Annotation

    @Column 注释用于指定某一列与某一个字段或是属性映射的细节信息。您可以使用下列注释的最常用的属性:

    - **name** 属性允许显式地指定列的名称。

    - **length** 属性为用于映射一个值，特别为一个字符串值的列的大小。

    - **nullable** 属性允许当生成模式时，一个列可以被标记为非空。

    - **unique** 属性允许列中只能含有唯一的内容
