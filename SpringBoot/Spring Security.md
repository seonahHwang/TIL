# Spring Security  
스프링 기반의 어플리케이션의 보안(인증과 권한)을  담당하는 프레임워크  

만약 시큐리티를 사용하지 않았다면 자체적으로 세션 체크와, redirect를 해야했을 것  
Spring Security는 filter 기반으로 동작하기 때문에 spring MVC와 분리되어 관리 및 동작  

## 보안용어
- 접근 주체 : 보호된 대상에 접근하는 유저  
- 인증 : 현재 유저가 누구인지 확인 ex) 로그인   
  애플리케이션의 작업을 수행할 수 있는 주체임을 증명  

- 권한 : 인증된 주체가 애플리케이션의 동작을 실행 할 수 있는 권한이 있는지 확인
  - 권한 승인이 필요한 부분으로 접근하려면 인증 과정을 통해 주체가 증명되어야만 한다
  - 권한 부여에도 두가지 영역이 존재
  1. 웹 요청 권한
  2. 메소드 호출 및 도메인 인스턴스에 대한 접근 권한부여  


## @EnableWebSecurity  

## HttpSecurity
## Method Security  
서비스계층의 메소드에 보안을 추가할 수 있도록 지원   
메소드에 ```@Secured``` 어노테이션을 추가하면 해당 메소드에 대한 액세스가 제한  
역할 단위로 제약조건 지정 가능   

## AntPathRequestMatcher  
시큐리티에서 원하는 url로 보내기 위한 객체

## UserDetails
security에서 사용자의 정보를 담는 인터페이스

이를 통해서 Security 인증, 인가에서 직접 구현한 인터페이스 기준으로 작업할 수 있다 .  
표준으로 User라는 클래스가 준비되어 있음   
User 클래스을 변경해야 하는 경우는 UserDetails 인터페이스를 구현한 클래스를 만들면 된다.

## UserDetailsService
DB에서 유저 정보 가져오는 역할   

참고    
https://sjh836.tistory.com/165  
https://victorydntmd.tistory.com/328  
[https://velog.io/@hellas4/2019-11-12-0811-작성됨](https://velog.io/@hellas4/2019-11-12-0811-%EC%9E%91%EC%84%B1%EB%90%A8)
