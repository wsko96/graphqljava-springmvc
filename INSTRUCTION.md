# graphqljava-springmvc

## Lab 2

- 새로운 Spring Framework Web MVC application 프로젝트 생성. 예)

```
  <groupId>com.example</groupId>
  <artifactId>graphqljava-springmvc</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>war</packaging>
  <name>graphqljava-springmvc</name>
  <description>GraphQL Java Demo project for Spring Web MVC</description>

  <properties>

    <spring.version>5.3.9</spring.version>
    <jackson2.version>2.12.3</jackson2.version>

    <slf4j.version>1.7.31</slf4j.version>
    <log4j2.version>2.14.1</log4j2.version>

    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>

    <jetty.version>9.4.43.v20210629</jetty.version>
    <jetty.port>8080</jetty.port>

  </properties>
```

- 아래와 같은 Dependency들과 플러그인 설정 추가

```
  <dependencies>
  
    <dependency>
      <groupId>com.graphql-java</groupId>
      <artifactId>graphql-java</artifactId>
      <version>17.0</version>
    </dependency>
    <dependency>
      <groupId>com.graphql-java</groupId>
      <artifactId>graphql-java-spring-webmvc</artifactId>
      <version>2.0</version>
    </dependency>
    <dependency>
      <groupId>com.google.guava</groupId>
      <artifactId>guava</artifactId>
      <version>30.1.1-jre</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>${jackson2.version}</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>${jackson2.version}</version>
    </dependency>

    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-web</artifactId>
      <version>${log4j2.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-core</artifactId>
      <version>${log4j2.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-slf4j-impl</artifactId>
      <version>${log4j2.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-jcl</artifactId>
      <version>${log4j2.version}</version>
    </dependency>

    <dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-1.2-api</artifactId>
      <version>${log4j2.version}</version>
    </dependency>

  </dependencies>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>3.0.0-M5</version>
      </plugin>
      <plugin>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-maven-plugin</artifactId>
        <version>${jetty.version}</version>
        <configuration>
          <port>${jetty.port}</port>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

- src/main/webapp/WEB-INF/web.xml 추가

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd"
         version="3.0">

  <display-name>graphqljava-springmvc</display-name>
  <description>graphqljava-springmvc</description>

  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <servlet>
    <servlet-name>api-service</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <async-supported>true</async-supported>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>api-service</servlet-name>
    <url-pattern>/api/*</url-pattern>
  </servlet-mapping>

</web-app>
```

- https://github.com/wsko96/graphqljava-demo 프로젝트에 있는,
  src/main/java/**/*.java 파일들과 src/main/resources/schema.graphqls 파일을
  같은 상대 경로들에 복사.

- src/main/webapp/WEB-INF/api-service-servlet.xml 추가

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <mvc:annotation-driven />

    <context:component-scan base-package="graphql.spring.web.servlet.components,
                                          com.example.graphqljavademo.bookdetails" />

</beans>
```

- src/main/webapp/WEB-INF/applicationContext.xml 추가

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="objectMapper" class="com.fasterxml.jackson.databind.ObjectMapper" />

</beans>
```

- 빌드 및 실행

```
mvn clean jetty:run
```

- GraphQL Playground를 실행하고, URL endpoint에 http://localhost:8080/api/graphql을 입력하고,
  GraphQL 질의 테스트
