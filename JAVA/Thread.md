# Thread

## syncronized  
#### 동기화 메소드  
여러 쓰레드 들이 공유객체의 메소드를 사용할 때 메소드에 Synchronized가 붙어 있으면 먼저 호출한 메소드가 객체의 사용권(Monitoring Lock)을 얻는다   

다른 쓰레드들은 모니터링 락을 놓을때까지 대기한다.  

synchronized를 붙히지 않은 메소드는 다른 쓰레드들이 synchronized메소드를 실행하면서 모니터링 락을 획득했다 하더라도, 그것과 상관없이 실행된다.

#### 동기화 블럭  
synchronized를 메소드에 붙혀서 사용 할 경우, 메소드의 코드가 길어지면, 마지막에 대기하는 쓰레드가 너무 오래 기다리는것을 막기위해서 메소드에 synchronized를 붙이지 않고, 문제가 있을것 같은 부분만 synchronized블록을 사용한다.  

출처 : https://programmers.co.kr/learn/courses/9/lessons/274  


## 쓰레드 실행와 상태제어  
## wait() notify()  
