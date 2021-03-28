# Configuring and Running a Job

# 4.1 Configuring a Job

Job 인터페이스 구현체는 builder로 이를 추상화한다

Job 빌더로 병렬화(Split), Flow 제어(Decision), Flow 정의 외부화(Flow) 같은 요소 설정 가능 

## 4.1.1 Restartablility

restartable false 

: Job을 재실행 하지 않고 항상 새 JobInstance로 실행, 재시작 하려하면 JobRestartException 

## 4.1.2 Intercepting Job Execution

job이 실행 중일 때 이 job의 라이프사이클 동안 발생한 이벤트를 통지받아 원하는 코드를 실행할 수 있다 

listener() 를 통해 지정 

```java
public interface JobExecutionListener{
	void beforeJob(JobExecution jobExecution);
	void afterJob(JobExecution jobExecution); //job의 성공 여부와 상관없이 호출됨 
}
```

## 4.1.4(4.1.3) JobParametersValidator

job parameter 검증 

.validator() 로 세팅 

# 4.2 Java Config

핵심 인터페이스 BatchConfigurer (아래의 bean을 설정하는 곳 커스터마이징 가능 )

JobRepository

JobLauncher

JobRegistry

PlatformTransactionManager

JobBuilderFactory

StepBuilderFactory 

# 4.3 Configuring a JobRepository

JobRepository는 여러 도메인 오브젝트 CRUD할 때 필요

필수 옵션 값은 

dataSource와 transactionManager

## 4.3.1 Transaction Configuration for the JobRepository

메소드의 디폴트 isolation 레벨은 SERIALIZABLE. 두 프로세스가 동시에 같은 job을 실행할 때 하나만 성공해야하므로. 그게 아니라면 READ_UNCOMMITTED까지 가능

## 4.3.2 Changing the Table Prefix

메타 테이블의 프리픽스를 JobRepository 속성을 이용해서 변경 가능 

## 4.3.3 In-memory Repository

인메모리 Map 버전의 jobRepository 제공

인메모리 레파지토리는 휘발성이어서 JVM을 새로 띄우는 식의 재실행은 불가

같은 파라미터로 두 개의 job인스턴스 실행 불가

멀디스레드 job or 프로그램 내에서 파티셔닝한 step에 적합하지 않음 

# 4.4 Configuring a JobLauncher

SimpleJobLauncher : JobLauncher 인터페이스의 가장 기본적인 구현체

 HTTP 요청으로 job을 실행할 때, jobLauncher의 taskExecutor를 SimpleAsyncTaskExecutor로 지정함으로써 job을 비동기로 처리할 수 있다

# 4.5 Running a Job

job을 실행하기 위해서는 Job과 JobLauncher 2가지가 필요 

두가지 모두 같은 컨텍스트 or 다른 컨텍스트 사용 가능 

커맨트라인에서 job을 실행시킬 땐 각 job마다 JVM 따로 초기화, 각자 다른 JobLauncher에서 job 실행 

HttpRequest로 job 실행할땐 여러 요청을 비동기로 실행하기 위해 JobLauncher 하나만 사용 

## 4.5.1 Running Jobs From the Command Line

The CommandLineJobRunner 라는 인터페이스를 두어 command에서 실행가능하게함 

`CommandLineJobRunner` 의 작업 4가지

- ApplicationContext 로딩
- 커맨드라인에서 받은 인자를 JobParameters로 변환
- 인자에 따른 적절한 job 선택
- 애플리케이션 컨텍스트에 있는 jobLauncher로 job 실행

커멘드라인 필수 인자 

`jobPath` ApplicationContext를 생성할 때 필요한 xml 파일 위치. 이 파일은 job 실행에 필요한모든것을 포함한다

`jobName` 실행할 job 이름

```bash
java CommandLineJobRunner io.spring.EndOfDayJobConfiguration endOfDay schedule.date(date)=2007/07/07
```

java CommandLineJobRunner job설정클래스풀네임 job이름 job파라미터 

ExitCodes

스케줄러를 사용하면 리턴코드로 잡의 성공/실패 여부 판단

스프링 배치 같은 프레임워크를 다룰 땐 특정 job의 exitcode를 숫자로 리턴할 방법이 필요 

스프링 배치에서 ExitStatus라는게 있고, 

이걸 CommandLineJobRunner가 ExitCodeMapper를 사용해 String → Number로 바꿔준다

## 4.5.2 Running Jobs from within a Web Container

httpRequest로 실행할 땐 job을 비동기로 실행해야한다

컨트롤러가 JobLauncher를 이용해 Job을 실행시킴

Job이 실행 중인데도 논블로킹 처리로 컨트롤러는 즉시 응답 가능 

# 4.6 Advanced Meta-Data Usage

![Configuring%20and%20Running%20a%20Job%2060057ff2376b487c8337b12103520174/IMG_05571643777D-1.jpeg](Configuring%20and%20Running%20a%20Job%2060057ff2376b487c8337b12103520174/IMG_05571643777D-1.jpeg)

## 4.6.1 Querying the Repository

execution요청은 JobExplorer가 지원 

JobExplorer은 JobRepository의 리드온리 버전

JobExplorer도 마찬가지로 테이블 프리픽스 설정 가능 

## 4.6.2 JobRegistry

컨텍스트 내에 있는 job을 추적하고 싶을 때 유용 

등록된 job의 이름이나 프로퍼티스 관리할 때 유용 

### JobRegistryBeanPostProcessor

### AutomaticJobRegistrar

자식 컨텍스트를 생성하고 각 Job을 하위 컨텍스트에 등록하는 라이프사이클 컴포넌트

이렇게 하면 자식 컨텍스트에 있는 Job이름은 registry전역에서 유일해야하지만,

job이 의존성을 갖는 bean 이름은 그럴 필요 없음 

⇒ 여러 job에itemReader가 여러개 있어도 상관없다

모든 파일이 같은 컨텍스트에 임포트된다면 reader는 충돌해서 서로를 덮어쓰게 되지만 이 자동 registrar가 이를 방지 

registrar 필수 프로퍼티 

- ApplicationFactory 배열 : 자식 컨텍스트 생성 담당
- JobLoader : 자식 컨텍스트와 JobRegistry에 등록된 job의 라이프사이클관리

## 4.6.3 JobOperator

중단, 재시작, job 요약등의 모니터링

## 4.6.4 JobParametersIncrementer

Job을 구분하기 위해 [run.id](http://run.id) + 1

## 4.6.5 Stopping a Job

job을 graceful 종료할때 JobOperator 사용 

바로종료시키지 않음 

강제 즉시 종료 불가능 

## 4.6.6 Aborting a Job

ABANDONED 상태인 job은 프레임워크에 의해 재실행되지 않는다

프로세스가 강제로 죽게되면 jobRepository는 프로세스가 죽은 사실을 알 수 없기 때문에

execution의 상태를 수동으로 알려줘야하는데( FAILED or ABANDONED) 

재시작할 수 없는 job이거나 데이터가 유효하다는 걸 확신할 수 있을 때만 status를 FAILED로 처리