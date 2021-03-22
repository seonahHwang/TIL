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

핵심 인터페이스 BatchConfigurer (아래의 bean을 설정하는 곳 커스터마이징 가능 )

JobRepository

JobLauncher

JobRegistry

PlatformTransactionManager

JobBuilderFactory

StepBuilderFactory

4.3 Configuring a JobRepository

JobRepository는 여러 도메인 오브젝트 CRUD할 때 필요

필수 옵션 값은

dataSource와 transactionManager

4.3.1 Transaction Configuration for the JobRepository

메소드의 디폴트 isolation 레벨은 SERIALIZABLE. 두 프로세스가 동시에 같은 job을 실행할 때 하나만 성공해야하므로. 그게 아니라면 READ_UNCOMMITTED까지 가능

4.3.2 Changing the Table Prefix

메타 테이블의 프리픽스를 JobRepository 속성을 이용해서 변경 가능

4.3.3 In-memory Repository

인메모리 Map 버전의 jobRepository 제공

인메모리 레파지토리는 휘발성이어서 JVM을 새로 띄우는 식의 재실행은 불가

같은 파라미터로 두 개의 job인스턴스 실행 불가

멀디스레드 job or 프로그램 내에서 파티셔닝한 step에 적합하지 않음
