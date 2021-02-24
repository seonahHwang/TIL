# pom.xml

[메이븐(Maven)은 알고 스프링(Spring)을 쓰는가? (pom.xml 분석하며 가볍게 정리하는 빌드 툴, Maven)](https://jeong-pro.tistory.com/168)

[Maven 명령어](https://www.notion.so/fe770f08d42c45119ee45651bef639cd)

- 의존성 분석 순서

    **로컬 저장소에서 의존성 확인원격 저장소 리스트에서 의존성 확인1과 2가 실패하면 의존성 에러 보고**

- 의존성 필수 선언 항목

    **<groupId>: 부분적인 프로젝트나 조직에서의 라이브러리 집합을 식별하기 위해 제공한다.**

    **<artifactId>: 프로젝트의 실제 이름으로 groupId와 합쳐져 프로젝트 식별에 쓰인다.**

    **<version>: 선언한 의존성 artifact의 버전으로 프로젝트 내에서 사용하는artifcat의 일관성을 추구할 수 있다.**

- 의존성 SCOPE
    - compile: 기본값으로 모든 클래스 패스에서 사용 가능하다.
    - provided: 컴파일과 유사하나 패키지에는 포함되지 않는다. 컨테이너나 JDK에서 제공한다. 예) Servlet API for web apps
    - runtime: 컴파일러가 아닌 런타임 시에 사용된다. 예) JDBC dirvers
    - test: 테스트 단계에서만 사용된다. 예)Junit
    - system: provided와 유사하나 개발자가 직접 JAR 파일을 제공해야 하고 저장소에서 지정한 dependency를 찾지 않는다.