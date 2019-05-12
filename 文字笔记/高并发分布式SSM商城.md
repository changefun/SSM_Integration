# manong商城（高并发分布式SSM项目）

## 技术选型

- maven

## 创建父模块和子模块

1. 创建空的Maven项目（不使用Maven模版）

   ![1557587811016](.\img\创建项目1.png)

   ![1557587907420](.\img\创建项目2.png)

   ---

2. 删除该项目下的src/、pom.xml

   > 因为该空项目是用于管理项目模块的，不需要代码，即是充当目录用的。

   ![1557588078566](.\img\创建项目3.png)

   ---

3. 创建父模块

   ![1557588257866](.\img\创建父模块1.png)

   ![1557588304081](.\img\创建父模块2.png)

   ![1557589183948](.\img\创建父模块3.png)

4. 父模块pom进行配置

   ```xml
   
       <!--当前模块的打包格式-->
       <!--由于父模块只用到pom来管理子模块，所以打包格式为pom-->
       <packaging>pom</packaging>
   
   
       <!--配置子模块-->
       <modules>
   
       </modules>
   
   
       <!--可以设置一些变量，然后在pom其他位置引用，一般用于配置依赖的版本变量-->
       <properties>
           <junit.version>4.12</junit.version>
           <maven-resource-plugin.version>3.0.2</maven-resource-plugin.version>
           <maven-compliler-plugin.version>3.8.0</maven-compliler-plugin.version>
       </properties>
   
   
       <!--声明项目所需依赖（不会真正引入依赖，而是声明依赖）-->
       <dependencyManagement>
           <dependencies>
               <!--用于单元测试的Jar-->
               <dependency>
                   <groupId>junit</groupId>
                   <artifactId>junit</artifactId>
                   <version>${junit.version}</version>
               </dependency>
   
           </dependencies>
       </dependencyManagement>
   
   
   
       <build>
           <!--声明插件（子模块需要显式引入才真正引入）-->
           <pluginManagement>
               <plugins>
   
               </plugins>
           </pluginManagement>
   
   
           <!--build>plugins下才会真正引入插件，子模块自动继承该插件-->
           <plugins>
   
               <!--该插件用于拷贝resource/下的资源到war包-->
               <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-resources-plugin</artifactId>
                   <version>${maven-resource-plugin.version}</version>
                   <configuration>
                       <encoding>utf-8</encoding>
                   </configuration>
               </plugin>
   
   
               <!-- 该插件用于编译代码-->
              <plugin>
                   <groupId>org.apache.maven.plugins</groupId>
                   <artifactId>maven-compiler-plugin</artifactId>
                   <version>${maven-compliler-plugin.version}</version>
                   <configuration>
                       <compilerVersion>1.8</compilerVersion>
                       <source>1.8</source><!--源代码jdk版本-->
                       <target>1.8</target><!--.class字节码jdk版本-->
                       <encoding>utf-8</encoding><!--编译编码-->
                   </configuration>
               </plugin>
           </plugins>
   
       </build>
   
   ```

   ---

5. 创建子模块

   - commom模块

     > 该模块用于开发配置项目所用到的工具类，它将聚合到其他模块。

     ![1557632763053](.\img\创建子模块1.png)

     ![1557632801254](.\img\创建子模块2.png)

     ![1557632906124](.\img\创建子模块3.png)

     ![1557632997542](.\img\创建子模块4.png)

     ---

   - rest模块

     > 该模块用于项目支持各种客户端，如PC，Android、IOS。
     >
     > 这里暂时不管打包形式。

   - redis模块

     > 缓存模块，复制项目缓存，提高项目的性能。

   - search模块

     > 搜索模块，商城搜索商品或其他信息。

   - sso模块

     > 单点登陆模块，一点登陆，多处权访。

   - manager模块

     > 后台管理系统模块。

     ![1557634009279](.\img\创建子模块5.png)

     

---

## manager模块

> 后台管理系统是一个web应用，需要将它配置成web应用。
>
> 将web应用的三层MVC架构分为子模块配置。
>
> 依赖关系：web>service>mapper>pojo

- pojo模块

  > 实体类模块，manager需要用到的所有实体类。

  ![1557635008653](.\img\manager模块.png)

  ![1557635097793](.\img\manager模块2.png)

  ![1557635281588](.\img\manager模块3.png)

  ---

- mapper模块

  > 该模块用于配置mybatis使用到的查询映射文件和dao接口，属于dao层，负责查询数据库。
  >
  > 该模块将聚合到service模块作为依赖，同时该模块也聚合了pojo模块。

  ![1557635689581](.\img\manager模块4.png)

  ---

- service模块

  > 业务层模块，依赖mapper模块，也作为web模块的依赖。还依赖pojo模块。
  >
  > pojo模块由于被mapper模块引入，servies模块引入mapper模块后也间接引入pojo模块。

  ![1557636394493](.\img\manager模块5.png)

  ---

- web模块

  > 该模块为web应用，将使用springmvc面向客户端请求。
  >
  > 依赖于service模块。

  - 创建模块后

    - 创建模块后

      ![1557640546788](.\img\manager模块6.png)

    - 配该模块为web应用

      ![1557640777132](.\img\manager模块7.png)

      ![1557640975159](.\img\manager模块8.png)

      ![1557641175303](.\img\manager模块9.png)

      ![1557641310772](.\img\manager模块10.png)

    - 配置Tomcat服务器程序

      ![1557641396886](.\img\manager模块11.png)

      ![1557641453615](.\img\manager模块12.png)

      ![1557641525502](.\img\manager模块13.png)

      ![1557641617226](.\img\manager模块14.png)

      ![1557641761051](.\img\manager模块15.png)

      > 阶段性测试：点击绿色播放按钮运行web应用，成功后浏览器自动打开index.jsp。
      >
      > 虽然该模块pom没有配置编译等插件，但maven默认有基本插件帮助我们完成这一系列工作。