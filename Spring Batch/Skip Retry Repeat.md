# Skip / Retry / Repeat

![Skip%20Retry%20Repeat%2000001d6538f84de6b9c3c51a90870a6c/_2020-11-23__8.31.58.png](Skip%20Retry%20Repeat%2000001d6538f84de6b9c3c51a90870a6c/_2020-11-23__8.31.58.png)

Job을 재시도하거나 건너뛰는게 아니라 tasklet 또는 step을 재시도하는것..! 

TimeoutRetryPolicy 

실패하면 시간 초과에 도달 할 때까지 재 시도

RetryPolicy를 어떻게 Job에 적용할지? 

어떤식으로 재시도가 가능한지?

`Skip Limit`

Skip의 횟수를 관리하는 Counter가 있는데 read, process, write 별로 분리되어 있으며, skip-limit에는 각 Counter의 합계가 적용된다.

### 11/29 일

FaultTolerant

```

FaultTolerantStepBuilder(SimpleStepBuilder<I,O> parent)
```

부모의 속성으로 초기화 된 새 빌더를 만듭니다.

faultTolerant() 를 통해 read-process-writ 과정중 Throwable 이 발생했을 경우 해당 Item을 Skip 하도록 설정 한다.

faultTolerant()의 return 값 TaskletStep

스탭을 새로 만들어서 재시도한다는 의미인지? 

첫 스텝은 무조건 오류가 허용(fault-tolerant)

faultTolerant()로 오류가 허용된 스텝을 얻은 후

출처:

[https://12bme.tistory.com/557](https://12bme.tistory.com/557)

RetryTemplate

문제 : 실패한 Job을 재시도 하려고 할 때, 그 Job이 아직도 실행중이어서 DB에서 status에 접근을 하려하는데 

lock이 걸릴까봐. 재시도는 status 값 기준 fail인걸 실행하려 하기때문.. 

알아야 할 것 : Job 재시도 시점 

[DB lock ](https://www.notion.so/DB-lock-a6f57d56749a43e8b10439e8ead4295c)

# 배치 Job이 재시도 되는 경우

- step에서 특정 exception 발생시 Retry 설정 가능
- (동일 파라미터로 잡을 재실행하는 걸 막아둔 경우) Job 실행 결과가 fail인 경우 같은 jobparameter로 job 실행
- preventRestart 하지 않으면 동일 JobInstance를 다시 실행함

대부분의 경우 동일 Job Parameter 실행을 막곤 하는데요.(중복 데이터가 쌓일 수가 있기 때문에)

일부 배치에서는 **동일 Job Parameter로 계속 실행이 될 수 있길** 원하기도 합니다.예를 들어 특정 데이터에 대한 검증 로직 혹은 데이터 갱신 배치 등이 이에 해당 됩니다.

동일한 job 파라미터를 사용해서 실행했을 때 실패한 인스턴스를 재시작하는데

end_time 기준으로 + 주기시간 후 시작이 됨 

재시도

`재시작`

```java
restart(long executionId)
```

Restart a failed or stopped **`[JobExecution](https://docs.spring.io/spring-batch/docs/current/api/org/springframework/batch/core/JobExecution.html)`**.

⇒ 처리 중간에 exception 내보고 다시 실행하면 어디서부터 실행하는지 확인 .. 

그리고 언제 재시작을 하는지도 확인 

테스크스케줄러 때문에 job이 모두 끝난 후 실행이되는건지,,,,,, job기준 판단이 필요  

Spring Batch의 경우 일반적으로 **동일 Job Parameter로 실행시** 어떻게 처리할 것인지에 대해 여러가지 옵션을 제공합니다.

- 해당 파라미터로 최근 실패한 이력이 있다면 이어서 실행할 것인지
- 해당 파라미터로 최근 실패 혹은 성공한 이력이 있다면 실행하지 않을 것인지
- 해당 파라미터로 최근 실행한 이력이 있어도 무시하고 다시 실행할 것인지

[Job 중복 실행 방지](https://www.notion.so/Job-1c27b9961b144eb1a0dc6f6af1c84f0c)

⇒  JobExecutionListener의 beforeJob을 구현하여 job이 돌기전에 

실행중인 jobExecution이 있으면 그걸 멈추기는 가능 

JobExplorer는 JobRepository의 read-only 버젼

출처:

[https://opennote46.tistory.com/77](https://opennote46.tistory.com/77)

[욱'S 노트]

**JobOperator**

JobRepository는 메타데이터에 대한 CRUD 기능을 제공하고, JobExplorer는 메타데이터에 대한 read-only 기능을 제공한다고 하였다. 하지만 이러한 기능들은 공통적인 모니터링 task는 즉, 중지, 재시작 또는 작업 요약등과 같은 배치 수행과 같이 사용되었을 때, 더욱 유용하다. JobOperator에 그러한 기능들을 스프링 배치에서 제공한다. 간단한 정의는 다음과 같다.

```
<bean id="jobOperator" class="org.springframework.batch.core.launch.support.SimpleJobOperator">
    <property name="jobRegistry" ref="jobRegistry"/>
    <property name="jobRepository" ref="jobRepository"/>
    <property name="jobExplorer" ref="jobExplorer"/>
    <property name="jobLauncher" ref="jobLauncher"/>
</bean>
```

출처:

[https://opennote46.tistory.com/77](https://opennote46.tistory.com/77)

[욱'S 노트]

[[Spring Quartz] Batch Job 중지 시키기 : JobOperator, Stop, StoppableTasklet](https://developyo.tistory.com/249)

ReschedulingRunnable 에서 job수행을 모두 기다린 후에 schedule()

SimpleJobLauncher 안에 taskExecutor로 job을 실행 

taskExecutor.execute(new Runnble(){

job.execute(jobExecution

}

⇒ 기존 스케줄링 job이 끝나기전까지는 다음 스케줄링이 등록되지않아서

오래 걸리더라도 겹칠일이 없다!//

메인문에서 스케줄링없이 job을 연달아 실행을 하고  첫번째 job이 실행중이라면 그 다음 job이 

JobExecutionAlreadyRunningException