+++
title = "Mac 上安装 Java 多版本"
date = 2020-06-13T23:12:55+08:00
categories = ["编程"]
tags = ["java"]
toc = false
+++

<!--more-->

Java 现在是一年一个版本（每六个月就能获得新版本），加快迭代的速度，每一个版本将延续前一个版本的新特性，但只有 LTS（Long-Term-Support）版本得到长期维护，见下表：

![Oracle SE](/images/java/Oracle_Java_SE_Support_Roadmap.png)

考虑到 Java 庞大的生态和生产环境，估计 Java8 还会呆很长一段时间，所以机器上得上多版本：

1. 加入源 [adoptopenjdk/openjdk](https://github.com/AdoptOpenJDK/homebrew-openjdk)

```bash
[I] ➜ brew tap adoptopenjdk/openjdk
```

2. 看看有什么版本

```bash
[I] ➜ brew search jdk
==> Formulae
openjdk ✔                                                    openjdk@11
==> Casks
adoptopenjdk                             adoptopenjdk12-openj9-large              adoptopenjdk8 ✔
adoptopenjdk10                           adoptopenjdk13                           adoptopenjdk8 ✔
adoptopenjdk11                           adoptopenjdk13-jre                       adoptopenjdk8-jre
adoptopenjdk11-jre                       adoptopenjdk13-openj9                    adoptopenjdk8-openj9
adoptopenjdk11-openj9                    adoptopenjdk13-openj9-jre                adoptopenjdk8-openj9-jre
adoptopenjdk11-openj9-jre                adoptopenjdk13-openj9-jre-large          adoptopenjdk8-openj9-jre-large
adoptopenjdk11-openj9-jre-large          adoptopenjdk13-openj9-large              adoptopenjdk8-openj9-large
adoptopenjdk11-openj9-large              adoptopenjdk14                           adoptopenjdk9
adoptopenjdk12                           adoptopenjdk14-jre                       jdk-mission-control
adoptopenjdk12-jre                       adoptopenjdk14-openj9                    oracle-jdk
adoptopenjdk12-openj9                    adoptopenjdk14-openj9-jre                oracle-jdk-javadoc
adoptopenjdk12-openj9-jre                adoptopenjdk14-openj9-jre-large          sapmachine-jdk
adoptopenjdk12-openj9-jre-large          adoptopenjdk14-openj9-large
```

3. 选择版本安装，例如：

```bash
[I] ➜ brew cask install adoptopenjdk8
[I] ➜ brew cask install adoptopenjdk14
```

4. Mac 的 JDK 标准安装目录在“/Library/Java/JavaVirtualMachines”，也是 brew 采纳的安装目录，看看安装后本机上的所有版本：

```bash
[I] ➜ /usr/libexec/java_home -V
Matching Java Virtual Machines (2):
    14.0.1, x86_64:	"AdoptOpenJDK 14"	/Library/Java/JavaVirtualMachines/adoptopenjdk-14.jdk/Contents/Home
    1.8.0_252, x86_64:	"AdoptOpenJDK 8"	/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
```

5. 切换不同的版本

把这段加入 .bash 或 .zshrd

```bash
export JAVA_HOME=`/usr/libexec/java_home -v 1.8`

jdk() {
    export JAVA_HOME=`/usr/libexec/java_home -v $@`
    echo "JAVA_HOME switched to '$JAVA_HOME'."
}
```

默认的就是 JDK8，通过 jdk xxx 来切换到想要的版本：

```bash
[I] ➜ java -version
openjdk version "1.8.0_252"
OpenJDK Runtime Environment (AdoptOpenJDK)(build 1.8.0_252-b09)
OpenJDK 64-Bit Server VM (AdoptOpenJDK)(build 25.252-b09, mixed mode)                                               /0.1s

~
[I] ➜ jdk 14
JAVA_HOME switched to '/Library/Java/JavaVirtualMachines/adoptopenjdk-14.jdk/Contents/Home'.                        /0.0s

~
[I] ➜ java -version
openjdk version "14.0.1" 2020-04-14
OpenJDK Runtime Environment AdoptOpenJDK (build 14.0.1+7)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 14.0.1+7, mixed mode, sharing)
```

6. 如果项目采用 Maven，指明 JDK 版本：

```xml
<properties>
 <java.version>14</java.version>
</properties>

<plugin>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <release>14</release>
        <compilerArgs>
            <arg>--enable-preview</arg>
        </compilerArgs>
        <forceJavacCompilerUse>true</forceJavacCompilerUse>
        <parameters>true</parameters>
    </configuration>
</plugin>

<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <argLine>--enable-preview</argLine>
    </configuration>
</plugin>
```

加入"enable-preview" 才能开启 JDK14 中“非”默认的新特性。
