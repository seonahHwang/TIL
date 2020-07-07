# Container  
스프링 컨테이너는 프로그래머가 작성한 코드의 처리과정을 위임받아 독립적으로 처리  

## 종류  
### Bean Factory  
Bean 객체를 생성하고 관리하는 인터페이스  
구동될 때 Bean 객체를 생성하는 것이 아니라, 클라이언트의 요청( `getBean()` )이 있을 때 객체 생성  

### Application Context  
BeanFactory를 상속 받은 interface  
부가적인 기능이 더 많아서 더 많이 사용  
구동되는 시점에 등록된 Bean 객체들을 스캔하여 객체화한다  

## Configuration MetaData  
Container에 Bean의 메타정볼르 등록하기 위한 방법  
  
### xml 설정파일을 통한 등록  
* web.xml
```java
 <?xml version="1.0" encoding="UTF-8"?>
 <beans>
     <bean id="beanA" class="test.bean.BeanA"/>
     <bean id="beanB" class="test.bean.BeanB">
         <property name="beanA" ref="beanA"></property>
     </bean>
 </beans>
```

### Java Config를 이용한 등록  
* WebConfig
 ```java
 @Configuration
 public class WebConfig {
   @Bean(name = "beanA")
   public BeanA beanA {
     return new BeanA();
   }
 
   @Bean(name = "beanB")
   public BeanB beanB(BeanA beanA) {
     BeanB beanB = new BeanB();
     beanB.setBeanA(beanA);
     return beanB;
   }
 }
```
### 스프링부트는 어노테션을 통해 Bean 설정/주입  
* `Container`에 `Spring Bean`으로 등록시켜주는 Annotation  
* Ex) `@Bean`, `@Component`, `@Controller`, `@Service`, `@Repository`  
* `@Bean`은 개발자가 컨트롤 할 수 없는 외부 라이브러리를 Bean으로 등록하고 싶은 경우  
* `@Component`는 개발자가 직접 컨트롤 할 수 있는 클래스(직접만든)를 Bean으로 등록하고 싶은 경우  
(선언된 클래스를 Bean으로 등록)
* `@Controller`, `@Service`, `@Repository`등은 `@Component`를 비즈니스 레이어에 따라 명칭을 달리 지정해준 것 
* `@Autowired`  
타입으로 참조할 Bean을 찾아 주입  
 
출처 : https://sehun-kim.github.io/sehun/springbean-lifecycle/
