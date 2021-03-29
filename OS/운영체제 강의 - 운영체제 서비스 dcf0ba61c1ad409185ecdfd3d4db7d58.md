# 운영체제 강의 - 운영체제 서비스

21/03/29 월

운영체제의 주요 서비스 : 프로세스, 메모리, 파일관리, 시스템 호출 

1. 프로세스관리

Process management

프로세스 : 메모리에서 실행 중인 프로그램

주요기능 

프로세스의 생성, 소멸

프로세스 활동 일시 중지, 활동 재개

프로세스 통신

프로세스간 동기화

2. 주기억장치 관리

메인 메모리 할당/회수 

메모리의 효과적인 사용 

3. 파일 관리

주요기능

파일의 생성과 삭제

디렉터리의 생성과 삭제

기본동작지원 

: open, close, read, write, create, delete

4. 보조기억장치 관리

하드디스크, 플래시 메모리 등

주요기능

- 빈공간 관리
- 저장공간 할당
- 디스크 스케줄링

5. 입출력장치 관리

## 시스템 콜

: OS야 이런일 해줘 저런일 해줘. OS에게 하는 부탁 

운영체제 서비스를 받기 위한 호출 

커널 영역의 기능을 사용자 모드가 사용 가능하게, 즉 프로세스가 하드웨어에 직접 접근해서 필요한 기능을 사용할 수 있게 해준다 

ex) 파일 만들어줘 

주요 시스템 콜
– Process: end, abort, load, execute, create, terminate, get/set
attributes, wait event, signal event 

– Memory: allocate, free(다쓰면 되돌려줌)
– File: create, delete, open, close, read, write, get/set attributes
– Device: request, release, read, write, get/set attributes,
attach/detache devices
– Information: get/set time, get/set system data
– Communication: socket, send, receive