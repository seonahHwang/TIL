# Configuring a Step 한글

# 5.1 Chunk-oriented Processing

reader로 읽어온 item수가 commit interval 수와 같아 지면 itemWriter가 청크 전체를 write하고 커밋된다

## 5.1.1 Configuring a Step

builder를 이용해서 설정하는 것들 

- reader : 처리할 아이템을 제공하는 ItemReader
- writer
- transactionManager : 디폴트는 transactionmanager
- repository : StepExecution 과 ExecutionContext를 주기적으로 저장하는 JobRepository
- chunk

## 5.1.3 The Commit Interval

보통은 한 트랜잭션에서 가능한 많은 아이템 처리하나 step마다 다름 

데이터를 처리하기 전에 트랜잭션 시작 

## 5.1.4 Configuring a Step for Restart

### Setting a Start Limit

정해진 횟수만 Step 실행. 그 이상 실행하면 StartLimitExccededException

start-limit의 디폴트 값은 Integer.MAX_VALUE

### Restarting a Completed Step

COMPLETED 상태를 가진 step은 스킵하도록 되어있음 

그래도 실행하고 싶다면 allowStartIfComplete(true)

## 5.1.5 Configuring Skip Logic

특정 익셉션을 특정 횟수만큼 skip

skipLimit이 10번이면 10번은 그냥 넘어가고 11번째에 같은 익셉션이 터지면 step 실패처리 

또한 특정익셉션이 아닌 다른 예외가 발생하면 job 실패 

## 5.1.6 Configuring Retry Logic

여러번 시도하면 될 수 도 있는 경우에

특정 익셉션 발생시에 몇번까지 재시도 할지 설정 가능

## 5.1.7 Controlling Rollback

재시도 여부에 상관없이 ItemWriter에서 발생하는 모든 예외는 Step에서 처리되는 트랜잭션을 롤백시킨다

skip설정이 되어있다면 ItemReader에서 발생한 예외는 롤백을 발생시키지 않는다 

Step에 롤백 유발하지 않는 exception리스트 지정 가능 

### Transactional Readers

ItemReader는 데이터를 읽을 때 앞→뒤 로 읽고, 역행 X

step은 데이터를 읽고나면 버퍼에 넣어둬서 롤백되었을때, 다시 읽어올 필요 X

그러나 어떤 경우에는 step이 버퍼를 사용하지 않도록 지정할 수도 있다 

## 5.1.8 Transactional Attributes

트랜잭션 고립수준, 전파, 타임아웃 설정 가능 

DefaultTransactionAttribute attribute = new DefaultTransactionAttribute

## 5.1.9 Registering ItemStream with a Step

itemStream 공부하고 다시 읽어보기 

## 5.1.10 Intercepting step Execution

### StepExecutionListener

step 시작 전/후의 리스너 

@BeforeStep

@AfterStep

### ChunkListener

청크 처리 전 후 

@BeforeChunk : 트랜잭션이 시작 된후 호출. ItemReader의 read메서드를 호출하기 전 

@AfterChunk

@AfterChunkError

### ItemReadListener

@BeforeRead : read메서드 호출 전 매번 호출 

@AfterRead : read메서드 호출에 성공할때마다 호출. 읽은 아이템을 인자로 받음

@OnReadError : 읽는도중에러발생 

### ItemProcessorListener

### ItemWriterListener

### SkipListener

@OnSkipInRead 아이템을 읽는동안 아이템이 스킵될때마다 

@OnSkipInWrite

@OnSkipInProcess

### SkipListeners and Transactions

스킵된 이슈를 확인하고 수정하려면 다른 배치 프로세스나 수동 작업이 필요할 때가 있다.

기존 트랜잭션이 롤백되었다면 여러 이유가 있을 수 있으므로 스프링 배치는 다음 2가지를 보장 

1. 적절한 skip 메서드를 아이템 마다 한번만 호출한다
2. SkipListener는 항상 트랜잭션이 커밋되기 직전에 호출

    따라서 writer에서 오류가 발생해도 리스너에서 호출하는 트랜잭션까지 롤백되지 않는다 

# 5.2 TaskletStep

Tasklet : execute 메소드 하나를 가진 인터페이스 

RepeatStatus.FINISHED가 리턴되거나 실패했단 뜻으로 exception이 던져지기 전까지 

TaskletStep 이 반복적으로 execute 호출

- 각 Tasklet 호출이 트랜잭션으로 감싸져 있다.
- tasklet이 stepListener 인터페이스를 구현했다면 TaskletStep 이 자동으로 tasklet을 stepListener로 등록

## 5.2.1 TaskletAdapter

# 5.3 Controlling Step Flow

step이 실패했다고 반드시 job도 실패로 끝나야하는것이 아니다

