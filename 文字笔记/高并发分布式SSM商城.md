# manong商城（高并发分布式SSM项目）

## 技术选型

- maven

## 创建父模块和子模块

1. 创建空的Maven项目（不使用Maven模版）

   ![1557587811016](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建项目1.png)

   ![1557587907420](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建项目2.png)

   ---

2. 删除该项目下的src/、pom.xml

   > 因为该空项目是用于管理项目模块的，不需要代码，即是充当目录用的。

   ![1557588078566](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建项目3.png)

   ---

3. 创建父模块

   ![1557588257866](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建父模块1.png)

   ![1557588304081](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建父模块2.png)

   ![1557589183948](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建父模块3.png)

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

   ​	![1557591358733](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建子模块1.png)

   <br>

   - pojo模块

     > pojo模块里开发配置了项目所用到的所有实体类，其他模块例如后台模块查询数据返回需要实体类去承载，所以可通过聚合方式来引入该pojo模块（添加Jar依赖）。

     ​	![1557591514522](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建子模块2.png)

     ![1557591631127](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建子模块3.png)

     ![1557591882873](D:\ChangefunStudy\JAVA_FrameWork_NOTE\SSM整合\文字笔记\img\创建子模块4.png)

     ---

   - mapper模块

     > 该模块用于开发配置Mybatis的映射文件和对应的映射类（dao层）