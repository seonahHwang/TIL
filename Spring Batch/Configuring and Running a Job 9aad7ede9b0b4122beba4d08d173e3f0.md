# Configuring and Running a Job

4.1 Configuring a Job

Job 인터페이스 구현체는 builder로 이를 추상화한다

Job 빌더로 병렬화(Split), Flow 제어(Decision), Flow 정의 외부화(Flow) 같은 요소 설정 가능 

4.1.1 Restartablility

restartable false 

: Job을 재실행 하지 않고 항상 새 JobInstance로 실행, 재시작 하려하면 JobRestartException 

4.1.2 Intercepting Job Execution

job이 실행 중일 때 이 job의 라이프사이클 동안 발생한 이벤트를 통지받아 원하는 코드를 실행할 수 있다 

listener() 를 통해 지정 

```java
public interface JobExecutionListener{
	void beforeJob(JobExecution jobExecution);
	void afterJob(JobExecution jobExecution); //job의 성공 여부와 상관없이 호출됨 
}
```

4.1.4(4.1.3) JobParametersValidator

job parameter 검증 

.validator() 로 세팅 

4.2 Java Config