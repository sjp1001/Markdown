## JAVA

- 配置文件

  1. 在scr目录下新建文件xxx.properties

  2. 在xxx.properties中写入想要的参数等式
  3. 通过静态代码块和反射读取配置文件

  方法一：

  ```java
  package cn.itcast.reflect;
  
  import cn.itcast.domain.Person;
  import cn.itcast.domain.Student;
  
  import java.io.IOException;
  import java.io.InputStream;
  import java.lang.reflect.Method;
  import java.util.Properties;
  
  /**
   * 框架类
   */
  public class ReflectTest {
      public static void main(String[] args) throws Exception {
          //可以创建任意类的对象，可以执行任意方法
  
          /*
              前提：不能改变该类的任何代码。可以创建任意类的对象，可以执行任意方法
           */
  
          //1.加载配置文件
          //1.1创建Properties对象
          Properties pro = new Properties();
          
          //1.2加载配置文件，转换为一个集合
          //1.2.1获取class目录下的配置文件
          ClassLoader classLoader = ReflectTest.class.getClassLoader();
          InputStream is = classLoader.getResourceAsStream("pro.properties");
          pro.load(is);
  
          //2.获取配置文件中定义的数据
          String className = pro.getProperty("className");
          String methodName = pro.getProperty("methodName");
  
  
          //3.加载该类进内存
          Class cls = Class.forName(className);
          
          //4.创建对象
          Object obj = cls.newInstance();
          
          //5.获取方法对象
          Method method = cls.getMethod(methodName);
          
          //6.执行方法
          method.invoke(obj);
  
  
      }
  }
  ```

  方法二：

  ```java
  package cn.itcast.util;
  
  import java.io.FileReader;
  import java.io.IOException;
  import java.net.URL;
  import java.sql.*;
  import java.util.Properties;
  
  /**
   * JDBC工具类
   */
  public class JDBCUtils {
      private static String url;
      private static String user;
      private static String password;
      private static String driver;
      /**
       * 文件的读取，只需要读取一次即可拿到这些值。使用静态代码块
       */
      static{
          //读取资源文件，获取值。
  
          try {
              //1. 创建Properties集合类。
              Properties pro = new Properties();
  
              //获取src路径下的文件的方式--->ClassLoader 类加载器
              ClassLoader classLoader = JDBCUtils.class.getClassLoader();
              URL res  = classLoader.getResource("jdbc.properties");
              String path = res.getPath();
  
              //2. 加载文件
              pro.load(new FileReader(path));
  
              //3. 获取数据，赋值
              url = pro.getProperty("url");
              user = pro.getProperty("user");
              password = pro.getProperty("password");
              driver = pro.getProperty("driver");
              //4. 注册驱动
              Class.forName(driver);
          } catch (IOException e) {
              e.printStackTrace();
          } catch (ClassNotFoundException e) {
              e.printStackTrace();
          }
      }
  
  
      /**
       * 获取连接
       * @return 连接对象
       */
      public static Connection getConnection() throws SQLException {
  
          return DriverManager.getConnection(url, user, password);
      }
  
      /**
       * 释放资源
       * @param stmt
       * @param conn
       */
      public static void close(Statement stmt,Connection conn){
          if( stmt != null){
              try {
                  stmt.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
          if( conn != null){
              try {
                  conn.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
  
  
      /**
       * 释放资源
       * @param stmt
       * @param conn
       */
      public static void close(ResultSet rs,Statement stmt, Connection conn){
          if( rs != null){
              try {
                  rs.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
          if( stmt != null){
              try {
                  stmt.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
  
          if( conn != null){
              try {
                  conn.close();
              } catch (SQLException e) {
                  e.printStackTrace();
              }
          }
      }
  
  }
  ```

  





##  命令行中操作

- javac xxx.java

  将xxx.java编译成java.class



- javap xxx.class

  将xxx.class反编译成xxx.java



- javadoc xxx.java

  将xxx.java文件的doc文档提取出来
