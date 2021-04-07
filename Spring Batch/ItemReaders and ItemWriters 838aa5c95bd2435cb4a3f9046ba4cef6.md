# ItemReaders and ItemWriters

# 6.1 ItemReader

- ItemReader의 read()는 아이템 하나를 리턴하거나  더이상 아이템이 없는 경우 null을 리턴
- ItemReader의 구현체는 앞에서 뒤로만 읽고 역행하지 말아야한다
- ItemReader는 더이상 처리할 아이템이 없어도 예외를 발생시키지 않는다
- 커스텀 가능
- 대표적인 구현체인 JdbcPagingItemReader를 보면

    ItemReader외에 ItemStream도 같이 구현하고 있음 

# 6.2 ItemWriter

이 인터페이스는 아이템 하나가 아니라 아이템 리스트를 받는다 

영속성 관련해서 write()안에서 flush함 

커스텀 Writer

```java
@Bean
    public ItemWriter<Pay2> customItemWriter() {
        return new ItemWriter<Pay2>() {
            @Override
            public void write(List<? extends Pay2> items) throws Exception {
                for (Pay2 item : items) {
                    System.out.println(item);
                }
            }
        };
    }
```

# 6.3 ItemProcessor

write 전에 비즈니스 로직을 추가하고 싶을 때 사용

객체 하나를 받아 다른 객체로 변환해서 반환한다 

## 6.3.1 Chaining ItemProcessors

여러 ItemProcessor 구현체를 연결하고 싶을 때 composite 패턴을 사용하면 된다

## 6.3.2 Filtering Records

writer에 데이터를 넘길 때 필터링 하고 싶으면 ItemProcessor에서 null을 리턴하면 된다

## 6.3.3 Fault Tolerance

청크가 롤백되면 데이터를 읽을 때 이미 캐시해둔 아이템이 다시 처리 될 수도 있다.

보통 skip이나 retry가 설정된 ItemProcessor는 멱등성을 보장해야한다

보통은 ItemProcessor의 입력 데이터는 바꾸지 않고 결과로 사용할 인스턴스만 바꾸는 식으로 구현 

# 6.4 ItemStream

- 주기적으로 상태를 저장하고 오류가 발생하면 해당 상태에서 복원하기위한 마커 인터페이스

즉, 배치 프로세스의 실행 컨텍스트(Execution Context)와 연계해서 ItemReader의 상태를 저장하고

실패한 곳에서 다시 실행 할 수 있게 하는 역할 

- ItemStream의 update()로 Execution Context에 상태 저장
- open(), close(), update() 3개의 메서드로 구성되어있음
- open(), close() 스트림 열고 닫기

# 6.5 The Delegate Pattern and Registering with the Step

step에서 위임 패턴을 사용한다면 거의 모든 경우 수동으로 Step에 등록해야한다