# 스프링 배치 TIL

# 스프링 배치 기본구조

계층형 구조 Job > Step > Task 

### Job

JobInstance 

JobExecution

### Tasklet 종류 2가지

- ChunkedOrientedTasklet

    read / process / write (RPW) 형식

    트랜잭션 단위 : Chunk

- Tasklet

    트랜잭션 단위 : Step

# 스프링 배치 메타 데이터

스프링 배치는 DB로 상태관리

Job history

실패 / 성공 여부

실행 재개 지점

JobParameter 

# 스프링 배치 Flow

`ExitStatus` 보다 `JobExecutionDecider` 인터페이스로 처리

# JobParameter

JobInstance를 식별하는 유일한 값 

# Scope

@JobScope

@StepScope

# Skip Retry Restart

# JobOperation / JobRepository / JobLauncher

# Chunk 지향 처리

Chunk 지향 처리 : 한 번에 하나씩의 데이터를 읽어 Chunk라는 덩어리를 만든 뒤, Chunk 단위로 트랜잭션처리를 하는 것 

# ItemReader / ItemWriter / ItemProcessor

참고 

[https://renuevo.github.io/spring/batch/spring-batch-chapter-1/](https://renuevo.github.io/spring/batch/spring-batch-chapter-1/)