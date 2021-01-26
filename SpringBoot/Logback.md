# Logback

LoggerFactory.getLogger(~.class) ⇒ root에 찍겠다는 거.!

<root>

<appender-ref ref="ERR"/>

</root>

라고 되어있으면 ERR 로그에 찍는다는 뜻

log4j를 토대로 만든 Logging 라이브러리

- logger

    실제 로깅을 수행하는 구성요소. Level 속성을 통해 출력할 로그레벨 조정 가능

- appender

    로그 메시지가 출력될 대상 결정하는 요소

- encoder

    Appender에 포함되어 사용자가 지정한 형식으로 표현될 로그메시지를 변환하는 역할

3.5 logback 로깅 레벨

TRACE라고 선언하면 모든 LEVEL 포함.

INFO로 설정하면 info, warn, error 포함

① ERROR : 일반 에러가 일어 났을 때 사용한다.

② WARN : 에러는 아니지만 주의할 필요가 있을 때 사용한다.

③ INFO : 일반 정보를 나타낼 때 사용한다.

④ DEBUG : 일반 정보를 상세히 나타낼 때 사용한다.

⑤ TRACE : 경로추적을 위해 사용한다.

출처:

[https://goddaehee.tistory.com/45](https://goddaehee.tistory.com/45)

[갓대희의 작은공간]

[(Spring Boot)Logging과 Profile 전략 : TOAST Meetup](https://meetup.toast.com/posts/149)

# Appender

Appender : 어디에 어떤 포맷으로 로그를 남길 것인지 정할 수 있는 방법을 제공
대표적으로
`ConsoleAppender`, `FileAppender`, `RollingFileAppender`

ConsoleAppender는 콘솔에 로그를 어떤 포맷으로 출력할지 설정

FileAppender는 파일에 로그를 어떤 포맷으로 출력할지 설정

RollingFileAppender는 로그 양이 많아지면 하나의 파일로 관리하기가 어려워진다. 이런 문제를 방지하기 위해 하루 단위로 로그 파일을 백업하면서 로그를 남기고자 할 때 사용할 수 있다.

## root  


root는 글로벌 로거를 의미하며, 위의 logger에 해당하지 않으면 root 로거가 실행된다.       
=> logger가 어떤 appender를 적용할지 미정이면 root에 있는 걸로 적용.  

<root level="warn">                 

<appender-ref ref="consoleAppender" />        

</root>

```
  <root level="debug">
        <appender-ref ref="CONSOLE"/>
        <appender-ref ref="FILE"/>
    </root>
```

이렇게 root 태그에 설정을 한다면, 모든 대상에 CONSOLE, FILE Appender를 적용하라는 의미이다.
