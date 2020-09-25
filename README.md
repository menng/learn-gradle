
## Groovy语言

基于Groovy（JVM语言），Groovy增强了Java的很多功能。

## Gradle基础

每个待编译的module称作Project，每个Project都可以引入自己需要的插件，引入插件的目的是可以使用插件的Task。一个Project可以引入多个插件，一个插件可以包含多个Task。Gradle通过一个个Task来完成具体的构建任务。工程根目录下有个文件`settings.gradle`。

```jsx
rootProject.name = 'learngradle'
include 'project1'
include 'project2'
include 'project3'
```

工程根目录下有个build.gradle文件，每个project下面也有一个自己的build.gradle文件。根目录下的配置文件可以删除，非必须的，存在的意思是将子module项目的配置提取到根目录下的build.gradle中统一管理。

**[Settings对象](https://docs.gradle.org/current/dsl/org.gradle.api.initialization.Settings.html)**：Settings.gradle文件被翻译为Settings对象。

**[Project对象](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html)**：build.gradle文件被翻译为Project对象。

```jsx
// 配置完当前Project后立刻调用
afterEvaluate{
   project -> println "app module afterEvaluate： $project.name"
}

// 配置的是子项目的共同配置，作用于所有子project 
subprojects {
}

// 配置的是所有项目的共同配置，作用于当前以及所有子project 
allprojects{
}
```

**[Gradle对象](https://docs.gradle.org/current/dsl/org.gradle.api.invocation.Gradle.html)**：执行gradle命令时，框架会创建Gradle对象，Gradle对象最重要的就是在构建时加入各种回调。

```jsx
gradle.beforeProject{
   project -> println "beforeProject $project.name"
}

gradle.afterProject{
   project -> println "afterProject $project.name"
}

gradle.buildFinished{
   result -> println "buildFinished"
}
```

buildScript() 配置的是gradle脚本执行所需依赖。

### 引入插件

```groovy
plugins {
    id "java"
}
```

## Gradle工作时序

- Initialization，初始化阶段为每个module创建Project对象。settings.gradle也会被解析为Settings对象。
- Configuration，解析每个module的build.gradle文件，确定tasks执行顺序，并且task处于待执行状态。
- 执行Task，Task是gradle的执行单元。
- gradle.buildFinished，所有待执行Task执行完成后，收尾工作（可选）。

## Gradle中的Task

```groovy
// 自定义task
task aTask {
    println "sss"
    doFirst {
        println("aTask doFirst")
    }
    doLast {
        println("aTask doLast")
    }
}

// 指定分组
task aTask(group:'myGroup') {
    println "sss"
}

task bTask() {
    doFirst {
        println("bTask doFirst")
    }
}

// task之间的相互依赖
aTask.dependsOn bTask
// 或
task aTask(dependsOn: 'bTask') {
    println "sss"
    doFirst {
        println("aTask doFirst")
    }
}
```

## Gradle常用插件

- [Java](https://docs.gradle.org/current/userguide/java_plugin.html)
- [Idea](https://docs.gradle.org/current/userguide/idea_plugin.html)
- [maven-publish](https://docs.gradle.org/current/userguide/publishing_maven.html)

## G**radle常用命令**

### 执行Task

`gradle -q taskName`

### **查看依赖**

IDEA：help -> dependencies

`gradle dependencies` 

`gradle :{projectname}:dependencies`

### 查看模块

`gradle projects` 

## 示例项目

[https://github.com/menng/learngradle](https://github.com/menng/learngradle)

## FQA

- [allprojects 与 subprojects区别](https://blog.csdn.net/u013700502/article/details/85231687)
- [引入插件的两种方式的区别：apply plugin 与 plugins id](https://stackoverflow.com/questions/32352816/what-the-difference-in-applying-gradle-plugin)
- [[compileJava, compileTestJava, javadoc]*.options*.encoding = 'UTF-8'](https://stackoverflow.com/questions/26615235/explain-what-is-meant-by-this-code)
- [api、implementation和compile区别](https://www.notion.so/menng/Gradle-4ce7ebc205b640bf9f3c4ad677cd176e#341b8b93b83343c48470ac88e83a20d6)
- [using-gradle-to-build-a-jar-with-dependencies](https://stackoverflow.com/questions/4871656/using-gradle-to-build-a-jar-with-dependencies)
## 参考文档

- [gradle入门到实战](https://www.cnblogs.com/leipDao/p/10385155.html)
- [Gradle Build Language Reference](https://docs.gradle.org/current/dsl/index.html)
- [Gradle docs](https://docs.gradle.org/current/userguide/userguide.html)