스프링프레임워크는 다른 많은 기능들이 있습니다. 그 기능들은 약 스무개의 모듈로 나위어져 있으며 흔히 발생하는 문제들을 해결하고 있습니다. 

아래는 몇가지 대중적인 모듈들입니다.

* Spring JDBC

* Spring MVC

* Spring AOP

* Spring ORM

* Spring JMS

* Spring Test

* Spring Expression Language( SpEL )

 

관점 지향 프로그래밍( AOP, Aspect Oriented Programming)은 스프링 프레임워크에서 아주 강력한 기능입니다. 객체지향 프로그래밍에서 중요 키포인트는 Class입니다. 반면 AOP에서는 관점(Aspect)입니다. 예를 들어, 기존 프로젝트에 security나 logging 등을 추가하고 싶을 때.. 기존 비즈니스 로직에는 손을 대지 않고 AOP를 활용하여 추가할 수 있습니다. 특정 메소드가 끝날 때 호출할 수도 있고, 호출 직전, 메소드가 리턴한 직후 혹은 예외가 발생했을 때 등 여러가지 상황에 활용할 수 있습니다.

스프링은 스프링만의 ORM을 가지고 있지 않습니다. 그러나 Hibernate, Apache Ibatis 등과 같은 ORM과의 매우 우수한 통합환경을 제공합니다.


 
간단히 말해, 스프링 프레임워크는 웹 어플리케이션을 개발하는데 결합도를 낮추는 방향의 개발방법을 제공한다고 말할 수 있습니다. Web 어플리케이션 개발은 스프링의 이러한 컨셉( Dispatcher Servlet, ModelAndView, and Vier Resolver ) 덕분에 쉬운 개발을 할 수 있게 됬습니다.

 

스프링이 이토록 많은 문제들을 해결하고 있다면, 왜 스프링 부트의 필요해졌을까??
현재 당신이 스프링으로 이미 개발하고 있다면, 모든 기능을 다 갖춘 스프링 어플리케이션을 개발하면서 직면한 문제들에 대해서 되새겨 생각해보세요. 만일 생각을 못하는 분들을 위해 몇가지 꼬집어보겠습니다. Transaction Manager, Hibernate Datasource, Entity Manager, Session Factory와 같은 설정을 하는데에 어려움이 많이 있었습니다. 최소한의 기능으로 Spring MVC를 사용하여 기본 프로젝트를 셋팅하는데 개발자에게 너무 많은 시간이 걸렸습니다.

 

<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix">
        <value>/WEB-INF/views/</value>
    </property>
    <property name="suffix">
        <value>.jsp</value>
    </property>
</bean>

<mvc:resources mapping="/webjars/**" location="/webjars/"/>

<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
    </servlet-class>
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>/WEB-INF/my-servlet.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
 

Hibernate를 사용할 때는 datasource, EntityManager 등과 같은 것을 설정해야 합니다. 이는 아래와 같습니다.

<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
    destroy-method="close">
    <property name="driverClass" value="${db.driver}" />
    <property name="jdbcUrl" value="${db.url}" />
    <property name="user" value="${db.username}" />
    <property name="password" value="${db.password}" />
</bean>

<jdbc:initialize-database data-source="dataSource">
    <jdbc:script location="classpath:config/schema.sql" />
    <jdbc:script location="classpath:config/data.sql" />
</jdbc:initialize-database>

<bean
    class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
    id="entityManagerFactory">
    <property name="persistenceUnitName" value="hsql_pu" />
    <property name="dataSource" ref="dataSource" />
</bean>

<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
    <property name="entityManagerFactory" ref="entityManagerFactory" />
    <property name="dataSource" ref="dataSource" />
</bean>

<tx:annotation-driven transaction-manager="transactionManager"/>
 

 

스프링부트는 스프링의 이러한 설정에 관한 이슈를 어떻게 풀었을까??
1.

스프링부트는 자동설정(AutoConfiguration)을 이용하였고 어플리케이션 개발에 필요한 모든 내부 디펜던시를 관리합니다. 개발자가 해야하는건 단지 어플리케이션을 실행할 뿐입니다. 스프링의 jar파일이 클래스 패스에 있는 경우 Spring Boot는 Dispatcher Servlet으로 자동 구성합니다. 마찬가지로 만약 Hibernate의 jar파일이 클래스 패스내에 존재한다면 이를 datasource로 자동설정하게 됩니다. 스프링부트는 미리설정된 스타터 프로젝트를 제공합니다.

 

2.

웹어플리케이션을 개발하는 동안, 우리는 사용하려는 jar, 사용할 jar 버전, 함께 연결하는 방법이 필요합니다. 모든 웹 어플리케이션에는 Spring MVC, Jackson Databind, Hibernate 코어 및 Log4j와 같은 유사한 요구사항들이 있습니다. 그래서 우리는 이러한 jar들의 서로 호환되는 버전들을 따로 선택을 해주어야 했습니다. 이런 복잡도를 줄이기 위해서 스프링 부트는 SpringBoot Starter라고 불리는 것을 도입했습니다. 

 

스프링 프로젝트의 의존성 관리
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>4.2.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.5.3</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.0.2.Final</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
 

 

Starter는 편리하게 스프링 부트 어플리케이션에 포함할 수 있는 편리한 dependency 세트입니다. Spring과 Hibernate를 사용하려면 프로젝트에 spring-boot-starter-data-jpa 라는 의존성을 포함시키면 간단하게 사용할 수 있습니다.

 

스프링 부트 프로젝트의 의존성 관리
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
아래의 스크린샷은 하나의 dependency에 포함된 여러가지 패키지들을 나타냅니다.

 


스프링 부트 패키지 jar 
 

당신이 한번 스타터 Dependency를 추가하면 스프링부트 스타터 웹프로젝트가 pre-packaged된 형태로 제공됩니다. 그로인해 개발자는 Dependency 관리와 호환버전에 대하여 고려할 필요가 없습니다.

 

스프링 부트 스타터 프로젝트 옵션들
응용프로그램의 종류에 따라 쉽고 빠르게 개발을 도와줄 수 있는 몇가지 스프링 스타터 프로젝트를 소개하겠습니다

* spring-boot-starter-web-services : SOAP 웹 서비스

* spring-boot-starter-web : Web, RESTful 응용프로그램

* spring-boot-starter-test : Unit testing, Integration Testing

* spring-boot-starter-jdbc : 기본적인 JDBC

* spring-boot-starter-hateoas : HATEOAS 기능을 서비스에 추가

* spring-boot-starter-security : 스프링 시큐리티를 이용한 인증과 권한


 
* spring-boot-starter-data-jpa : Spring Data JPA with Hibernate

* spring-boot-starter-cache : 스프링 프레임워크에 캐싱 지원 가능

* spring-boot-starter-data-rest : Spring Data REST를 사용하여 간단한 REST 서비스 노출





Reference
https://sas-study.tistory.com/274

 
