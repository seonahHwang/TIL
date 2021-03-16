# Exception

# Checked / Unchecked Exception

![Exception%200fab1b268a224d26a94ef91becb5b036/_2021-03-16__9.31.54.png](Exception%200fab1b268a224d26a94ef91becb5b036/_2021-03-16__9.31.54.png)

출처 : [https://itmining.tistory.com/9](https://itmining.tistory.com/9)

unchecked exception은 예외처리를 해주지 않아도 컴파일러가 문제삼지 않음

# 메서드에 예외 선언하기

throw 예외 발생시키는 키워드 

throws 메서드 선언시 사용 

throws로 계속 호출 스택의 메서드 따라가다가 제일 마지막 main메서드에서도 처리 안되면 프로그램 종료 

적어도 어느 한 곳에서는 try-catch 필요 

# 호출한 메서드에서 묵시적 예외처리

```java
public class Example {
// 함수
  public void run() {
// 콘솔 출력
    System.out.println("run!!");
  }
// 실행 함수
  public static void main(String... args) {
    try {
// 클래스 ex를 선언하고, 인스턴스 생성은 하지 않았다.
      Example ex = null;
// Example 클래스의 run 함수 호출
      ex.run();
// 콘솔 출력
      System.out.println("not error");
// NullPointerException만 잡는다.
    } catch(NullPointerException e) {
// 에러 콘솔 출력
      System.out.println("null exception");
// 모든 에러를 잡는다.
    } catch (Throwable e) {
// 에러 콘솔 출력
      System.out.println(e);
    }
// 콘솔 출력
    System.out.println("hello world");
  }
}

```

[https://t1.daumcdn.net/cfile/tistory/99514B465EBD2BBE10](https://t1.daumcdn.net/cfile/tistory/99514B465EBD2BBE10)

위 예제는 catch가 두 개가 있습니다. ex.run()에서 NullPointerException가 발생(참고 - 가장 처음 예제를 보면 결과에 NullPointerException가 발생했다고 빨간색으로 표시됩니다.)했기 때문에 catch(NullPointerException)의 영역으로 실행이 되는 것입니다.

그럼 만약 여기서 NullPointerException이 아닌 다른 에러가 발생하면, catch (Throwable)로 가게 되는 것입니다.

⇒ ex.run에서 nullpointerException 발생하면 이 메서드를 호출한 try-catch문의

nullpointerException catch문으로 감 

출처:

[https://nowonbun.tistory.com/308](https://nowonbun.tistory.com/308)

[명월 일지]