# jetty-alpn-agent

`jetty-alpn-agent` is a JVM agent that enables Jetty ALPN (or NPN) support for Java 7 and 8 by loading the correct `alpn-boot` (or `npn-boot`) JAR file for the current Java version. For more information, please refer to the following resources:

- Jetty documentation: [ALPN](http://www.eclipse.org/jetty/documentation/9.3.0.v20150612/alpn-chapter.html) and [NPN](http://www.eclipse.org/jetty/documentation/9.2.10.v20150310/npn-chapter.html)
- Java API documentation: [`java.lang.instrument`](https://docs.oracle.com/javase/8/docs/api/java/lang/instrument/package-summary.html)

## Usage

Specify the path to the agent JAR file with a `-javaagent` option:

```bash
java -javaagent:<path/to/jetty-alpn-agent.jar> ...
```

To use NPN instead of ALPN, specify the `forceNpn=true` option:

```bash
java -javaagent:<path/to/jetty-alpn-agent.jar=forceNpn=true ...
```

## Using with Maven

Use `maven-dependency-plugin` to fetch the agent JAR and add the path to the downloaded agent JAR to the command line arguments:

```xml
<project>
  <properties>
    <alpn.agent.version>1.0.0.Final</alpn.agent.version>
    <alpn.agent.path>${settings.localRepository}/kr/motd/javaagent/jetty-alpn-agent/${alpn.agent.version}/jetty-alpn-agent-${alpn.agent.version}.jar</alpn.agent.path>
  </properties>
  
  <build>
    <plugins>
      <plugin>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>2.10</version>
        <executions>
          <execution>
            <id>get-jetty-alpn-agent</id>
            <phase>validate</phase>
            <goals>
              <goal>get</goal>
            </goals>
            <configuration>
              <groupId>kr.motd.javaagent</groupId>
              <artifactId>jetty-alpn-agent</artifactId>
              <version>${alpn.agent.version}</version>
            </configuration>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.19.1</version>
        <configuration>
          <argLine>-javaagent:${alpn.agent.path}</argLine>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

## Legal stuff

This product is licensed under [Apache License 2.0](http://www.apache.org/licenses/LICENSE-2.0).

This product redistributes the original `alpn-boot` and `npn-boot` JARs, which are licensed under [GPLv2 with classpath exception](http://openjdk.java.net/legal/gplv2+ce.html), whose source code is located at:

- https://github.com/jetty-project/jetty-alpn/
- https://github.com/jetty-project/jetty-npn/
