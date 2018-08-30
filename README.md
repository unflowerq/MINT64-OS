# MINT64-OS

MINT64 OS 개발 시작

QEMU 등의 에뮬레이터를 사용하여 업로드된 img파일을 실행시켜 확인가능

========================2018/07/31========================

부트로더 제작

16비트 리얼모드에서 32비트 보호 모드로 전환

A20 게이트를 활성화하여 1MB 이상 영역에 접근


========================2018/08/01========================

IA-32e 모드 커널을 위해 64GB까지 매핑하는 테이블 생성

생성된 페이지 테이블을 프로세서에 설정하여 페이지 기능을 활성화

프로세서의 제조사 확인 및 IA-32e 지원 여부 검사

IA-32e 모드용 세그먼트 디스크립터 추가

페이징 활성화


========================2018/08/06========================

IA-32e 모드로 전환(64비트 모드로)

키보드 디바이스 드라이버 생성(키보드 입력을 화면에 출력 가능)


========================2018/08/09=======================

OS에서 반드시 처리해야 하는 두 가지 이벤트 인터럽트, 예외 추가

위의 두 가지를 처리하기 위한 IST(Interrupt Stack Table) 추가

TSS(Task Status Segment) 추가

TSS세그먼트를 참조할 수 있게 GDT 테이블에 TSS디스크립터 추가

IDT테이블 추가

PIC컨트롤러를 추가하여 19가지의 예외와 16개의 인터럽트 처리

이를 위한 각각의 핸들러 생성

최종적으로 화면 오른쪽 위에는 타이머에 대한 인터럽트의 횟수 증가를 표현

키보드 입력에 따른 인터럽트의 횟수 증가를 왼쪽 위에 표현


========================2018/08/10=======================

이전의 폴링 방식으로 구현했던 키보드 디바이스 드라이버를 인터럽트 방식으로 변경

변경을 위해 버퍼로 사용할 범용 큐 구현

멀티태스킹 환경에서도 키보드 이벤트를 처리할 수 있게 됨


========================2018/08/13=======================

셸을 구현하는데 필요한 콘솔 라이브러리 구현

콘솔 라이브러리를 구성하는 printf()와 sprintf() 함수 구현

콘솔 라이브러리를 바탕으로 셸을 구현

help 명령어를 통해 각 커맨드의 기능에 대한 설명들을 확인할 수 있음


========================2018/08/16=======================

PIT컨트롤러, 타임 스탬프 카운터, RTC컨트롤러와 같이 시간에 관계된 디바이스 드라이버 추가

settimer, date 등 시간에 관련된 커맨드 5개 추가


========================2018/08/20=======================

TCB 자료구조를 생성하고 초기화

스케줄러 구현을 위해 범용 리스트 구현

태스크를 관리하기 위한 태스크 풀과 스택 풀 생성

여러 태스크를 관리하기 위해 라운드 로빈 스케줄러 구현

PIT컨트롤러(타이머)가 발생시키는 IRQ 0 인터럽트를 활용하여 발생하는 신호를 바탕으로 시분할 멀티태스킹 구현

2개의 예제 태스크 구현 - help 명령어를 통해 사용법 확인 가능


========================2018/08/22=======================

기존의 라운드 로빈 스케줄러 업그레이드

각 태스크에 우선순위를 할당하고 우선순위에 따라 태스크를 실행하는 빈도를 조절하는 멀티레벨 큐 스케줄러 구현

태스크를 종료시키고 프로세서의 부하를 줄이는 유휴태스크 추가


========================2018/08/23=======================

태스크와 인터럽트 간의 동기화 문제를 해결하기 위해 인터럽트 제어

인터럽트 플래그를 제어하는 잠금과 해제 함수를 따로 구현

태스크와 태스크 간의 동기화 문제를 해결하기 위해 뮤텍스 구현

뮤텍스를 활용해 태스크들이 변수를 공유하는지 확인하기 위해 태스크 3개를 생성하고 각각 변수를 1씩 증가시키는 mutextest 커맨드 추가


========================2018/08/24=======================

기존의 태스크를 바탕으로 프로세스와 스레드 구현

태스크 자료구조의 플래그 처리를 통해 프로세스와 스레드를 구분

프로세스를 killtask 명령어를 통해 종료시키면 자식 스레드들도 모두 종료

testthread 커맨드를 추가하여 하나의 프로세스와 해당 프로세스의 자식 스레드 3개를 생성

showmatrix 커맨드로 스레드를 활용한 화면을 출력하도록함

기존의 구현한 멀티태스킹 기능을 바탕으로 실수 연산 장치(FPU) 구현

testpie 커맨드를 추가하여 실수연산 태스크 100개를 생성하도록 구현


========================2018/08/30========================

효율적인 메모리 관리를 위해 동적할당 및 해제 기능 구현

메모리 단편화 문제는 버디 블록 알고리즘으로 해결

동적 메모리 정보를 표시하는 dynamicmeminfo 커맨드 추가

testseqalloc커맨드는 블록 리스트별로 할당할 수 있는 최대 블록 수만큼 모두 할당하고 해제하는 명령

testranalloc커맨드는 태스크 1000개를 생성하여 1KB~32MB 범위의 메모리 블록을 할당받고 데이터를 쓰고 읽는 방식으로 검증한 뒤 다시 해제

20MB 크기의 하드 디스크 이미지를 생성

하드 디스크 드라이버 추가

hddinfo 커맨드를 통해 하드 디스크의 정보를 출력

readsector 커맨드를 통해 하드 디스크의 데이터를 읽어들임

writesector 커맨드를 통해 하드 디스크에 데이터를 쓸 수 있음

