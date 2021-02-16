# Configuring a Step

# Chunk-Oriented Processing

step은 개발자가 구현하기에 따라 달렸고

step에서 필수로 설정되어야 할 부분은

reader

writer

transactionManager

respository

chunk

# The Commit Interval

chunk가 커밋 단위

chunk 10 이면,

reader로 10개 읽어온 후 writer로 전달 되고 커밋됨

# Configuring a Step for Restart

## Setting a Start Limit

특정 Step을 재시작 시킬 수 있는 제한 횟수

limit을 넘으면 StartLimitExceededException

## Restarting a Completed Step

기본적으로 job이 재시작되도 실행 결과가 Complete인 Step은 재시작되지 않는다.

항상 특정 Step을 재시작 시키고 싶다면 `allow-start-if-complete`

# Configuring Skip Logic

Step에서 발생한 특정 Exception을 skip/noSkip 설정 할 수 있다

SkipLimit도 정할 수 있음

noSkip / skip 호출 순서는 중요하지 않음

# Configuring Retry Logic

재시도해서 성공할 만한 Exception을 재시도 설정

retryLimit 설정가능

# Controlling Rollback

롤백일으키고 싶지 않은 경우에는 특정 익셉션을 `noRollback()` 설정 가능

# Transactional Readers

다시 공부..ㅠㅠ

# Transcation Attributes
