# 1. [@Controller](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html)별 [@ControllerAdvice](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html) 설정하기
public @interface ControllerAdvice.assignableTypes() 를 이용하여 설정한다. 
Class<?>[] 이기 때문에 여러 개의 Controller에 동시에 적용할 수도 있다.
```java
/**
 * Array of classes.
 * <p>Controllers that are assignable to at least one of the given types
 * will be assisted by the {@code @ControllerAdvice} annotated class.
 * @since 4.0
 */
Class<?>[] assignableTypes() default {};
```

예) GlobalExceptionHandler는 전체적으로 적용되는 Handler이고, AExcpetionHandler와 BExceptionHandler는 특정 클래스를 위한 Handler이다.

```java
GlobalExceptionHandler.java
--------------------------
package xxx.handler;

import org.springframework.core.annotation.Order;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice
@Order(Ordered.LOWEST_PRECEDENCE)
public class FirstExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}

==========================

AExceptionHandler.java
--------------------------
package xxx.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice(assignableTypes = { AController.class} )
public class AExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}

==========================

BExceptionHandler.java
--------------------------

package xxx.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice(assignableTypes = { BController.class } )
public class BExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}
```

# 2. 패키지별 [@ControllerAdvice](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ControllerAdvice.html) 설정하기
public @interface ControllerAdvice.basePackageClasses() 를 이용하여 설정한다. Class<?>[] 이기 때문에 여러 개의 패키지를 동시에 적용할 수도 있다.
```java
/**
 * Type-safe alternative to {@link #value()} for specifying the packages
 * to select Controllers to be assisted by the {@code @ControllerAdvice}
 * annotated class.
 * <p>Consider creating a special no-op marker class or interface in each package
 * that serves no purpose other than being referenced by this attribute.
 * @since 4.0
 */
Class<?>[] basePackageClasses() default {};

```

예) GlobalExceptionHandler는 전체적으로 적용되는 Handler이고, SecurityExceptionHandler는 xxx.security 패키지에 포함된 @Controller들을 처리하고, PerformanceExceptionHandler는 xxx.performance 패키지에 있는 @Controller들을 처리한다.

```java
GlobalExceptionHandler.java
--------------------------
package xxx.handler;

import org.springframework.core.annotation.Order;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice
@Order(Ordered.LOWEST_PRECEDENCE)
public class FirstExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}

==========================

# xxx.security.XXXSecController 
SecurityExceptionHandler.java
--------------------------
package xxx.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice(assignableTypes = { XXXSecController.class} )
public class SecurityExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}

==========================

# xxx.performance.XXXPerfController 
PerformanceExceptionHandler.java
--------------------------
package xxx.handler;

import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;

@ControllerAdvice(assignableTypes = { XXXPerfController.class } )
public class PerformanceExceptionHandler extends ResponseEntityExceptionHandler {
    ...
}
```

