# 필터

# 필터

HTTP 요청과 응답을 변경할 수 있는 재사용 가능한 코드

객체의 형태로 존재하며 클라이언트로부터 오는 요청(request)와 최종자원(서블릿/JSP/기타문서) 사이에 위치하며 클라이언트의 요청정보를 알맞게 변경가능하다

또한, 최종자원과 클라이언트로 가는 응답사이에 위치하여 최종 자원의 요청 결과를 변경할 수 있다

![Untitled/filter_20190313.jpg](Untitled/filter_20190313.jpg)

필터는 클라이원트와 자원사이에 1개 존재하는 경우가 보통이지만, 여러개의 필터가 모여 하나의 체인을 형성할 수 있다

여러개의 필터를 사용하는 경우

첫번째 필터의 대상 - 클라이언트의 요청정보

두번째 필터의 대상 - 1번째 필터로 변경된 정보

..

이런식으로 요청정보를 거듭 변경한다

응답의 경우 위의 과정과 반대의 과정이다

## 필터 관련 인터페이스 & 클래스

javax.servlet.Filter 인터페이스

⇒ 클라이언트와 최종 자원 사이에 위치하는 필터를 나타낼 때, 이 인터페이스를 상속받아 구현해야한다

javax.servlet.ServletRequestWrapper 클래스

javax.servlet.ServletResponseWrapper 클래스

⇒ 필터가 요청/응답을 변경한 결과를 저장할 래퍼 클래스

## 필터 인터페이스의 주요 메서드

```java
void init(FilterConfig filterConfig) throw ServletException
```

필터를 웹 컨테이너에 생성한 후 초기화할때 호출

```java
void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
throws java.io.IOException, ServletException
```

체인을 따라 다음에 존재하는 필터로 이동한다

체인의 가장 마지막에는 클라이언트가 요청한 최종자원이 위치

```java
void destroy()
```

필터가 웹 컨테이너에서 삭제될때 호출

주로 필터가 사용한 자원을 반납

## 필터의 설정

필터를 사용하기 위해 특정 필터가 특정 자원에 적용된다는 것을 서블릿/JSP 컨테이너에 알려줘야한다

web.xml 파일에서 설정

예시

```xml
<web-app>

     <filter>
        <filter-name>HighlightFilter</filter-name>
        <filter-class>javacan.filter.HighlightFilter</filter-class>
        <init-param>
           <param-name>paramName</param-name>
           <param-value>value</param-value>
        </init-param>
     </filter>

     <filter-mapping>
        <filter-name>HighlightFilter</filter-name>
        <url-pattern>*.txt</url-pattern>
     </filter-mapping>

  </web-app>
```

- init() 메소드가 호출 될 때 전달되는 파라미터 값

    주로 필터를 사용하기 전에 초기화해야하는 객체나 자원을 할당할 때 필요한 정보를 제공하기 위해 사용

- 클라이언트가 요청한 URI에 대해서 필터링할 때 사용
    - '/' 로 시작하고 '/*' 끝나는 건 경로 매핑 시 사용됨
    - '*.'로 시작하는 url-pattern은 확장자에 대한 매핑 시 사용됨

[https://twofootdog.github.io/Spring-필터(Filter)란-무엇인가/](https://twofootdog.github.io/Spring-%ED%95%84%ED%84%B0(Filter)%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80/)

# 인터셉터

![Untitled/2155894358C6091209.png](Untitled/2155894358C6091209.png)

요청을 가로챈다

DispatcherServlet이 컨트롤러를 호출하기 전, 후로 끼어든다

스프링 내의 모든 객체 접근가능
