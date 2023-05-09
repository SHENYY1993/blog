---
layout: post
title:  "SpringBoot多模块项目Swagger配置"
categories: SpringBoot
tags:  后端 SpringBoot Swagger
author: SHENYY
---

* content
{:toc}

## 1.引言
Swagger是一组开源工具，用于设计、构建、记录和使用RESTful API（Representational State Transfer）。Swagger定义了一个接口描述语言（IDL）以及一组工具，用于生成、文档化和测试RESTful APIs。它提供了一个可视化的界面，让开发者可以轻松地测试API，而无需编写代码或使用命令行工具。

Swagger的主要目标是提高RESTful API的开发效率和可维护性。通过使用Swagger，开发人员可以更快地构建API，生成文档，并自动生成代码，从而大大减少了开发时间和成本。

以下介绍SpringBoot多模块项目Swagger的配置。




## 2.配置过程
### 2.1添加Swagger依赖
父模块的pom.xml文件添加swagger依赖
```
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
 <dependency>
     <groupId>io.swagger</groupId>
     <artifactId>swagger-models</artifactId>
      <version>1.5.21</version>
</dependency>
<dependency>
     <groupId>io.swagger</groupId>
     <artifactId>swagger-annotations</artifactId>
</dependency>
 
```
### 2.2创建Swagger配置类
例如在父模块的src/main/java目录下创建一个名为SwaggerConfig的类，并添加以下内容：
```
@Configuration
@EnableSwagger2
public class SwaggerConfig {
 
    
    /**
     * Module API
     */
    @Bean
    public Docket createRestApiModule() {
        return new Docket(DocumentationType.SWAGGER_2)
                // 用来创建该API的基本信息，展示在文档的页面中（自定义展示的信息）
                .apiInfo(apiInfo())
                // 标记以启用或禁用可能使用属性文件加载的
                .enable(true)
                // 设置哪些接口暴露给Swagger展示
                .select()
                // 扫描所有有注解的api，用这种方式更灵活
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                // 扫描指定包中的swagger注解
                .apis(RequestHandlerSelectors.basePackage("com.example.module"))
                .paths(PathSelectors.any())
                .build().groupName("XXX");
    }
 
    /**
     * Module2 API
     */
    @Bean
    public Docket createRestApiModule2() {
        return new Docket(DocumentationType.SWAGGER_2)
                // 用来创建该API的基本信息，展示在文档的页面中（自定义展示的信息）
                .apiInfo(apiInfo())
                // 标记以启用或禁用可能使用属性文件加载的
                .enable(true)
                // 设置哪些接口暴露给Swagger展示
                .select()
                // 扫描所有有注解的api，用这种方式更灵活
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                // 扫描指定包中的swagger注解
                .apis(RequestHandlerSelectors.basePackage("com.example.module2"))
                .paths(PathSelectors.any())
                .build().groupName("XXX");
    }
    
    /**
     * 摘要信息
     */
    private ApiInfo apiInfo() {
        // 用ApiInfoBuilder进行定制
        return new ApiInfoBuilder().title("XXX")// 设置文档的标题
                .description("描述：XXXAPI文档")// 设置文档的描述
                .version("XXX")// 设置文档的版本信息
                .build();
    }
}
 
```

这里的com.example.module、com.example.module2应该替换为你实际的控制器所在的包名。

### 2.3添加Swagger注解描述
在子模块中使用Swagger注解来描述API，例如在子模块的Controller类上添加如下注解：

```
@Api(tags = "用户相关接口")
@RestController
@RequestMapping("/user")
public class UserController {
 
    @ApiOperation("获取用户列表")
    @GetMapping("")
    public List<User> getUsers() {
        // ...
    }
 
    @ApiOperation("获取单个用户信息")
    @GetMapping("/{id}")
    public User getUser(@PathVariable("id") Long id) {
        // ...
    }
 
    @ApiOperation("添加用户")
    @PostMapping("")
    public User addUser(@RequestBody User user) {
        // ...
    }
 
    @ApiOperation("更新用户信息")
    @PutMapping("/{id}")
    public User updateUser(@PathVariable("id") Long id, @RequestBody User user) {
        // ...
    }
 
    @ApiOperation("删除用户")
    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable("id") Long id) {
        // ...
    }
}
```

这里的@Api、@ApiOperation等注解是Swagger提供的，用于描述API的基本信息，例如接口名称、请求方式、请求参数等。

### 2.4访问Swagger UI界面
启动应用程序，并访问http://localhost:port/swagger-ui.html（其中port是你的应用程序端口号），可以看到Swagger UI界面，可以在这里查看API文档和测试接口。





