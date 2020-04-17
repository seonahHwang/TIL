# Java Bean VS Spring Bean

## Java Bean

: 데이터를 표현하는 것을 목적으로 하는 자바 클래스 

Java Bean 규약에 맞춰서 만든 클래스 

### <Java Bean 규약>
1. 기본생성자가 존재해야한다.
2. 모든 멤버변수의 접근제어자는 private이다.
3. 멤버변수마다 getter/setter가 존재해야한다. (속성이 boolean일 경우 is를 붙힘)
4. 외부에서 멤버변수에 접근하기 위해서는 메소드로만 접근할 수 있다.
5. Serializable(직렬화)가 가능해야한다.

Spring은 뷰 영역에 데이터를 출력하고 싶을 때 Java Bean 규약에 맞춰 만들어진 객체를 사용하고 객체들을 외부 저장소에 저장/전송 한다 

## Spring Bean

: Spring Framework의 Container에 의해 등록, 생성, 조회 되는 객체 

일반 Java Object와 동일하지만 IoC 방식으로 관리되는 오브젝트를 뜻함

Bean은 싱글톤으로 관리된다 


출처 : https://sehun-kim.github.io/sehun/springbean-lifecycle/
