### 错误：
pom.xml报错：Missing artifact jdk.tools:jdk.tools:jar:1.6


### 解决：
添加：
```
 <dependency>
        <groupId>jdk.tools</groupId>
        <artifactId>jdk.tools</artifactId>
        <version>1.6</version>
        <scope>system</scope>
        <systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>
    </dependency>
```