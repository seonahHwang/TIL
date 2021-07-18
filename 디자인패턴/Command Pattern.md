# Command Pattern

정의

실행될 기능을 캡슐화함으로써 여러기능을 실행할 수 있는 

재사용성이 높은 클래스를 설계하는 패턴

이벤트가 발생했을 때 실행될 기능이 다양하면서도

변경이 필요한 경우에 이벤트를 발생시키는 클래스를 변경하지 않고 재사용하고자 할 때 유용 

역할이 수행하는 작업

- Command (interface)

    실행될 기능에 대한 인터페이스

    실행될 기능을 execute 메서드로 선언함

- ConcreteCommand (class)

    실제로 실행되는 기능을 구현

- Invoker

    기능의 실행을 요청하는 호출자 클래스

- Receiver

    ConcreteCommnad의 기능을 실행하기 위해 사용하는 수신자 클래스 

장단점