step성공이 모두 같은 성공은 아니다

## 5.3.1 Sequential Flow

next attribute 사용 

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
				.start(stepA())
				.next(stepB())
				.next(stepC())
				.build();
}
```

stepA가 실패하면 전체 job이 실패로 끝나 stepB 는 실행 X

## 5.3.2 Conditional Flow

한개의 step이 next attribute와 transition element를 둘 다 가질 수는 없다.

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
				.start(stepA())
				.on("*").to(stepB())
				.from(stepA()).on("FAILED").to(stepC())
				.end()
				.build();
}
```

on은 step의 ExitStatus와 매칭

Step은 transition element 수에 제한이 없긴 하지만, Step의 결과로 받은 ExitStatus가 어떤 element와도 매칭되지 않으면 프레임워크 단에서 예외을 발생시키고 Job은 실패한다.

### Batch Status Versus Exit Status

`BatchStatus`는 JobExecution과 StepExecution의 프로퍼티로, 

단순 열거형(enumeration)이며 Job과 Step의 상태를 나타낸다

COMPLETED, STARTING, STARTED, STOPPING, STOPPED, FAILED, ABANDONED, UNKNOWN

on()메서드는 BatchStatus가 아닌 Step의 ExitStatus를 참조

## 5.3.3. Configuring for Stop

Step의 상태는 실행되는 코드로 결정

Job의 상태는 설정에 따라 달라짐

Step에 정의된 transition이 없다면 아래 규칙으로 Job 상태 정의

- Step이 끝나고 ExitStatus FAILED를 리턴했다면, Job의 BatchStatus와 ExitStatus도 FAILED이다
- 그 외에는 Job의 ExitStatus, BatchStatus모두 COMPLETED

### Job을 종료시키기 위한 transition element 3가지

### Ending at s Step

step이 끝나기만 하면 Job의 BatchStatus를 COMPLETED로 둔 채 종료 가능

`end()` end메서드에 Job의 ExitStatus를 커스텀할 수 있는 ExitStatus 파라미터를 넘길 수도 있다

디폴트는 COMPLETED

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
				.start(step1())
				.next(step2())
				.on("FAILED").end() // step2 fail이면 job 종료. step3은 실행되지 않음 
														//job BatchStatus가 COMPLETED인 상태로 종료이므로 
														//재시작해도 다시실행되지않음
				.from(step2()).on("*").to(step3())
				.end()
				.build();
}

```

### Failing a Step

아래 예제에서는 `step2`가 실패하면 `Job`의 `BatchStatus`는 `FAILED`, `ExitStatus`는 `EARLY TERMINATION`으로 두고 종료되며, `step3`를 실행하지 않는다. 

반대로 `step2`가 성공하면 다음은 `step3`를 실행한다. 

`step2`가 실패해서 `Job`을 재시작하면 `step2`부터 다시 실행한다.

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
			.start(step1())
			.next(step2()).on("FAILED").fail()
			.from(step2()).on("*").to(step3())
			.end()
			.build();
}
```

### Stopping a Job at a Given Step

특정 step에서 작업을 중단하도록 설정하면 BatchStatus가 STOPPED인 상태로 Job종료

Job을 중단하면 중간에 텀이 생겨 Job을 재시작하기 전 액션을 실행할 수 있다

아래 예제에서는 `step1`이 `COMPLETE`로 끝나면 job을 중단한다. 재시작되면 `step2`를 시작한다.

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
			.start(step1()).on("COMPLETED").stopAndRestart(step2())
			.end()
			.build();
}
```

## 5.3.4 Programmatic Flow Decisions

다음 step을 결정하기 위한 조건

JobExecutionDecider

```java
public class MyDecider implements JobExecutionDecider {
    public FlowExecutionStatus decide(JobExecution jobExecution, StepExecution stepExecution) {
        String status;
        if (someCondition()) {
            status = "FAILED";
        }
        else {
            status = "COMPLETED";
        }
        return new FlowExecutionStatus(status);
    }
}
아래 예제에선 자바 기반 설정을 사용해 JobExecutionDecider를 구현한 빈을 next 호출부에 직접 넘긴다.

