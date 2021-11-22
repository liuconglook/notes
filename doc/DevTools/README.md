## DevTools

### VSCode

#### 插件

- Java Extension Pack
  - Language Support for Java
  - Debugger for Java
  - Java Test Runner
  - Maven for Java
  - Project Manager for Java
  - Visual Studio ==IntelliCode==
- Spring-boot-pack
  - Spring Boot Tools
  - Spring Initializr Java Support
  - XML

- ==Lombok== Annotations Support for VS Code
- ==markdown==lint（markdown文件支持）
- theme（主题）
  - Remedy
  - https://vscodethemes.com/

#### 配置

- User（用户配置）
- Workspace（工作区配置）

一般登录后，配置到用户

~~~json
# Java Test Runner 配置
"java.test.config": {
    "name": "myConfiguration",
    "workingDirectory": "${workspaceFolder}",
    "vmArgs": [
        "-Xmx512M"
    ],
    "env": {
        "key": "value"
    }
},
# javaJDK运行环境配置
"java.configuration.runtimes": [
    {
        "name": "JavaSE-1.8",
        "path": "C:\\Program Files\\Java\\jdk1.8.0_144",
    },
    {
        "name": "JavaSE-9",
        "path": "C:\\Program Files\\Java\\jdk-9.0.4",
    },
    {
        "name": "JavaSE-11",
        "path": "C:\\Program Files\\Java\\jdk-11.0.9",
    }
],
# javaHome默认指定JDK11，以运行 Language Server for Java扩展的要求
"java.home": "C:\\Program Files\\Java\\jdk-11.0.9",
# 配置maven
"java.configuration.maven.globalSettings": "C:\\Program Files\\Java\\apache-maven-3.6.1\\conf\\settings.xml",
"maven.executable.path": "C:\\Program Files\\Java\\apache-maven-3.6.1\\bin\\mvn.cmd"
~~~

#### 命令

##### 创建Maven项目

- Ctrl+Shift+P

- \> Maven: Create Maven Project（创建Maven项目）
- \> maven-archetype-quickstart（选择原型）
- \> 1.4（选择版本）
- 选择项目存放的文件夹，确认后看Terminal的执行。
- 提示输入groupId：com.belean
- 提示输入artifactId：maven-demo（项目名）
- 提示输入version：（默认回车即可）
- 提示输入package：（默认回车即可）
- 最后回车确认，创建完毕后打开项目即可。

#### 代码模板

- class：生成java类
- sysout：打印到控制台的方法
- main：main方法

#### 快捷键

- `Ctrl + Shift + P`：命令
  - `>`运行命令
  - `:`跳转到行数，或`Ctrl + G`
  - `@`跳转到symbol，搜索变量或者函数，或`Ctrl + Shift + O`
  - `@:`根据分类跳转symbol，查找属性或函数，或`Ctrl + Shift + O`
  - `#`根据名字查找symbol，或`Ctrl + T`

- `Ctrl + /`：单行注释

- `Alt + Shift + A`：选中，多行注解（全注解）

- `F2`：代码重构

- `Shift + Alt + F`：代码格式化

- `> formt code`：代码格式化

- `Ctrl + F`：查找

- `Ctrl + Shift + F`：全局搜索

- `Ctrl + H`：替换
- `Shift + Alt + Up/Down`：复制一行

### IntelliJ IEDA

#### 快捷键

Alt+Enter：导包

Alt+Insert：生成代码

Ctrl+Alt+M：提取方法

Ctrl+Alt+F：提取属性

#### 创建文件时注释

`File`/`Settings`/`Editor`/`File and Code Templates`

`includes`/`File Header`

~~~java
/**
 * Created by belean on ${DATE}.
 **/
~~~

#### Services模块

> .idea/workspace.xml

新版版忽略

~~~xml
<component name="RunDashboard">
    <option name="configurationTypes">
        <set>
            <option value="SpringBootApplicationConfigurationType" />
        </set>
    </option>
</component>
~~~

> 打开视图

`View` => `tool Windows` => `services`

添加springboot

#### 插件

>IDE Eval Reset

最新版IDEA无限重置30天

添加插件仓库：https://plugins.zhile.io

>CamelCase

Shift + Alt + U：转换大小驼峰、下划线、全大写、小写。

>Alibaba Java Coding Guideline

阿里巴巴Java开发手册规约扫描，右击扫描

>Key Promoter X

鼠标操作时右下角快捷键提示

>Statistic

项目信息统计，如：总计代码行数等

>CodeGlance3

代码缩略图，`Settings` => `Other Settings`可配置宽度、位置、颜色等等。

> Background Image Plus +

背景图片，`Settings` => `Appearance & Behavior` => `Background Image Plus`配置图片目录，View视图启用，可设置自动换图

`Settings` => `Appearance & Behavior` => `Appearance` => `Background Image...`可设置图片透明度。

> Translation

翻译插件

悬浮类显示描述，右击描述选择`Translation documentation`可翻译

选中翻译：Ctrl + Shift + Y

翻译窗口：Ctrl + Shift + O





### HbuilderX



### Chrome

#### 插件

- JSONView：格式化的json视图。
- Vue.js devtools：vue开发者工具。
- GitZip for github：GitHub网页右击直接下载文件或压缩包。

- Axure RP Extension for Chrome：允许本地查看Axure RP原型（原型设计工具）

- Tampermonkey：世界上最流行的用户脚本管理器。

- ARC cookie exchange：支持高级REST客户端会话管理读取Chrome cookies。





