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
## Skip

데이터를 처리하는 동안 설정된 Exception이 발생하면 데이터 처리를 건너 뛰는 기능. 빈번한 실패 줄일 수 있음

skip-limit : skip할 수 있는 최대 횟수 지정

include / exclude 익셉션 설정 가능  

## Retry

Processing, Writing 하는 동안 설정된 Exception이 발생했을 경우, 지정한 정책에 따라 데이터 처리를 재시도하는 기능

Reader에서 사용 불가

retry-limit

include / exclude 지정

테스트job 코드 확인해보기

Read과정까지 성공한 데이터는 캐쉬에 저장된다.

재시도가 일어날 경우 캐쉬의 데이터를 가져와 Process 과정부터 다시 수행

retryable exception은 기본적으로 rollback을 유발하므로 너무 많은 Retry는 성능을 저하시킬 수 있으므로 주의해야 한다

### RetryTemplate

`RetryOperations` 작업에 대한 재시도를 자동화하기 위한 전략 인터페이스

`RretryTemplate` RetryOperation의 구현체

`RetryPolicies` RetryTemplate에서 재시도 할지 여부는 RetryPolicy에 의해 결정된다.

Repeat
# JobOperation / JobRepository / JobLauncher

# Chunk 지향 처리

Chunk 지향 처리 : 한 번에 하나씩의 데이터를 읽어 Chunk라는 덩어리를 만든 뒤, Chunk 단위로 트랜잭션처리를 하는 것

# ItemReader / ItemWriter / ItemProcessor

참고

[https://renuevo.github.io/spring/batch/spring-batch-chapter-1/](https://renuevo.github.io/spring/batch/spring-batch-chapter-1/)