@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
			.start(step1())
			.next(decider()).on("FAILED").to(step2())
			.from(decider()).on("COMPLETED").to(step3())
			.end()
			.build();
}
```

## 5.3.5 Split Flows

병렬 플로우 

‘split’ element는 ‘next’ attribute, ‘next’, ‘end’, ‘fail’ element같이 앞에 나온 transition element라면 어떤 것이든 포함할 수 있다.

```java
@Bean
public Job job() {
	Flow flow1 = new FlowBuilder<SimpleFlow>("flow1")
			.start(step1())
			.next(step2())
			.build();
	Flow flow2 = new FlowBuilder<SimpleFlow>("flow2")
			.start(step3())
			.build();

	return this.jobBuilderFactory.get("job")
				.start(flow1)
				.split(new SimpleAsyncTaskExecutor())
				.add(flow2)
				.next(step4())
				.end()
				.build();
}
```

## 5.3.6 Externalizing Flow Definitions and Dependencies Between Jobs

flow는 빈으로 따로 정의해서 재사용도 가능 

```java
@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
				.start(flow1())
				.next(step3())
				.end()
				.build();
}

@Bean
public Flow flow1() {
	return new FlowBuilder<SimpleFlow>("flow1")
			.start(step1())
			.next(step2())
			.build();
}
```

외부 flow를 사용하는 또하나의 방법은  Jobstep 활용

```java

@Bean
public Job jobStepJob() {
	return this.jobBuilderFactory.get("jobStepJob")
				.start(jobStepJobStep1(null))
				.build();
}

@Bean
public Step jobStepJobStep1(JobLauncher jobLauncher) { //step안에서 job실행 
	return this.stepBuilderFactory.get("jobStepJobStep1")
				.job(job())
				.launcher(jobLauncher)
				.parametersExtractor(jobParametersExtractor()) 
//jobParametersExtractor는 Step의 ExecutionContext를 Job이 실행되는 데 
//필요한 JobParameters로 변환하는 방법을 정의한다
				.build();
}

@Bean
public Job job() {
	return this.jobBuilderFactory.get("job")
				.start(step1())
				.build();
}

@Bean
public DefaultJobParametersExtractor jobParametersExtractor() {
	DefaultJobParametersExtractor extractor = new DefaultJobParametersExtractor();

	extractor.setKeys(new String[]{"input.file"});

	return extractor;
}
```

# 5.4 Late Binding of Job and Step Attributes

(job을 실행하는 시점마다 bean으로 등록되서

프로퍼티에 있는 값을 그 때마다 읽어오는게 되는걸까?? 

→ 프로퍼티에 있는 값이 아니라, 보통 program Argument로 jobParameter 전달 )

JobScope, StepScope 역시 Job이 실행되고 끝날때, Step이 실행되고 끝날때 생성/삭제가 이루어진다고 보시면 됩니다.

@JobScope는 Step 선언문에서 사용 가능하고, @StepScope는 Tasklet이나 ItemReader, ItemWriter, ItemProcessor에서 사용할 수 있습니다.

```java
@StepScope
@Bean
public FlatFileItemReader flatFileItemReader(@Value("#{jobParameters['input.file.name']}") String name) {
	return new FlatFileItemReaderBuilder<Foo>()
			.name("flatFileItemReader")
			.resource(new FileSystemResource(name))
			...
}
//JobExecution, StepExecution 레벨의 ExecutionContext 모두 같은 방식으로 사용할 수 있다:

//jobExecutionContext에 있는 input.file.name 가져오기 
@StepScope
@Bean
public FlatFileItemReader flatFileItemReader(@Value("#{jobExecutionContext['input.file.name']}") String name) {
	return new FlatFileItemReaderBuilder<Foo>()
			.name("flatFileItemReader")
			.resource(new FileSystemResource(name))
			...
}

//stepExecutionContext에 있는 input.file.name 가져오기 
@StepScope
@Bean
public FlatFileItemReader flatFileItemReader(@Value("#{stepExecutionContext['input.file.name']}") String name) {
	return new FlatFileItemReaderBuilder<Foo>()
			.name("flatFileItemReader")
			.resource(new FileSystemResource(name))
			...
}
```

### JobParameter 내용 보충

JobParameter를 사용하기 위해서는 꼭 @StepScope, @JobScope로 Bean을 생성해야한다

JobParmeter는 Step, Tasklet, Reader 등 Batch 컴포넌트 Bean의 생성 시점에 호출 할 수 있음 

@StepScope, @JobScope Bean을 생성할때만 JobParameters가 생성

### JobParameter VS 시스템 변수

1. 시스템변수를 사용하면 Spring Batch의 Job Parameter관련 기능을 못쓰게됨

parameter관련 메타테이블이 전혀 관리되지 않음

2. Command Line이 아닌 다른 방법으로 job 실행하기가 어려움

다른 방법으로 실행하려면 시스템 변수 또는 환경변수를 동적으로 계속 변경시킬수 있어야함 

예를 들어, Jobparameter를 이용한다면 

외부에서 (컨트롤러 등) parameter값을 받아와 그걸로 job 수행 가능

즉, 개발자가 원하는 어느 타이밍이든 Job Paramter를 생성하고 수행할 수 있음