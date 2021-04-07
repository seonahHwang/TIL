# ItemReaders and ItemWriters

# 6.1 ItemReader

ItemReader의 read()는 아이템 하나를 리턴하거나  더이상 아이템이 없는 경우 null을 리턴

ItemReader의 구현체는 앞에서 뒤로만 읽고 역행하지 말아야한다

ItemReader는 더이상 처리할 아이템이 없어도 예외를 발생시키지 않는다

# 6.2 ItemWriter

이 인터페이스는 아이템 하나가 아니라 아이템 리스트를 받는다 

# 6.3 ItemProcessor

write 전에 비즈니스 로직을 추가하고 싶을 때 사용

객체 하나를 받아 다른 객체로 변환해서 반환한다