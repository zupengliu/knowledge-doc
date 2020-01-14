## springboot多模块项目下，子模块调用报错：程序包xxxxx不存在
### 1. 场景描述
今天在用springboot搭建多模块项目，结构中有一个父工程Parent  一个通用核心工程core 以及一个项目工程A

当我在工程A中引入core时，没有问题，maven install正常

当我在工程A中使用core的类时，编译器没有报错，但是在maven install时就会报如下错误：

Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:2.3.2:compile (default-compile) on project A: Compilation failure: Compilation failure:
[ERROR] xxxxxController.java:[3,29] 错误: 程序包core.xx不存在
[ERROR]xxxxxController.java:[4,29] 错误: 程序包core.xx不存在



### 2. 解决方案：

最后发现原因是父工程使用的是springboot插件

```xml
<plugin>
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

把父工程中该插件删除，然后在具体的项目工程添加该插件，就正常了
