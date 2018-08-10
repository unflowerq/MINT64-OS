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

키보드 입력에 따른 인터럽트의 횟수 증가를 왼쪽 위에 
