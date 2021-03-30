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