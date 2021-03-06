spring-boot-admin-server
================================

## Easy Setup
Add the following dependency to your pom.xml.

```xml
<dependency>
	<groupId>de.codecentric</groupId>
	<artifactId>spring-boot-admin-server</artifactId>
	<version>1.1.2</version>
</dependency>
<dependency>
	<groupId>de.codecentric</groupId>
	<artifactId>spring-boot-admin-server-ui</artifactId>
	<version>1.1.2</version>
</dependency>
```

Create the Spring Boot Admin Server with only one single Annotation.
Example in spring-admin-samples/spring-boot-admin-sample.
```java
@Configuration
@EnableAutoConfiguration
@EnableAdminServer
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

## Hazelcast Support

Spring Boot Admin Server supports cluster replication with Hazelcast.
It is automatically enabled when its found on the classpath.

Just add Hazelcast to your dependencies:
```xml
<dependency>
	<groupId>com.hazelcast</groupId>
	<artifactId>hazelcast</artifactId>
	<version>3.3.3</version>
</dependency>
```

And thats it! The server is going to use the default Hazelcast configuration.

### Custom Hazelcast configuration
To change the configuration add a com.hazelcast.config.Config bean to your application context (for example with hazelcast-spring):

Add hazelcast-spring to dependencies:
```xml
<dependency>
	<groupId>com.hazelcast</groupId>
	<artifactId>hazelcast-spring</artifactId>
	<version>3.3.3</version>
</dependency>
```

Import hazelcast spring configuration xml-file:
```java
@Configuration
@EnableAutoConfiguration
@EnableAdminServer
@ImportResource({ "classpath:hazelcast-config.xml" })
public class Application {
	public static void main(String[] args) {
		SpringApplication.run(Application.class, args);
	}
}
```

Write xml-config hazelcast-config.xml:
```xml
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:hz="http://www.hazelcast.com/schema/spring" 
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd http://www.hazelcast.com/schema/spring http://www.hazelcast.com/schema/spring/hazelcast-spring-3.3.xsd">
	<hz:config id="hazelcastConfig">
		<hz:instance-name>${hz.instance.name}</hz:instance-name>
		<hz:map name="spring-boot-admin-application-store" backup-count="1" eviction-policy="NONE" />
	</hz:config>
</beans>
```

### Further configuration
To disable Hazelcast support by setting ``spring.boot.admin.hazelcast.enable=false``.

To alter the name of the Hazelcast-Map set ``spring.boot.admin.hazelcast.map= my-own-map-name``.
