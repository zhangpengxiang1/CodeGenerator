# CodeGenerator
# MyBatis Plus 代码生成器

这是一个基于 MyBatis Plus 的代码生成器，可以根据数据库表自动生成 Entity、Mapper、Mapper XML、Service、ServiceImpl、Controller 等各种代码文件。

## 使用方法

1. 修改 `MPGenerator.java` 中的数据库连接信息。

2. 配置全局、包、策略等配置信息。

   ```java
   import com.baomidou.mybatisplus.generator.FastAutoGenerator;
   import com.baomidou.mybatisplus.generator.config.OutputFile;
   import com.baomidou.mybatisplus.generator.config.rules.NamingStrategy;
   import org.springframework.boot.test.context.SpringBootTest;
   
   import java.util.Collections;
   
   
   @SpringBootTest
   public class MPGenerator {
       public static void main(String[] args) {
           FastAutoGenerator.create("jdbc:mysql://127.0.0.1:3306/springsecurity?serverTimezone=Asia/Shanghai&characterEncoding=UTF-8&useUnicode=true&useSSL=false", "root", "root")
                   // 全局配置
                   .globalConfig(builder -> {
                       builder.author("yunfei") // 设置作者
                               .commentDate("yyyy-MM-dd hh:mm:ss")   //注释日期
                               .outputDir(System.getProperty("user.dir") + "/src/main/java") // 指定输出目录
                       //.disableOpenDir() //禁止打开输出目录，默认打开
                       ;
                   })
                   // 包配置
                   .packageConfig(builder -> {
                       builder.parent("com.yunfei") // 设置父包名
                               .pathInfo(Collections.singletonMap(OutputFile.xml, System.getProperty("user.dir") + "/src/main/resources/mappers")); // 设置mapperXml生成路径
                   })
                   // 策略配置
                   .strategyConfig(builder -> {
                       builder.addInclude("sys_menu", "sys_role", "sys_role_menu", "sys_user", "sys_user_role") // 设置需要生成的表名
                               .addTablePrefix("sys_") // 设置过滤表前缀
                               // Entity 策略配置
                               .entityBuilder()
                               .enableLombok() //开启 Lombok
                               .enableFileOverride() // 覆盖已生成文件
                               .naming(NamingStrategy.underline_to_camel)  //数据库表映射到实体的命名策略：下划线转驼峰命
                               .columnNaming(NamingStrategy.underline_to_camel)    //数据库表字段映射到实体的命名策略：下划线转驼峰命
                               // Mapper 策略配置
                               .mapperBuilder()
                               .enableFileOverride() // 覆盖已生成文件
                               // Service 策略配置
                               .serviceBuilder()
                               .enableFileOverride() // 覆盖已生成文件
                               .formatServiceFileName("%sService") //格式化 service 接口文件名称，%s进行匹配表名，如 UserService
                               .formatServiceImplFileName("%sServiceImpl") //格式化 service 实现类文件名称，%s进行匹配表名，如 UserServiceImpl
                               // Controller 策略配置
                               .controllerBuilder()
                               .enableFileOverride() // 覆盖已生成文件
                       ;
                   })
                   .execute();
       }
   }
   ```

3. 运行 `MPGenerator.java` 中的 `main` 方法即可自动生成代码。

4. 需要导入的依赖

   ```
   Mybatis-Plus代码生成器
   -->
   <dependency>
       <groupId>com.baomidou</groupId>
       <artifactId>mybatis-plus-generator</artifactId>
       <version>3.5.3</version>
   </dependency>
   <dependency>
       <groupId>org.apache.velocity</groupId>
       <artifactId>velocity-engine-core</artifactId>
       <version>2.3</version>
   </dependency>
   <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
   </dependency>
   ```

   

## 注意事项

1. 在运行代码生成器前，请确认数据库连接信息是否正确。
2. 代码生成器默认生成的代码放在 `src/main/java` 和 `src/main/resources` 目录下。
