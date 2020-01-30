## Maven
Maven是一个基于项目对象模型（POM）的概念的纯Java开发的开源的项目管理工具，主要用来管理Java项目，进行依赖管理（jar包管理，能自动分析项目所需要的依赖软件包，并到Maven仓库区下载）和项目构建（项目编译、打包、测试、部署）此外还能分块开发，提高开发效率。

### 1. Maven 下载、安装和配置
#### 下载
https://maven.apache.org/download.cgi
!["maven_download"]()

#### 安装
解压放在路径下：D:\software\maven\apache-maven-3.6.3

>-bin：含有mvn的运行的脚本
>-boot：含有plexus-classwords类加载器的框架
>-conf：含有settings.xml配置文件
>-lib：含有Maven运行时需要的java类库

####  环境变量
maven以来java环境（Maven 3.3+ 需要jdk7+）

maven本身有两个环境变量要配置：
>MAVEN_HOME：maven的安装目录
>PATH：maven安装目录下的bin目录

#### Maven配置
在maven的conf目录下有一个*settings.xml*文件
添加*<localRepository>*配置maven“本地仓库“的位置(默认是 ${user.home}/.m2/repository的位置，可以修改本地库的位置到D:\software\maven\reposirity_hh)

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
  <localRepository>/path/to/local/repo</localRepository>
  -->
  <localRepository>D:\software\maven\reposirity_hh</localRepository>
```

在*<profiles>*标签里添加一个*<profile>*标签，限定maven项目的默认的jdk版本。内容如下：
```xml
  <profiles>
	<profile>  
    　　<id>jdk-1.8</id>    
    　　<activation>   
        　　<activeByDefault>true</activeByDefault>    
        　　<jdk>1.8</jdk>   
   　　 </activation>    
   　　 <properties>   
       　　 <maven.compiler.source>1.8</maven.compiler.source>    
       　　 <maven.compiler.target>1.8</maven.compiler.target>    
       　　 <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>   
   　　 </properties>   
	<profile>
  </profiles>
```

### 2. 仓库
仓库分类
!["maven_repository_classfication"]()

本地仓库：位于自己计算机中的仓库
远程仓库：需要联网才可以使用的仓库，私服一般在内网中便可以使用，但是中央仓库则需要外网的支持。