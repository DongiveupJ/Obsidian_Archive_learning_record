# 1장
오퍼레이팅 시스템의 정의
	딱 하나로 정의된 것은 없음
	1. 하드웨어를 컨트롤 하는 프로그램
	2. 프로그램의 실행을 컨트롤 하는 프로그램(스케쥴링 등)

OS의 일반적 역할
	하드웨어 매니지먼트
		I/O 디바이스 엑세스
		파일 엑세스
		어카운팅
		에러 디텍션
	프로그램 실행
		스케쥴링
		에러 보고

OS는 컴퓨터 하드웨어와 어플리케이션 프로그램 사이에 있음

OS의 목표
	1. 유저 프로그램을 실행하고, 유저의 문제를 해결하기 쉽게 함
	2. 컴퓨터 시스템을 사용하기 쉽게 함
	3. 컴퓨터 하드웨어를 효율적으로 사용

OS는~
	1. 자원(하드웨어) 관리
	2. 프로그램 컨트롤 (순서 관리)
	커널(kernel)이라고도 함 - 커널: OS중에서도 핵심, OS와 같은 의미로도 쓰임

컴퓨터 시스템의 구조
![[Pasted image 20250319140445.png]]
	위를 자원(하드웨어) 라고 함
	메모리는 cpu가 직접 사용함
	각 하드웨어끼리는 bus로 정보를 주고받음

4개의 기본적 원리
	1. 컴퓨터 시스템 I/O 오퍼레이션
	2. I/O 구조
	3. 인터럽트
	4. 저장공간 구조
추가적인 특징
	멀티프로세서 시스템

컴퓨터 시스템 I/O 오퍼레이션
	모든 기기는 컨트롤러(보드)가 있음
	I/O: 데이터가 메모리와 로컬 버퍼로 오고감
	DMA (Direct Memory Access)를 통해 I/O 기기와 CPU가 동시에 실행 가능
	디바이스 컨트롤러가 인터럽트를 발생시켜 CPU에 작업이 완료되었음을 알림

I/O 구조
	I/O 트랜잭션은 버스를 통해 수행됨
		버스는 주소, 데이터 및 제어 신호를 전달하는 병렬 와이어의 모음
	DMA: CPU의 간섭이 없는 I/O transaction(read, write) - CPU는 다른 일 할 수 있음

==인터럽트== 
	-I/O가 끝났을 때 비동기적으로 발생
	-unconditionally, immediately (무조건, 즉각적)
	-외부 디바이스가 CPU에게 인터럽트 전달
	-OS는 CPU가 인터럽트를 받으면 무조건 즉시 각 ISR을 실행 함 (ISR: 인터럽트 서비스 루틴, 커널 안에 있음 - 인터럽트를 받을 때 수행해야 할 일)
		각 소스마다 별도의 ISR을 가지고 있고, 인터럽트 안에 저장되어 있음
	-모든 디바이스의 드라이버는 ISR을 가져야 함
	H/W 인터럽트, S/W 인터럽트(trap)가 있음
	-인터럽트가 발생됐을 때, ISR이 끝나고 돌아갈 원래 주소를 저장해야 함

![[Pasted image 20250319142120.png]]

인터럽트와 트랩의 차이점
	인터럽트 - 외부 디바이스가 유발, 비동기적
		키보드의 ctl-c 누르기
	트랩 - 연산의 수행으로 발생 됨(소프트웨어 인터럽트) 동기적
		시스템 콜, segmentation fault
		(각각 고유의 ISR 넘버를 가짐)


스토리지 구조
	메인 메모리(DRAM) - 휘발성
	세컨더리 스토리지 - 비휘발성
	![[Pasted image 20250319142945.png]]

==Caching==(중요!!) - 저장 관리 기법
	빠른 스토리지(캐시)에 자주 접근하는 파일 먼저 넣기
	자주 접근하는 파일은 SSD, 드문 파일은 HDD에
	'' SRAM, " DRAM

컴퓨터 시스템 구조
	대부분의 시스템은 하나의 프로세스 사용
	멀티프로세서 시스템이 중요해지는중
		CPU가 여러개로 concurrently - 부담스러우니 칩 하나에 코어를 여러개 가지는 멀티 코어로 많이 배포(병렬)
		장점
			1. throughput(1초동안 하는 일의 양) 향상
			2. 하나의 메모리를 공유 가능
			3. 신뢰성 향상
			DVFS: 다이나믹 볼태지 프리퀀시 스케일링 - 전력, 효율 조정

두개의 타입의 멀티프로세서 시스템
	비대칭적 멀티프로세싱 - master, slave 관계
		마스터 프로세서가 전체 메모리 맵에 접근 가능
		슬레이브 프로세서는 각자 고유의 메모리를 가지고있음
	대칭적 멀티프로세싱 - shared memory, private memory
		프로세서들이 동등한 관계
		각자 프로세서들이 메모리 공유

멀티프로세서 메모리 모델
	Uniform memory access(UMA) 메모리에 접근하는 시간이 균등
	Non-uniform memory access(NUMA) 메모리에 접근하는 시간이 다름 - 프로그램의 저장 위치에 따라 성능 차이가 남
![[Pasted image 20250319234356.png]]


**==멀티프로그래밍==** = 멀티태스킹 - OS를 배우는 이유
프로세스 = task = job : 실행중인 프로그램
	목적: CPU와 I/O기기들을 항상 최대로 활용

![[Pasted image 20250319234753.png]]

![[Pasted image 20250319234808.png]]

현재 수행중인 프로세스들이 버츄얼 메모리에 모두 올라감 -> 프로세스들의 크기가 실제 메모리보다 더 큼
*job scheduling* - 어떤 프로세스를 메모리에 올릴지 결정
*Swapping* - 필요한 프로그램을 올리고, 불필요한 프로그램을 뺌
*paging* - 프로그램을 페이지 단위로 쪼개 사용하는 일 (page = 4kb)
*CPU scheduling* - 어떤 프로세스를 CPU로 수행할지 결정

운영체제 오퍼레이션
	현대 OS에서는 인터럽트가 자주 발생함
	현대 OS는 듀얼모드 오퍼레이션 지원
		1. 커널 모드
		2. 유저 모드   -   어떤것이 수행중이냐에 따라 모드 바뀜
		모드는 비트로 표현 / 커널-0 유저-1
		모드 비트가 0일떄만, 특권 명령어 수행 가능
		
		모드를 구분하는 이유 : 허가받지 않은 유저로부터 OS를 보호함
		Privileged instruction 특권 명령어 -> 커널 모드에서만 실행이 허가됨
			오직 커널코드가 컴파일 될 때 나오는 명렁어
			-특정 명령어나 프로그램이 하드웨어에 접근하는 것을 막음 -> 유저 프로그램의 특권 명령어를 막음



# 2장
운영체제의 역할
	-유저
		1.유저 인터페이스
			CLI(커맨드 인터페이스 라인) - MS DOS
			Batch interface
			GUI
		2.프로그램 실행
		3.I/O 오퍼레이션 - 하드웨어 접근 제어
		4.파일시스템 관리(파일 관리 - 탐색기)
		5.커뮤니케이션 - 각 프로세스의 통신
		6.에러 방지
	-효율
		1.자원 관리 - 하드웨어, 멀티태스킹 관리
		2.어카운팅 - 하드웨어를 어떻게 쓰는지 통계자료를 제공해줌 - 작업관리자, 장치관리자
		3.보호
		4.보안

유저 인터페이스
	CLI(커맨드 인터페이스 라인) - MS DOS
		== shell 프로그램
		2가지 메소드
		1. 쉘 안에 명령어 프로그램을 내포 -> 명령어가 너무 많아서 힘들다
		2. 실행파일 디렉토리에서 찾아서 명렁어 수행 -> 이방법 주로 사용
	Batch interface - Make file (make 명령어)
	GUI - 윈도우, 유닉스, 리눅스

시스템 콜
	커널 모드에 진입
	1. 하드웨어 인터럽트
	2. 트랩(소프트웨어 인터럽트)
	소프트웨어 이벤트를 커널에 전달하는 메커니즘:
		1. 시스템 콜
		2. 예외 - 0으로 나누기 등
	시스템 콜: 어플리케이션 프로그램이 OS에 호출 요청하는 유일한 메커니즘
		일반적으로 중개자로서의 라이브러리임(API 통함)
		문제점: 직접 호출이 아닌 간접 호출임
		간접 호출하는 이유: 운영체제마다 라이브러리 함수가 다르지만, 같은 시스템 콜을 사용 가능함
	API
		Win32 API - CreateProcess() 를 사용함으로 NTCreateProcess() 시스템 콜을 간접호출
		POSIX API: malloc(): 라이브러리 함수 -> sbrk(): 실제 시스템 콜
	시스템 콜 인터페이스 = [table]
		많은 시스템 콜을 효과적으로 관리
		![[Pasted image 20250320014630.png]]
	시스템 콜에서 파라미터를 패싱하는 방법
		1.레지스터에 argument 저장 -> 빠르지만, 레지스터가 제한되어있어 많은 argument를 저장 못함
		2.라이브러리 함수의 argument 들을 커널 영역에 카피해서 전달
		인자가 6개 이하일 때 -> 1번방법
		인자가 6개 이상일 때 -> 2번방법
	시스템 콜 예시
	![[Pasted image 20250320015149.png]]
		1.유저 프로세스에서 라이브러리 함수 호출(포크)
		2.함수에서 인터럽트 발생
		3.인터럽트 벡터에서 ISR의 주소 찾아서 커널에 전달
		4.시스템콜 테이블에서 커널의 시스템콜 함수 호출
		위 과정 반복 -> 유저모드와 커널모드를 오고 감

OS의 구조
	1. Simple structure(이해 안해도 됨 -> 사장됨) - MS-DOS
	2. Layered approach(계층구조) -> 대부분의 OS
		몇개의 레이어(레벨)들로 구성되어 있음
		바텀 레이어(layer 0) - 하드웨어
		가장 높은 레이어 - 유저 인터페이스
		특징
			1. 하부 레이어에서 제공되는 인터페이스를 상부 레이어에서 접근 -> 반대는 성립x
			2. 각 레이어는 독립적으로 작동
	3. 마이크로커널 (<-> 모놀리틱 커널 - 리눅스, 윈도우, 안드로이드...)
		가능한 많은 것을 유저레벨로 넘김
		![[Pasted image 20250320021118.png]]
		장점: 확장성 용이, 포트 쉬움, 오류적음->신뢰도 상승
		단점: 메세지 패싱(통신)이 많이 일어남 -> 기능을 프로세스로 올려서 통신 증가
	4. 모듈 - OS를 계층이 아닌 기능으로 분류한 것
		각각의 모듈은 동적으로 로딩, 언로딩이 가능 - Dynamic loading, unloading
		![[Pasted image 20250320021422.png]]
		다이나믹 모듈 로딩(링킹) -> 드라이버가 타겟
			:프로세스가 런타임 중일 때 커널을 멈추지 않고도 모듈 추가, 제거 가능
		리눅스에서
			insmod driver.o -> 커널 내에 driver.o 추가됨
			rmmod driver -> 커널 내에 드라이버 삭제됨
	5. 버츄얼 머신 - 하나의 OS(Host) 위에서 다른 OS(Guest) 실행
		하드웨어의 일부를 가상 OS에 할당 -> 하드웨어 공유
		![[Pasted image 20250320021818.png]]
		![[Pasted image 20250320021848.png]]
		호스트와 게스트 운영체제의 관점이 다르다

시스템 부팅
	부팅: 커널의 main함수의 첫번쨰 줄 실행까지를 부팅이라고 함(커널에게 제어권 넘기기)
	1. 하드웨어 초기화, 2. 커널 로딩, 3. 커널에게 제어권 넘김
	Boot(strap) loader: 부팅하는 프로그램
		위치: 1. ROM이나 플래시 메모리의 첫째 칸에 있음
		2. 디스크의 첫째 칸에 부트블록 넣어두고, RAM에 올림

# 3장
프로세스 - 실행중인 프로그램/job, task라고도 부름
	프로그램과의 차이점
		1. 프로세스 -> 메모리 / 프로그램 -> 메모리에 x
		2. PCB -> 프로세스는 pcb가짐, 프로그램은 x
		3. 상태 -> 프로세스는 state 있음, 프로그램은 x
	Process Address Space - cpu가 바라보는 프로세스 공간. 모든 프로세스가 가짐, virtual 주소임
		스택 - 로컬변수, 함수 파라미터 - 동적임
		힙 - malloc한 공간 - 동적임
		PCB(프로세스 컨트롤 블럭) - 프로세스 디스크립터, 구조체임
		프로그램 카운터 - 특수목적의 레지스터(+스택 포인터)
		프로세서 레지스터
		![[Pasted image 20250320170938.png]]
	프로그램은 passive entity, 프로세스는 active entity
		-> stack, heap이 계속 바뀜 - active
	
프로세스 state
	new - 프로세스가 만들어질 때
	running - 실행중
	waiting - 이벤트 발생 대기중(입력 등), sleep함수, 시그널, 디버깅
	ready - 프로세스가 할당되기를 대기중
	terminated - 종료됨
	![[Pasted image 20250320172055.png]]

|         | 실행가능 | 실행중 | I/O | sleep |
| ------- | ---- | --- | --- | ----- |
| ready   | O    | X   | X   | X     |
| running | O    | O   | X   | X     |
| waiting | X    | X   | O   | O     |
	1.스케쥴러 - 결정을 하는 역할
	2.Dispatcher - 컨텍스트 스위치 - 다른 프로세스로 전환함(기존에 수행되던 프로세스의 레지스터 값->constext을 PCB에 저장 후, 다른 context로 전환함)
	ISR이 종료될 때, 스케쥴러 호출
	->우선순위 높은 task등장시 컨텍스트 스위칭
	->원래 프로세스는 ready가 됨

좀비 state(리눅스)
	프로세스는 종료됐지만, 완전히 사라지지 않음
	PCB, 리소스 등은 남아있음 -> 부모프로세스가 거둬들이지 않은 결과

PCB(프로세스 컨트롤 블록) - 프로세스의 모든 정보가 담김
	-프로세스 상태
	-프로세스 카운터(다음 연산)
	-CPU 레지스터
	-CPU 스케쥴링 정보 등
	![[Pasted image 20250326141759.png]]
	![[Pasted image 20250326142122.png]]

프로세스 스케쥴링	
	Ready 상태에서 수행 할 프로세스 정하기
	프로세스 스케쥴링 큐 - 프로세스들의 대기줄
		==Ready queue== : ready 또는 waiting 상태의 프로세스가 메인 메모리 안에서 기다림 - CPU 스케쥴러가 고름
		==Device queues== : I/O디바이스를 기다리는 프로세스들이 모임 - I/O, storage, disk가 고름
		프로세스들은 여러 큐들을 옮겨다님
	프로세스는 아래 두가지로 분류됨
		**I/O-bound process** -> 우선순위 더 높음, 대기시간 짧음, 많은 수의 짧은 cpu 실행
		입출력 작업에 의해 실행 속도가 제한됨
		**CPU-bound process** -> 대기시간 김, cpu 길게 사용
		cpu의 계산 능력에 의해 실행 속도가 제한됨
		
		스케듈러가 I/O바운드 프로세스에게 더 높은 우선순위 부여

스케쥴러
	호출되는 시점에 따른 분류
	1. 롱-텀 스케쥴러 : X현재엔 의미가 없음
		프로세스의 개수에 제한을 둠
	2. 숏-텀 스케쥴러 : 자주 호출, CPU 스케쥴러(흔히 말하는 스케쥴러)
	3. 미디엄-텀 스케쥴러 : 가끔등장
		어떤 프로세스를 swapping in/out 할지 결정

Context switch
	Dispatcher가 수행
	CPU가 다른 프로세스로 스위치 할떄:
		올드 프로세스의 상태를 저장, 새 프로세스의 상태를 불러옴
	컨텍스트 스위치는 시간 비용이 큼
	"load/store multiple instruction" : 하나의 연산을 여러개하는 방식 -> 시간 단축

프로세스 생성/종료
	부모 프로세스가 자식 프로세스를 생성함
	-> 트리 형태로 만들어 짐
	이슈들:
		리소스 공유 옵션 - 자식은 부모의 리소스의 일부를 공유함(파일 등)
		실행 옵션 - 부모와 자식은 동시에 실행됨, 부모는 자식이 종료될 때 까지 기다림
		주소 공간 - 자식이 부모의 주소 공간을 복제함, 자식은 새로운 프로그램을 load함
	리눅스/유닉스의 경우
		fork 시스템 콜 사용
			부모와 자식 간 파일 공유, CPU time이나 메모리는 공유x
			부모와 자식이 동시에 실행
			복제하지만, 분리된 주소 공간 사용
			fork는 부모에 의해 호출 됨, 리턴은 두번 됨(부모1 자식1)
		child
			1. process address space(복제됨) - 자식 프로세스 PCB의 text, data엔 부모 프로세스의 코드가 복사되어 들어감 -> 부모프로세스와 text, data가 같음
			2. call once return twice - 리턴값으로 자식과 부모 구분
				1. pid == 0 자식
				2. pid > 0 부모
				3. pid < 0 에러
		자식이 exec 시스템 콜로 부모와 다른 프로그램 실행
		부모는 wait 시스템 콜로 자식 프로세스가 끝날 때까지 기다리고, 끝나면 거둠 -> 이때 waiting 상태임
			부모가 wait 함수를 호출하지 않았을 때, 자식이 먼저 종료됨 -> 좀비 프로세스
			꼭 이렇게 작성할 필요는 없음
		.
		Copy On Write
			 fork시 부모의 코드를 복사하는 것은 비싼 연산, 자식은 어차피 exec으로 새로운 프로그램을 실행하기 때문에 필요 없음=> 최근엔 COW사용
			 부모의 address space를 공유해서 쓰다가, exec호출시 복제하여 각자의 address space를 갖게 됨

프로세스 종료
	1. exit 로 나 자신을 종료시킴
	2. abort로 다른 프로세스를 종료시킴(부모가 자식 프로세스 종료)
	부모 프로세스가 종료될 때, 모든 자식 프로세스를 종료시키는 것 - cascading termanation(연쇄 종료)
	고아 프로세스 - 모두 종료시키거나 다른 프로세스(init)의 양자로 넣어줌

Inter-process communication (IPC) => 커널 활용
	1. Message passing => 커널활용 == 시스템콜
	마이크로커널에서 자주 씀
	send, post 할 때 마다 시스템콜 계속 발생함
		1. mailbox(create)
		2. send (post)
		3. receive (pend)
	2. Shared memory
		1. shmget 특정 주소를 타 프로세스에게 공개
		2. shmat (attach) 타 프로세스의 주소 접근
		이 시스템콜 2번 호출 후엔, 추가적인 시스템 콜 X => 더 빠름
		Producer-Consumer problem
			프로듀서(버퍼에 데이터를 공유하는 프로세스)와 생산자(버퍼에 공유된 데이터를 소비하는 프로세스)문제
			프로듀서는 버퍼가 꽉 차면 대기, 소비자는 버퍼가 비어있으면 대기해야 함
			=> 동기화 필요함 - Spin lock(busy looping) 기법으로 동기화
			프로듀서 - while(Buffer is full); 컨슈머 - while (Buffer is empty);의 무한 루프를 추가해 줌으로서, 동기화시킴

Message passing
	링크 연결
		1. 다이렉트 커뮤니케이션 (유닉스의 pipe) -> 불편
		프로세스는 반드시 각자의 PID를 알아야 함 - PID 알기 불편함
		send(P, meaasage) - P에게 메세지 전달
		receive(Q, message) - Q로부터 메세지 받기
		2. 인다이렉트 커뮤니케이션 -> 대부분. mailbox
		메일 박스를 통해 간접적으로 메세지 전달
		-> PID 알 필요X
		각각의 메일박스는 유니크한 id가 있음
		프로세스들은 메일 박스가 공유된 경우에만 통신할 수 있음
	.
	메일박스 사용 시 발생하는 이슈
		1.하나의 메일박스를 여러 프로세스가 공유
		2.여러개의 메일박스를 두개의 프로세스가 공유
		모두 가능함
	P1이 send, P2, P3이 받을 때
		1.메일박스를 최대 두 개의 프로세스만 연결하게 제한(receive를 제한)
		2.receive를 한 모든 프로세스가 받음(sender가 요청)
		3.우선순위가 높은 프로세스에게 보냄
		4.sender가 받을 프로세스 선택
		

메일박스 synchroniaztion
	Blocking => 동기화
	Non-blocking => 비동기화
	![[Pasted image 20250401154342.png]]
	send는 논블록킹이 디폴트(그냥 메세지 보내고 계속 할일함)
	receive는 블록킹이 디폴트(메세지 올 때 까지 기다림)
	=>I/O에도 동일한 이슈 있음
	Blocking send: 메세지를 리시버나 메일박스가 받을 때 까지 waiting
	NonBlocking sned: 메세지를 보내고 할일 함
	Blocking receive: 메세지가 올 때 까지 기다림
	NonBlocking receive: 안와도 밑의 라인 수행

메세지 큐가 링크에 attach하는 것을 구현
	1. Zero capacity(랑데뷰) - 메세지를 저장할 수 없는 큐 => 메세지 전달 없이 리시브 요청이 있었는지 확인, sender는 반드시 receiver를 기다려야 함, 버퍼링 없음
	2. Bounded capacity - 정해진 최대 크기를 가진 큐, 큐에 저장할 수 있는 메세지의 수가 제한되어 있음 => 링크가 꽉 찰시 센더가 기다려야 함 - 자동 버퍼링
	3. Unbounded capacity - 무제한의 메시지 저장, 실제로는 시스템의 메모리 한계에 따라 결정 => 센더가 기다릴 일 없음 - 자동 버퍼링

Shared memory VS Message passing
	Message passing
		- 작은 정보 교환시 유리
		- 프로그래밍 쉬움
	Shared memory
		-빠름
		-Spin lock, Busy looping등 프로텍션 메카니즘 필요


# 4장
쓰레드 & concurrency

Motivation
	많은 프로그램들은 여러 클라이언트의 리퀘스트를 처리해야 함
	서버 프로그램 - 리퀘스트마다 담당하는 프로세스 만듦, 해당 파일을 읽고 전송해 줌 -> 하는 일이 똑같음
	![[Pasted image 20250402143831.png]]
	fork는 비싼 연산임 - 별도의 address space 필요, 프로세스간 통신 필요
	![[Pasted image 20250402143951.png]]
	프로세스와 스레드의 차이
	프로세스 - fork로 생성
	스레드 - pthread-create
	pthread_create(&tid, NULL, &func, argument);
	NULL: 디폴트, 다른 옵션 가능
	&func: 핵심, 스레드에서 수행 할 함수 명, 미리 정의해야 함
	pthread_create(&tid2, NULL, &func2, argument);
	&func2: 별도의 함수 => 하나의 프로그램 내에서 여러가지 일 동시에 수행
	![[Pasted image 20250402144101.png]]
	스레드는 Data, Text는 공유하면서, 별도의 stack과 레지스터를 가짐
	![[Pasted image 20250402144221.png]]
	스케쥴러가 각 스레드마다 스택과 레지스터를 별도로 할당함

스레드의 장점
	1. 반응성 - 하나가 waiting이어도 나머진 할일 가능 -> 별도의 state를 가짐
	2. 자원 공유 - 코드(text)와 data, 프로세스 자원을 공유함
	3. 효율 - fork보다 생성이 더 빠르고, 컨텍스트 스위치도 . 더빠름
	4. 멀티코어를 병렬로 사용 가능

구조 컨셉
	프로세스는 2파트로 나뉨
	- 스레드의 집합
	- 자원의 모음
	![[Pasted image 20250403154033.png]]

멀티코어 프로그래밍
	병렬성의 타입
	- Data 병렬성: 하는 일은 같지만, 넘겨준 data가 다름 ( + )
	- Task 병렬성: 하는 일이 다름 (+, -, x)
	concurrent - 동시성, 병행성
	Parallelism - 병렬성
	.
	*왜 멀티코어 프로그래밍이 어려운가?*
		1. task 아이덴티파잉
		2. 밸런스
		3. 데이터 스플릿
		==4. 데이터 의존 (여러 스레드가 같은 data 사용 시)==
		==5.테스트 및 디버깅 어려움==
		+ critical section을 해결해야 함

멀티 스레딩 모델
	커널 스레드와 유저 스레드는 각각 독립적으로 생성 및 관리됨
	- 커널 스레드: 커널에서 실제로 할당되어 실행되는 스레드
	- 유저 스레드: 유저 라이브러리가 제공
		pthread_create -> 유저 라이브러리임
		gcc -lpthread a.c
		g++ -lpthread a.cpp
		==>>디버깅 시 -lpthread 추가해줘야 함
	sched_setaffinity => 리눅스에서 유저가 커널에 할당 요청하는 시스템 콜
	커널 스레드와 유저 스레드 mapping 필요
	![[Pasted image 20250403155847.png]]
	- 스케쥴러는 유저 스레드의 존재를 모름
	- 스케쥴러는 ka를 호출 -> UA UB UC 모두 호출
	- ka가 wating이면, UA UB UC도 모두 waiting
	- LWP(Lightweight-process)가 mapping시켜줌
	- ka가 수행되면 UA, UB, UC중 하나 실행 => 스레드 라이브러리가 결정
	- ka가 블락되면 모두 블락 (UA가 디스크 I/O호출하면 UA, UB, UC 모두 블락)

멀티스레딩 모델2
	- Many-to-one
		![[Pasted image 20250403162158.png]]
		매우 빠르고, 적은 오버헤드
		커널 스레드가 블락되면, 모든 유저 스레드 블락
		하나의 코어만 사용해서 (멀티스레딩)의미가 없음
	- One-to-one
		![[Pasted image 20250403162358.png]]
		최근에 주로 사용하는 모델
		멀티코어 사용
		Bound thread라고도 함
	- Many-to-many
		![[Pasted image 20250403162512.png]]
		Multiplexing - LWP가 하는 일(매핑)
		Mapping이 복잡함 -> LWP가 바빠짐
		원투원과 매니투원의 장점을 합침
		- 스레드 많이 만들기 가능
		- 병렬 가능(멀티코어)
		- 모두 Block 안됨(유저 스레드가 블락되어도 커널 스케쥴러가 다른 커널 스레드로 매핑시켜줌)
		![[Pasted image 20250403163041.png]]
			유저 스레드가 블락됨 -> 커널 스레드가 LWP에게 upcall을 보냄 -> LWP가 다른 커널 스레드로 다시 mapping 해줌
	-Two-level model
		매니 투 매니 + 원 투 원 모델
	.
	리눅스에서
		1. mapping, LWP 없음
		2. 유저 스레드에 PCB 별도로 할당 - fork와 pthread_create 상관 없이 thread마다 PCB 제공 => 스레드마다 별도로 스케쥴링
		3. 커널 스레드는 커널의 . 할일만 자체적으로 함 - 유저스레드와 매핑x

스레드 프로그래밍 예시
	![[Pasted image 20250404235005.png]]
	pthread_create
	phtread_join - 메인 스레드에서 호출함. 생성된 스레드가 종료되길 기다림
	pthread_exit - 하면 join함수에서 거둠

스레딩 이슈들
	1. 스레드 함수에서 fork 호출 시? => exec이 즉시 실행되면 exec를 실행한 스레드만 복제, 실행 안하면 모든 스레드가 복제됨
		-실제로는 이런짓 안하는듯 
	2. 스레드가 종료될 때, 옆의 스레드는?
		1. 모두 종료
		2. 나중에 종료

시그널 핸들링
	이벤트 발생마다 커널의 프로세스에 signal 보냄 -> 시그널 핸들러
		우선순위
		1. 유저 시그널 핸들러 -> 시그널 핸들러로 유저가 직접 정의
		2. 디폴트 시그널 핸들러 -> 컨트롤c, 컨트롤z, 예외(0으로 나누기 등)
	시그널이 전달되는 곳
		1.시그널이 발생한 스레드(0으로 나누기 등)
		2.프로세스 안의 모든 스레드(컨트롤c)
		3.프로세스의 특정 스레드
		4.특정 스레드가 모든 시그널을 받도록 명시(pthread_kill)
		pthread_kill(pthread_t tid, int signal)->특정스레드에게 시그널 전달하는 함수

스레드 pools
	프로세스마다 생성되는 스레드의 수 제한 or 스레드를 미리 만들어 두기 -> 시스템 안정화를 위해
	스레드를 미리 만들어 둠 -> waiting 상태, 요청이 오면 ready상태로 전환
		장점: pthread함수를 계속 호출하지 않아도 돼 빠름
	![[Pasted image 20250407230641.png]]

POSIX 스레드 프로그래밍
	스레드 생성
	![[Pasted image 20250407230816.png]]
	종료된 스레드 거두기(기다리기)
	![[Pasted image 20250407230936.png]]
	스레드 종료시키기
	![[Pasted image 20250407231015.png]]
	thread_return의 리턴 값과 함께 스레드 종료 후, 리턴값은 join에게 전달
	

# 5장
###### 프로세스 스케줄링 -> ready상태인 프로세스 중 하나 선택

선점형 vs 비선점형 스케쥴링
	CPU스케쥴러는 메모리 안의 ready상태인 프로세스를 선택해 cpu에 할당함
	CPU 스케줄링이 발생하는 상황:
		1. 러닝중인 프로세스를 waiting으로
		2. 러닝중인 프로세스를 ready로
		3. waiting중인 프로세스를 ready로
		4. 종료(러닝중인 프로세스 종료)
		-> 1, 4번이 비선점형 -> 먼저 수행되는 코드가 자발적으로 CPU사용 끝냄
		Nonpreemptive 비선점형: 자발적으로 CPU 놓음
		preepmtive 선점형: 타이머 인터럽트 때 마다 선택
		시스템 콜이나 인터럽트로 선점형 수행
		![[Pasted image 20250408113044.png]]
		리눅스에서는 인터럽트로 preemption 안됨

스케쥴링 기준
	전체 시스템
		CPU 효율 -> CPU가 노는 시간을 없애서 효율 상승
		Throughput -> 특정 시간 내에 종료되는 job의 개수 증가
	개별 프로세스(평균으로)
		Turnaround time -> 프로세스가 끝나는데 걸리는 시간
		Waiting time -> ready큐에서 대기한 총 시간(턴어라운드 타임에 포함)
		Response time -> 첫번 째 응답이 오는데 걸리는 시간
		이 세가지의 평균을 최소화 시키는게 목적

스케쥴링 알고리즘
	First-Come, First-Served (FCFS)
	간트차트
	![[Pasted image 20250408114229.png]]
	평균 대기 시간이 높음
	FCFS의 다른 경우
	![[Pasted image 20250408114338.png]]
	평균 대기 시간이 훨씬 짧음
	==Convoy effect==
	긴 프로세스가 먼저 오는 것 -> 안좋음
	work-load에 따라서 성능차이 많이남 -> 안좋음
	.
	Shortest-job-First(SJF)
		평균 대기 시간을 줄이는데 최적
		프로세스의 미래 수행시간을 몰라 실질적으로 사용 못함
		![[Pasted image 20250408130408.png]]
		대기시간, 실행시간 모두 최적
		.
		![[스크린샷 2025-04-10 오전 1.48.09.png]]
		비선점형이라서 P1이 가장 먼저 도착함으로 먼저 실행, 중간에 바뀌지 않음
		.
		![[Pasted image 20250410015031.png]]
		선점형은 먼저 실행되는 프로세스가 있어도, 중간에 수행시간(남은 수행시간)을 비교해서 더 짧은 프로세스 수행함.
		waiting time은 레디큐에서 기다린 총 시간
		.
		SJF를 사용하기 위해 프로세스의 예상 수행시간이 필요
		==Exponential Averaging== 로 구함
		1. 과거의 CPU burst 길이
		2. 최근의 가중치를 높게 잡음
		![[Pasted image 20250410015801.png]]
		![[Pasted image 20250410015834.png]]
		α가 0일 때: 현재까지의 예측시간 (타우 엔)그대로 씀
		α가 1일 때: 이전 수행시간 그대로 씀
	.
	Priority scheduling
		프로세스의 우선순위 순으로 스케쥴링
		==starvation== 발생 -> 우선순위 낮은 프로세스는 실행이 안됨 -> Static priority
		==Aging==으로 해결 -> 오랫동안 실행이 안된 프로세스의 우선순위 조금씩 높이기 -> Dynamic priority -> RTOS
	.
	Round-robin scheduling(RR) -> 돌려쓰기
		time quantum마다 컨텍스트 스위칭 발생
		->starvation 발생 안함
		quantum이 많을 수록 컨텍스트 스위칭이 자주 일어남 -> 자원 소모 심함
	.
	Multilevel Queue scheduling (리눅스)
		-프로세스들을 여러 개의 큐로 나누어서 관리하는 방식
		-가장 최근 방식, 기본 방식들을 혼합하여 사용
		-각 큐마다 자체 스케줄링 알고리즘을 가짐
		-규 간의 스케줄링도 필요함 -> 각 큐별로 1등끼리 다시 스케줄링
	.
	Multilevel Feedback Queue
		멀티레벨 큐에서 프로세스가 각 큐를 이동할 수 있는 것
		-> + 언제 큐에서 이동할 것인지 결정하는 방법 필요
		

Multiple-Processor Scheduling
	스케줄러는 결정, 할당의 역할도 있음
	->어떤 코어에 할당해줄 것인지 고민해야 함
	Master-slave 관계 발생
	이슈
	1. 프로세스 어피니티 -> NUMA(non uniform memory access, 비균일 메모리 접근, 메모리 접근 시간이 프로세서와 메모리의 물리적 위치에 따라 달라짐)에서 중요
		sched_setaffinity 시스템콜로 특정 코어에 할당 가능 => 하드 어피니티
		소프트 어피니티는 보장x
	2. 로드 밸런싱
		Push migration - load가 적은 CPU에게 push해줌
		Pull migration - load가 많은 CPU에게 가져옴 => 리눅스

리눅스 스케줄링 (multilevel queue 사용)
	기본적으로 스케줄링 클래스에 기반
	real-time과 conventional 간의 스케줄링: 우선순위
	(real-time)class 0~99
	-priority (FIFO먼저온거먼저, RR번갈아가며) -> 우선순위게 젤 높은게 2개 이상일 때 사용
	(conventional)class 100~139 -> 기본적으로 fork나 pthread 생성시 여기로 감, pthread_create 2번째 인자로 NULL 대신 sched_set_scheduling policy를 사용하면 Real Time에 넣을 수 있음
	-completely fair scheduler(CFS) 공정하게
		우선순위에 기반하여 비율만큼 CPU 할당
		vruntime 메카니즘으로, 최대한 공평하게 할당
		![[Pasted image 20250415092744.png]]
		-가중치 (weight): 프로세스의 우선순위(nice 값)에 따라 결정됨. nice 값이 낮을수록(우선순위가 높을수록)가중치가 커지고, nice값이 높을수록(우선순위가 낮을수록)가중치가 작아짐
		-기본 가중치 상수(NICE_0_LOAD): 일반적으로 nice 0에 해당하는 가중치(1024)를 기준으로 사용.
		vruntime을 각 시점마다 계산하여 작은 값을 선택함, 같으면 이전에 실행되지 않은 것 실행
		![[Pasted image 20250415093137.png]]


# 6, 7장
프로세스 동기화(synchronization)

Background
	==Race condition== 아웃풋이 스케줄링 순서에 따라 달라짐
	여러 스레드에서 하나의 전역변수(shared variable)에 동시에 접근할 때 발생(버퍼는 공유 변수지만, read시 수정이 안됨 -> 상관x)
	-> critical section에 세마포어 적용해서 방지
	![[Pasted image 20250415095910.png]]
	코드의 명령어는 atomic하지 않음 -> 여러개의 레지스터 연산으로 구성됨. 각각의 레지스터 명령어 자체가 atomic
	race condition으로 인해 제대로 반영되지 않은 명령어 -> lost update
	해결법: 여러개의 레지스터 명령어들을 모아 atomic 연산으로 묶어줌

Critical-section problem
	크리티컬 섹션: 코드에서 shared data에 접근하는 영역
	entry section에 spin lock, busy looping 코드를 넣어 크리티컬 섹션을 atomic하게 만들 수 있음
	![[Pasted image 20250415100428.png]]
	해결책:
		1. Mutual Exclusion - 프로세스가 크리티컬 섹션에 있을 때, 다른 프로세스는 못들어감 -> 오직 1개만 접근
		2. Progress - 크리티컬 섹션이 비어있을 때, 누군가 접근하려고 하면 막지 않아야 함
		3. Bounded Waiting - 순서대로 크리티컬 섹션에 진입해야 함 -> 프로그래머가 순서 정함
	화장실 칸과 비슷
	공유변수가 다르면 critical section이 아님!!

피터슨's 솔루션
	![[Pasted image 20250415100752.png]]
	파란색 코드들을 추가하여 Race condition 방지
	크리티컬 섹션에 프로세스 2개가 있으면? -> turn이 i, j 중 하나만 가질 수 있기 때문에 성립 X


Mutual exclusion, Progress, Bounded waiting을 만족하기 위한 Instruction(test and set, swap) 지원(크리티컬 섹션의 솔루션)
	피터슨 솔루션 대신 사용 (주로 세마포어)
	1. TestAndSet
		![[Pasted image 20250417020118.png]]
		boolean 타입의 타겟을 받아 TRUE로 만들고, 처음 받은 타겟 그대로 리턴(인자 값을 그대로 리턴)
		.
		![[Pasted image 20250417020334.png]]
		lock(타겟)이 FALSE여야 크리티컬 섹션 진입 가능 -> 진입하면서 lock을 true로 바꿔줌
		다른 스레드에선 lock이 true가 되어서 while루프에 갇히게 됨
	2. swap
		![[Pasted image 20250417020627.png]]
		인자로 들어오는 두 변수의 값을 교환해줌
		![[Pasted image 20250417022759.png]]
		lock = false(key = false)일 때 while루프를 빠져나가며 크리티컬 섹션 진입 가능 -> lock과 key를 교환하여 lock = true가 됨 -> 잠김
		크리티컬 섹션 끝난 후 lock = false로 바꿈 -> 열림
	위 두개는 Bounded waiting 만족 x
	3. 세마포어
		크리티컬 섹션에 접근하기 위한 integer 변수
		semaphore S -> 초기값 1로 설정
		wait() : s-- / signal() : s++ 이 있음
		![[Pasted image 20250417024903.png]]
		wait 함수가 s를 0으로 만들고, 다른 스레드의 크리티컬 섹션 접근 제한
		끝나면 signal 함수가 s를 1로 만들어 제한 해제 -> wait, signal 연산은 atomic함
		.
		Counting semaphore -> 초기값이 1보다 큰 세마포어 - 크리티컬 섹션 문제에는 적합하지 않음
		Binary semaphore(=mutex) -> 초기값이 1인, 0과 1밖에 없는 세마포어
	4. busy looping이 없는 세마포어
	(1~3은 busy looping이 있음)
		크리티컬 섹션에 접근하는 2번째 스레드부터 state를 waiting으로 바꿈
		-> 커널 필요 -> 시스템 콜 필요
		block 연산으로 waiting으로 바꿈
		wakeup 연산으로 ready로 바꿈  ---> 커널에게 요청(둘 다) 
		![[Pasted image 20250417030311.png]]
		- wait가 PCB에 프로세스 리스트에다가 기다리는 프로세스 등록
		![[Pasted image 20250417030329.png]]
		- 등록된 프로세스는 waiting 상태로 변함
		- signal에서 리스트에서 기다리는 프로세스 제거 후 ready큐로 올림
		while이 없어 비지루핑 아님
		Bounded waiting도 만족함
	.
	짧게 기다릴 땐 busy loop, 길게 기다릴 땐 without busy loop가 좋음

리눅스에서의 atomic variable
	리눅스는 preemptive kernel을 사용하기 때문에, 커널에 있는 시스템 콜 수행 중에 컨텍스트 스위칭으로 다른 프로세스가 같은 시스템 콜 호출 시 동일한 크리티컬 섹션에 진입할 수 있음
	.
	리눅스 커널에서
	1. atomic operation
		atomic 타입의 변수 선언
		atomic_t v;
		atomic_set(v,4); = (v = 4);
	2. 세마포어(비지루핑x)
	3. 스핀락(비지루핑o)
	4. preemption disable
	5. interupt disable
		4, 5번은 커널 자체에 영향을 주기 때문에 strong, 바람직하진 않음



# 8장
데드락

데드락 문제
	프로세스와 resource와의 관계에서 발생
	![[Pasted image 20250528101122.png]]
	차가 프로세스, 섹션을 리소스에 비유
	각자 리소스를 하나씩 소유하면서 놓지않고, 다른 리소스 요청 -> 데드락
	.
	해결법:
	1. 프로세스 죽이기(일반적)
	2. 리소스를 Preempt하고 돌아가기(context로)
		가장 최근의 체크포인트로 돌아감
	데드락 방지는 어려운 프로세스임
	1.데드락 detection
	2.데드락 avoidance
	3.데드락 recovery
	4.데드락 prevention
	으로 해결

시스템 모델
	자원의 종류
	-CPU 사이클, 메모리공간 I/O장치 등
	-파일, 세마포어 등
	-> counting semaphore 사용 (최대 4개까지)
	각 프로세스는 자원을 다음과같이 사용
	-리퀘스트
	-사용
	-릴리즈
	Hold & request 때문에 데드락 발생(데드락의 필요 조건, 반드시 일어나는건 아님)
	: 자원을 가지고 있으면서 다른 자원 요청

데드락 characterization(특성화)
	필요조건
		-Mutual exclusion, No preemption은 자동 가정
		- Hold and wait: 자원을 가지고 있으면서 추가 자원 요청
		- ==Circular wait==: 프로세스들이 서로가 가진 자원을 기다리며 원형으로 대기하는 상황
		Hold & wait의 하위호환이며, 핵심 필요조건. 없으면 데드락도 없음
		<math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><msub><mi>P</mi><mn>0</mn></msub><mo>→</mo><msub><mi>P</mi><mn>1</mn></msub><mo>→</mo><msub><mi>P</mi><mn>2</mn></msub><mo>→</mo><msub><mi>P</mi><mn>0</mn></msub></mrow><annotation encoding="application/x-tex">P_0 \to P_1 \to P_2 \to P_0</annotation></semantics></math> 원형으로 순환 대기

리소스 allocation 그래프
	P -> R : 리퀘스트 엣지
	R -> P : 할당 엣지
	![[Pasted image 20250528110526.png]]
	![[Pasted image 20250528110553.png]]
	그래프 그려서 사이클 없으면 데드락X
	사이클 있으면 데드락 있을 수 있음(1개 - 데드락, 여러개 - 있을 수 있음)

데드락 핸들링 방법
	데드락 절대 일어나지 않게: avoidance, prevention
	데드락 허락 후 recovory
	데드락 발생해도 무시하고 관여안하기

데드락 Prevention
	4가지 데드락 필요 조건 중 하나 이상 제거
	부작용이 있거나, 불가능함
	![[Pasted image 20250528112501.png]]
	- 상호 배제 - 없애기 불가능(자원 무조건 사용해야 함)
	- Hold & wait - 부작용(효율 떨어짐), 모든 리소스를 할당 받고 시작함(모든 리소스가 이용 가능해야 함)
	- No Preemption - 불가능(대부분의 자원은 비선점형임, 프린터 인쇄중 선점하면?)
	- Circular Wait - 부작용(효율 떨어짐), 모든 리소스 타입의 접근 순서 미리 정함

데드락 Avoidance
	요청량보다 사용 가능한 자원이 많거나 같으면 데드락 발생X
	-> safe state: 항상 보장이 될 수 있도록 프로세스 순서 정함
	앞 프로세스가 자원 사용중이면, 뒤 프로세스들은 멈춤
	->프로세스가 요구하는 자원 양을 알아야 하는데, 코드를 보면 알 수 있음
	Safe state: Demand <= 현재 이용 가능 + 앞의 프로세스가 반환하는 자원의 양
	1) 리소스 얼로케이션 그래프 -> 리소스 타입이 싱글일 때 사용
	![[Pasted image 20250528124633.png]]
	P2의 R2 요청을 차단하면 데드락x
	2) 뱅커's 알고리즘(미래 요청값) -> 리소스 타입이 멀티플일 때 사용
		- 은행원(운영체제)은 고객(프로세스)이 요청한 자원을 대출(할당)하기 전에, 대출 후에도 모든 고객이 작업을 완료할 수 있는지 확인.
		- 만약 대출이 안전하지 않다면(데드락 가능성이 있다면), 대출을 거부하고 고객이 기다리게 함.
		1. Available (가용 자원 벡터)
		2. Max(최대 요구 행렬)
		3. Allocation(할당 행렬)
		4. Need(필요 행렬)
		![[Pasted image 20250528130551.png]]
		P2-P3-P4-P1 순서로 (for loop라 P1부터 체크)
		Need값이 Available보다 작거나 같으면 그 프로세스 수행
		-> 끝나면 allocation 값을 available에 추가

데드락 detection(감지)
	single - RAG 그리기(사이클 없애기), multiple - Banker's 알고리즘 (avoidance와 비슷)
	![[Pasted image 20250528131041.png]]
	avoidance 는 Need = MAX - Available
	Detection 은 Need = Real requests

데드락 Recovery
	detection알고리즘이 데드락이 존재한다고 판단했을 때 발동
	1. 프로세스 종료(kill)
	- 프로세스의 우선순위, 요청하는 자원 양 등을 파악하고 종료 프로세스 결정
	2. 데드락이 발생하면 체크포인트로 롤백(잘 안씀)

Dining-Philosophers 문제
	deadlock 설명
	![[Pasted image 20250528132527.png]]
	밥을 먹으려면 양쪽이 비어있어야 함
	초기값이 1인 세마포어로 구현 가능
	밥그릇이 shared data
	세마포어 chopstick[5] -> 1로 초기화
	.
	i -> thread
	{
		wait(i);
		wait((i + 1) % 5);
		eat;
		signal(i);
		signal((i + 1) % 5);
		think;
	}
	wait로 양쪽 들기
	signal로 양쪽 놓기
	![[Pasted image 20250528133348.png]]
	데드락 발생
	![[Pasted image 20250528133424.png]]
	모든 스레드들이 첫 번째 줄 (wait)수행 후 컨텍스트 스위칭
	-> 왼쪽만 잡게됨
	예방책
	Hold & wait 일 때 -> 두 개를 집을 수 있을 때만 집기
	circular wait 일 때 -> 접근 순서 정하기
	![[Pasted image 20250528133931.png]]
	avoidance 하려면 -> RAG그리기(싱글 인스턴스) 점선이 실선으로 바뀔 때 stop
	recovery 하려면 -> philosopher 1명 쫓아내기

동기화의 고전적 문제
	Bounded buffer problem
	item의 개수가 한정된 버퍼
	->counting 세마포어 사용
	- N개의 버퍼가 있고, 각자 하나의 아이템 hold
	- 세마포어 뮤텍스를 1로 초기화
	- 세마포어 full을 0으로 초기화
	- 세마포어 empty를 N으로 초기화
	![[Pasted image 20250528135023.png]]
	![[Pasted image 20250528135057.png]]
	![[Pasted image 20250528140723.png]]
	


# 9장
메모리 관리 전략

9장에서는 주소가 어떻게 바뀌는지 알아야 함(Address translation)
![[Pasted image 20250530001238.png]]

Overview
	버츄얼 address space와 피지컬 address space 존재
	- 가상 주소 공간은 프로그램상의 주소
	- 피지컬 주소 공간은 실제 물리적 주소, 공간 더 적음
	=> 더 많은 공간을 사용하는 것이 목표
	![[Pasted image 20250528144418.png]]
	버츄얼 주소가 피지컬 주소로 변환되는 과정을 알아야 함
	피지컬 메모리보다 더 큰 공간을 사용함
	-> 디스크로부터 불러와야 함
	-> 원래 쓰던 것 메모리에서 빼냄

Background - 기본 하드웨어
	==MMU==(memory management unit) -> 베이스 레지스터임
	==contigueus allocation== 모든 프로세스가 연속적으로 저장
	==Base address + virtual address = physical address==
	![[Pasted image 20250529004931.png]]
	베이스 레지스터와 리미트 레지스터의 쌍으로 logical 주소 공간을 정의함
	base: 레지스터에 있음, memory(30004) -> 찾아야 함
	컨텍스트 스위치 될 때 마다 base, limit를 업데이트 함
	![[Pasted image 20250529005155.png]]
	V.A. + base가 base보단 크거나 같고, base + limit보단 작아야 함 -> 아니면 trap 날림

address(physical) binding -> 주소를 확정지음
	symbolic address -> relocatable address -> absolute address
	- 심볼릭 주소(소스 코드 안의 주소): int a; int b;
	- Relocatable address(Compiler): 처음부터 얼마나 떨어져있는지 보여줌(offset) +50
	- Absolute address: 최종 메모리에 넣어질 주소
	.
	Absolute address binding은 언제 일어나는가?
	- Compile time: 컴파일 타임에 결정 안됨, 컴파일 시점에는 프로그램이 메모리 어디에 로드될지 알 수 없음 -> 리컴파일 해야함
	- Load time: 안됨. 프로세스가 메모리에서 디스크에 갔다 다시 메모리에 올라올 때, 같은 위치에 올라오기 힘듦
	- Execution time: 실행시간에 결정, - 프로세스가 실행 중 메모리 세그먼트 간 이동(예: 페이지 교체, 압축)이 가능하다면, 주소 바인딩을 컴파일/로드 타임에 고정할 수 없음. 실행 타임에 바인딩이 지연되어야 MMU가 동적으로 물리적 주소를 갱신하며, 이동에도 불구하고 프로그램이 정상 실행.
		- > MMU의 도움을 받음, Address mapping table 사용

Multistep processing
	![[Pasted image 20250529010113.png]]
	유저 프로그램은 여러 단계에 거쳐 메모리에 올라가게 됨

Background - logical address
	Logical address - CPU에 의해 생성됨, virtual address라고도 불림
	Physical address - 메모리 유닛에게 보이는 주소, 가변적임
	두 주소는 컴파일타임, 로드타임에는 같음
	실행 타임에 주소 바인딩이 달라짐(실행 타임 바인딩)
	![[Pasted image 20250529130025.png]]
	TLB: CPU안에 있는 mapping table의 일부를 저장하는 빠른 공간 -> 적중 확률 높음
	==Memory mapping table== -> 메모리(PCB)에 있음. 실행 타임에 바인딩

Memory Management Unit (MMU)
	1) 레지스터
	2) TLB(translation look-aside buffer)
	주소 mapping하는 하드웨어
	![[Pasted image 20250529130939.png]]

Process address space
	virtual address임
	stack, heap, data, text -> 최대 3기가까지 가능(사실상 4기가)
	.
	각 프로세스의 V.A가 같아도, P.A는 다름
	-> 각자의 프로세스 address space를 가져, 그 안에서의 주소는 같을 수 있음
	프로세스 매핑 테이블 -> PCB(프로세스마다 가짐)안에 있음

Dynamic loading, linking, shared library
	다이나믹 로딩 -> 로딩을 가능한 뒤로 미룸
		실제로 호출될 때 로딩됨 -> 메모리 공간 효율 좋음
		OS가 아닌 라이브러리에서 지원
	다이나믹 링킹(오브젝트 파일을 모아 실행파일로 바꿈) -> 링킹을 실행시간 까지 미룸
		shared library에서 효과적임
	Shared library: 오브젝트 모듈이 실행 타임에 임의의 메모리 주소에 로드되어, 메모리에 있는 프로그램과 동적으로 연결될 수 있는 형태
	![[Pasted image 20250529132516.png]]
	static library는 동적 링킹X, 효율 떨어지고, 라이브러리 업데이트가 필요함(프로그램마다)
	shared 라이브러리는 동적 링킹O, 라이브러리를 메모리의 특정 위치에 넣고, 이것만 업데이트 하면 됨
	![[Pasted image 20250529132742.png]]
	Stub: 중개자 역할을 하는 작은 코드 조각, 라이브러리를 locate하거나 load함
	동적 링커에 라이브러리 주소만 포함하고, 실행시 stub이 라이브러리 주소를 반환함

Swapping
	메모리에서 프로세스를 디스크로 보냄
	swap in: 디스크에서 가져옴
	swap out: 메모리에서 디스크로 쫓아냄
	Backing store - 디스크의 swap영역

Contiguous Allocation
	input: V.A.
	output: P.A.
	프로세스가 연속적으로 저장
	일반 OS에선 안쓰임(너무 단순)
	간단한 시스템에서 사용
	.
	Address Translation
		V.A. = offset
		Base address + V.A = P.A
	Memory protection
		V.A <= limit(프로세서의 크기)
		relocation register(다른 프로세서 등으로부터 보호, OS의 코드 데이터 )
		limit register(logical address의 범위 포함)
	Issues(MMU)
		MMU가 로지컬 주소를 동적으로 매핑함
		Dynamic storage Allocation
		Fragmentation(External)
	![[Pasted image 20250529170944.png]]
	Base address와 offset(V.A.)만 알면 메모리 allocate 할 수 있음

Multiple-partition allocation
	Hole - 비어있는 공간
	메모리 홀에 프로세스가 들어감
	

Dynamic storage allocation
	Hole중에 어디에 넣어야 할지 결정
	First-fit: 들어갈 수 있는 첫번째 홀에 넣기
	Best-fit: 가장 작으면서 맞는 홀에 넣기
	Worst-fit: 가장 큰 홀에 넣기
	-> 무엇이 가장 좋은지는 모름

Fragmentation(단편화)
	==External Fragmentation== - 전체 홀 크기는 프로세스보단 크지만, 연속적으로 넣지는 못하는 문제
	compaction: 큰 홀을 만들어 해결
	==Internal Fragmentation== - 메모리 관리의 최소단위 때문에 발생 -> 계속 못씀(Paging, Disk에서 발생)
	![[Pasted image 20250529173246.png]]

==Paging==
	프로세스를 4KB(Page)단위로 쪼갬
	->External Fragmentation 발생 안함(non contiguous)
	->Internal Fragmentation은 발생
	![[Pasted image 20250529174008.png]]
	Page table(PCB에 있음) 에서 페이지와 프레임의 매핑 정보를 담음
	->프로세스마다 존재(각자의 가상 주소가 있기 때문)
	.
	V.A => P.A 바꾸기
	offset: 페이지의 시작 주소에서 떨어진 양
	frame의 시작 주소 + offset => P.A
	1. V.A 받으면, 그 주소가 속한 Page number 찾음 -> V.A/페이지 크기(4) = page num, V.A % Page-size = offset
	2. Page table에서 해당 페이지의 프레임 넘버 찾기(look-up)
	3. Frame의 시작 주소에서 offset 더하기 -> frame num x page-size + offset
	목: page num, 나머지: offset
	![[Pasted image 20250529174841.png]]
	![[Pasted image 20250529175347.png]]
	![[Pasted image 20250529175600.png]]
	9 = virtual address, 뒤 2비트는 오프셋, 앞은 페이지 
	이진수도 그냥 10진수로 바꾸고 나누기 4, -> 프레임 넘버 찾고 곱하기 4, 오프셋 더하기

Frame table
	비어있는 프레임을 관리할 수 있는 테이블

Paging - hardware support
	페이지 테이블은 메인 메모리에 보관됨
	TLB - 여기에 페이지 테이블 저장함 -> look-up 빨라짐 (레지스터에 저장됨)
	address-space identifiers(ASIDs) -> 각 프로세서의 페이지 테이블 구분(TLB에는 여러가지 프로세서의 Page table 있음)
	페이지 테이블을 병렬로 탐색함
	![[Pasted image 20250529223958.png]]
	EAT(Effective Access Time 실제 메모리 엑세스 타임)
	= 2 + 엡실론 - 알파 (엡실론 = 룩업 시간, 알파 = 히트 비율)
	.
	Memory protection
		Valid-invalid 비트를 페이지 테이블에 추가
		valid - 페이지가 로지컬 주소 안에 있음 - legal page
		invalid - 페이지가 로지컬 주소 안에 없음
	.
	같은 프레임은 다른 로지컬 페이지에 맵핑 될 수 있음 -> 로지컬 주소는 각 프로세스 내에서 결정

페이지 테이블의 구조
	프로세스 하나당 페이지 테이블 크기가 4MB 필요
	- ==Hierarchical Paging==
	- Hashed Page Tables
	- Inverted Page Tables
	=>> 테이블 크기 줄이기
	==Hierarchical Paging== - 페이지 테이블 조차도 여러개의 페이지로 구성됨
	two-level page table
	![[Pasted image 20250529235747.png]]
	![[Pasted image 20250529235817.png]]
	2레벨 페이지 테이블 에서의 16비트 로지컬 주소 공간
	4KB (2^12) page, 2^10 inner page, up to 2^52 entry
	![[Pasted image 20250529235956.png]]
	![[Pasted image 20250530000018.png]]
	3 레벨 페이지 테이블 -> 더 많이 쪼갤 수 있음
	.
	Hashed page table - 매핑 과정에 해시함수 사용
	Inverted page table - OS에 페이지 테이블이 1개만 있음
		페이지 테이블이 프로세스마다 하나씩 있어서 너무 큼
		단점: 전체 테이블을 탐색해야 함
		![[Pasted image 20250530000219.png]]
	리눅스의 페이지 테이블은 3레벨로 구성됨

==Segmentation==
	프로세스의 메모리를 논리적 단위인 세그먼트로 나누어 관리하는 방식, 각 세그먼트는 프로그램의 의미 있는 부분(함수, 배열, 스택 등)으로 나뉘며, 크기가 가변적임
	contiguous allocation과 비슷 - 각 프로세스가 연속적임
	<=> 각 segment는 contiguous함
	리눅스는 segmentation + paging 둘 다 씀(protection에 효과적)
	![[Pasted image 20250530001326.png]]
	로지컬 주소는 두개의 튜플로 구성됨
	<segment-number, offset>
	*Segment table* - 2차원 피지컬 주소를 매핑함, 메모리에 있음
	세그먼트 테이블 베이스 레지스터(STBR) - 세그먼트의 위치
	세그먼트 테이블 렝스 레지스터(STLR) - 세그먼트의 개수
	.
	Protection
	-read 권한만 줌 -> read/write/execute 권한 설정 가능


# 10장
버츄얼 메모리 관리
Goal : Page fault 최소화

Demand paging
	요청이 있을 때 페이징을 함 -> demand가 있는 페이지만 메모리에 올림
	C.A, Segmentation보단 페이징을 주로 사용
	요청이 있을 때만 페이징이 이루어지기 때문에 게으른 swapper라고 함
	Swapper vs pager
	- swap은 전체 프로세스를 다룸
	- paging은 개별 페이지를 다룸

==Valid & invalid bits==
	메모리에 있냐, 없냐를 나타내는 비트 => 페이지 테이블에 있음
	v = 메모리에 있음
	i = 메모리에 없음, illegal한 프로세스
	처음에는 무조건 invalid 였다가,demand가 되면 valid

Page-fault trap
	페이지가 처리되는 과정
	요청한 페이지가 메모리에 없을 때 발생 -> 처음엔 무조건 발생(디맨드 페이징이기 때문)
	1. valid/invalid 비트 체크
		invalid면 page fault
		paging in 해야 함
	2. 비어있는 프레임 확보
	3. 비어있는 프레임에 swapping in
	4. 페이지 테이블 업데이트 -> 프레임 넘버 입력, valid로 바꿈
	5. 재실행

Pure paging
	실제 메모리 access 패턴 -> "locality of reference" 특정 시점에 특정 페이지만 참조

디맨드 페이징의 성능
	page fault rate :0 <= p <= 1.0
	Effective Access Time(EAT)
	EAT = (1 - p) x memory access + p x (fault overhead)
	fault 확률이 조금이라도 있으면, 성능 저하가 심함 -> page fault 확률을 최소화 해야 함

Page replacement
	Paging in을 해야하는데, 비어있는 frame이 없는 경우 -> victim을 골라 교체
	빅팀 선택이 핵심
	.
	![[Pasted image 20250530005758.png]]
	disk I/O가 2번 일어남
	프레임이 메모리에 올라오고 수정이 안됐다면, victim으로 지정됐을 때, swap out을 안해도 됨 -> disk I/O 1번만 발생
	-> modified bit로 수정 여부 표시

FIFO
	가장 먼저 올라온 페이지를 victim으로
	reference string - 접근하는 순서를 나타내는 string
	reference string: 1, 2, 3, 4->page fault 발생, 1, 2, 5, 1, 2, 3, 4, 5
	Belady's Anomaly: 프레임 개수가 많아져도 fault가 많이 발생할 수 있음

Optimal 알고리즘
	가장 오랫동안 안쓰일 페이지 교체 -> 미래의 reference string 확인
	-> 미래의 reference를 모르니 과거를 봄
	LRU(least recently used) 알고리즘 사용
	->가장 오래 전에 쓴걸 victim
	.
	LRU의 구현
	1. 카운터 구현
		언제 프로그램이 메모리에 접근했는지 시간을 기록 -> 구현 편하지만 탐색 필요
	2. 스택 구현
		LRU의 위치를 고정 -> 먼저 들어온게 LRU가 되도록 리스트 유지
		![[Pasted image 20250530013612.png]]
	LRU-approximation 알고리즘(LRU비슷한)
		레퍼런스 비트 활용 -> 해당 프레임이 참조되었는지 확인
		additional-reference-bits 알고리즘 -> 레퍼런스가 프레임 별로 8개 -> 시간 단위를 쪼개놓음
		![[Pasted image 20250530013725.png]]
	second-chance 알고리즘 -> 1개의 레퍼런스 비트만 필요
		레퍼런스 비트가 1인 페이지에 한 번 기회를 더 줌
		clock = second bit
		- clock hand (pointer)
		- if (R. B == 1)
			second chance R.B = 0
		else if (R. B == 0)
			victim
	Enhanced Second chance 알고리즘
		![[Pasted image 20250530014849.png]]
	Counter-Based 알고리즘
		- LFU 알고리즘 -> 접근한 횟수 가장 낮은게 victim
		- MFU 알고리즘 -> 접근한 횟수 가장 큰게 victim

Thrashing
	프로세스가 페이지를 swapping in, out하느라 바쁨
	교체 -> 무엇을 교체할 것인가?
	이유: memory storage 부족
	->Replacement
		swap in, out (Disk I/O)
		Busy in swapping - 프로세스 증가, 효율 감소
	메모리가 부족한 것인데, CPU 성능 저하가 원인이라고 착각할 수 있음
	<Demand <= 제공되는 frame의 수> 되면 thrashing 감소
	thrashing을 줄이기 위해 프로세스가 필요한 만큼의 프레임을 제공해야 함
	Locality model로 해결
	.
	Locality model
		프로세스는 하나의 locality 단위로 변경
		->하나의 locality만 잘 파악하면 됨
		모든 time의 메모리 사이즈 대신 locality만 고려
		thrashing이 일어나는 이유?
		-locality의 총 합이 전체 메모리 사이즈보다 크기 때문

Working Set Model
	Thrashing이 발생됐는지 확인
	Demand <= 제공되는 프레임의 수
	=> D = 합 WSS i(모든 프로세스 워킹 셋의 합) <= m(피지컬 메모리 사이즈)
	만약 D가 m보다 크면? => Thrashing
	->메모리를 늘리거나 윈도우 사이즈 줄여서 해결
	![[Pasted image 20250530020132.png]]

Allocating Kernel Memory
	Buddy System
		커널의 특정 시스템에서 연속적인 여러개의 페이지들이 Buddy allocator(연속적인 페이지를 할당해 줌)에게 요청
		==-메모리를 2의 거듭제곱 크기(예: 4KB, 8KB, 16KB)로 나누어 관리
		-2의 지수 단위로 할당 - 2^0, 2^1, 2^2....==
		14page => 2^4 contiguous page 줌
		-splitting, coalescing(병합) 연산 수행
		![[Pasted image 20250530022239.png]]
		![[Pasted image 20250530022608.png]]
		- 16KB요청이 들어오면, 16KB블록을 할당
		- 16KB 블록이 없으면 32KB 블록을 16 + 16KB로 나누고 한쪽을 할당
		- 해제 시 버디 블록을 다시 합침 16 2개 -> 32KB
	Slab Allocator
		커널 내에서 object(PCB, 세마포어...)를 생성/삭제 할 때 발생
		- 해당 오브젝트의 캐시에서 slab주소(공간)을 가져다 씀
		- internal fragmentation으로 인한 메모리 낭비 최소
		- 이미 부팅할 때 슬랩 공간을 할당해놓음
		- slab은 연속적인 페이지(공간)임
		- 캐시는 한개 이상의 slab으로 구성됨
		- 캐시가 비면 free, 차면 used
		- 캐시가 꽉차면, 다음 오브젝트는 빈 슬랩에 할당함
		장점
		-no fragmentation, 빠른 메모리 요청 만족
		![[Pasted image 20250530023024.png]]
		![[Pasted image 20250530023123.png]]


# 13~15장
파일 시스템

파일 컨셉 - 파일 속성
	파일: 관련 정보의 '이름이 있는 컬렉션' -> 디스크에 저장
	파일이 아니면 데이터 저장이 안됨
	- 파일시스템은 파일 오퍼레이션을 처리해 줌(open, read, write, seek...)
	- inode를 알아야 함 -> 아이노드 확보하는 방법이 핵심
	타입 - 확장자로 확인
	위치 - 연속적이지 않은 파일 저장공간의 주소
	사이즈 - Block(4KB)발생 -> internal fragmentation 발생

파일 오퍼레이션
	create - 파일 생성
	write - 내용을 바꾸고 현재 파일 포지션 포인터를 업데이트
	read - 파일 포지션 포인터(현재 읽고 쓰는 위치)로부터 데이터를 읽음
	delete
	truncate - 컨텐츠는 지우되, 파일은 유지
	(f)seek - 파일 포지션 포인터를 바꿔줌

open file
	inode를 open file table에 올려놓음 (inode를 찾아 메모리에 keep)
	File pointer: 마지막으로 read/write한 위치의 포인터, 
	File-open count: 파일을 연 프로세스의 수 - 파일은 여러개의 프로세스에서 오픈 가능(파일 포지션 포인터는 다름), inode 안에 있음
	Access rights: 프로세스당 접근 모드 정보

파일 시스템 구현 - 디스크에서의 정보
	파티션 = 볼륨
	포맷: 새로운 파일 시스템을 파티션에 입히는 것 -> 데이터 사라짐
	1. Boot control block
		부팅에 필요한 컨트롤 블록, 파티션(C드라이브에)의 첫 블록을 boot control block으로 지정
	2. Volume control block = super block
		파일 시스템 자체의 정보
		1. 프리블록
		2. 아이노드의 위치
		3. 전체 블록의 수 등
		복제해서 여러개 저장함
		![[Pasted image 20250531004951.png]]
	3. Directory structure for a file system
		파일 구성을 위해 사용됨
		사용자는 디렉토리 못봄(파일 시스템은 봄)
		목적
		1. 관리의 용이성
		2. 동일 파일이름 허용x
		3. 하위 디렉토리 정보 가짐
		4. inode 찾는데 핵심적 역할(Array로 되어있음, 배열 인덱스만 알면 찾음)
	4. Per-file FCB
		파일에 관한 많은 정보를 담고있음
		유닉스에서는 inode라고 부름 (인덱스 노드)
		pointers to file data blocks - 파일의 위치 정보 가르킴 = offset

파일 시스템 구현 - 메모리 안에서의 정보
	1. In-memory mount table 마운트 된 파일 시스템을 간직하는 테이블
		mount: 파일 시스템 인식 -> 자동으로 해줌
		마운트된 볼륨 각각에 대한 정보
		파일 시스템이 마운트 되었는지 아닌지에 대한 정보
		윈도우 - 드라이브마다 이름 붙임
		유닉스 - 임의의 디렉토리에 마운트
	2. In-memory directory structure(테이블 형태)
		open때마다 look-up
		최근 접근된 디렉토리의 디렉토리 구조
	3. System-wide open-file table - inode 있음
		오픈된 파일들 각각의 inode의 복제본을 가지고있음
	4. Per-process open-file table
		시스템 와이드 오픈 테이블과 파일 오프셋, 프로텍션에 접근하는 포인터 가짐
		프로세스마다 다른 정보를 필요로 함(offset, permition 등)
	open file 테이블은 2개 필요

파일이 열렸을 때
	파일 컨트롤 블록(아이노드)을 open file table에 가져다 놓음

파일을 읽거나 쓸 때
	1. per-process open-file table을 탐색
	2. system-wide open-file table을 탐색
	![[Pasted image 20250531013053.png]]

파일을 닫을 때
	 프로세스 종료 시 자동으로 꺼짐
	 1. per-프로세스 파일 테이블 엔트리가 삭제됨
	 2. 시스템 와이드 오픈 파일 테이블 안의 open count가 줄어듦

inode의 위치
	Directory structure에 있음
	파일 이름과 inode 넘버의 매핑 정보를 담고있음
	Directory structure를 사용하여 아이노드의 번호를 찾으면, inode 테이블에서 아이노드를 읽음
	![[Pasted image 20250531015815.png]]
	루트부터 차례대로 아이노드를 찾음

VFS(Virtual File System)
	![[Pasted image 20250531020123.png]]
	포괄적 파일 시스템 오퍼레이션
	-동일한 시스템 콜 인터페이스 제공(서로 다른 파일 시스템에서도)
	Vnode(버츄얼 노드) ->아이노드의 in-memory버전, 전체 파일 시스템에서 unique함

Allocation methods
	하나의 파일을 구성하고 있는 블록들을 디스크에 저장
	Contiguous allocation
		각각의 파일을 디스크에 연속적으로 존재	
		장점: 구현 심플
		파일 시스템이 마지막 블록의 디스크 주소를 기억함
		Sequential access(동영상, mp3)에서 좋음
		Direct acces -> block i에 접근시, b+i를 구해서 즉시 접근할 수 있음(i번째 블록 접근 쉬움)
		- 공간 낭비
		- 파일의 크기가 커질 수 없다는 것이 치명적
		시작 위치와 크기를 디렉토리에 기록
	Extent-based allocation
		![[Pasted image 20250531022604.png]]
		extent: 디스크의 연속적인 블록
		extent는 파일 allocation에 할당됨
		파일은 하나 이상의 extent를 포함함
		파일 크기가 커지면 새로운 익스텐트를 추가로 할당
		충분하진 않음
	Linked allocation
		다음 블록의 위치를 이전 블록에 저장
		장점: external fragmentation X
		파일의 i번째 블록을 찾으려면 파일의 시작 부분부터 시작하여 포인터를 따라 i번째 블록을 찾을 때까지 진행함
		단점: 다이렉트 엑세스가 안됨, 포인터가 손실되면 Bad block 발생 => 신뢰성 이슈
	FAT(File Allocation Table)
		![[Pasted image 20250531024302.png]]
		variation of linked allocation
		마이크로소프트(윈도우)에서 사용
		다음 블록의 위치를 테이블에 기록 -> 0이면 빈 블록
		파일을 삭제하면 테이블을 0으로 바꾸는 것이 아닌, 'd'삭제 표시
		->다른 파일로 바뀌면 진짜 삭제
	Indexed allocation
		![[Pasted image 20250531024356.png]]
		파일이 저장되어 있는 블록의 번호를 저장
		인덱스 블록이 하나인 경우, 파일의 크기가 4MB = 1Block으로 제한됨
		파일이 생성될 때, 인덱스 블록의 모든 포인터는 nil로 설정됨
		블록이 처음으로 써지면, 주소는 인덱스 블록 엔트리에 넣어짐
		-인덱스 블록의 사이즈가 이슈
		1. Linked scheme(인덱스드 얼로케이션 아님)
			![[Pasted image 20250531024647.png]]
		2. Multilevel index
			![[Pasted image 20250531032452.png]]
			outer인덱스에서 블록의 번호 저장, 인덱스 테이블에서 실제 block 넘버 저장
			파일 용량 훨씬 늘어남
			-3번 읽어야 파일 위치 앎
		3. Combined scheme (리눅스, 유닉스)
			![[Pasted image 20250531032657.png]]
			![[Pasted image 20250531032737.png]]
			직접 인덱스(Direct Index), 간접 인덱스(Indirect Index), 다중 레벨 인덱스를 결합한 방식. 파일 크기에 따라 유연하게 인덱스를 구성.

log-structured file system
	"consistensy" 파일 시스템에선 일관성이 가장 중요
	data, file과 meta-data, meta-file이 같이 않으면 블루스크린 발생
	파일이 생성되는 케이스에서
	1. FCB(inode)할당
	2. 데이터 블록 할당
	3. 데이터 블록의 프리 카운트가 감소함
	4. 디렉토리 구조가 FCB를 나타내도록 수정
	3~4 사이에 crash(정전 등) 발생 -> 이상한 파일을 가지게 됨
	.
	각 파일 시스템의 업데이트를 트랜잭션으로 함
	- log에 모든 transaction 기록
	- 작성이 끝나면 commit -> nothing or all 반영

NFS
	네트워크 파일 시스템을 사용하여, 파일이 로컬 기기에 저장되어 있고, read/write 가능
	1. 디렉토리를 서버에 공개함
	2. 클라이언트에 해당 디렉토리를 Mount함


# Mass-Storage Structure

- HDD
- SSD
- Disk Scheduling
- RAID (Redundant Array of Independent Disks)

디스크 구조
	![[Pasted image 20250604223127.png]]
	spindle: 회전 축
	track: 판의 층
	sector: 판의 구역
	platter: 판
	cylinder: 스핀들로부터 같은 거리에 위치한 트랙들의 집합
	arm assembly: arm과 헤드로 이루어 짐, 헤드가 트랙으로부터 데이터 읽어옴
	- 각각의 arm은 동기화 되어서 움직임(같은 시간에 같은 트랙에 있음)
	- arm의 개수는 platter의 수만큼 있음
	- seek: 요청한 트랙의 섹터가 있는 곳 까지 arm이 움직임

Access time
	Seek time + Rotational delay + data transfer time
	Seek time : head가 데이터가 저장된 실린더까지 이동하는 시간
	Rotational delay : 요청한 섹터가 헤드 밑으로 오는 시간(회전시간)
	.
	Disk bandwidth
	1. effective bandwidth(MB/s) = # of bytes transferted/seek time + R.D. + T.T.
	2. sustainable bandwidth(속지 말기) = # of bytes transfered / T.T
		Format
		1. logical format -> 소위 말하는 포맷
		2. physical format

SSD characteristics
	SSD는 칩 형태라 더 튼튼함(안정적)
	더비쌈
	write횟수에 제한이 있음(내구성이 있음) - life span = f(# of bytes written) -> wear-leveling 균등하게 사용
	용량 더 적음
	더 빠름
	움직이는 부품이 없어, seek time과 rotational latency가 없음
	.
	읽기/쓰기를 페이지 단위로 함
	1. overwrite X => 쓰기 전에 지우기
	2. Erase: 블록단위, read/write: 페이지단위
	1, 2번으로 인해 ==Garbage collection== 발생
	FTL로 해결
	이유? -> SSD가 플래시 메모리의 속성이기 때문
	![[Pasted image 20250604233515.png]]
	==Garbege collection==: 쓸모없는 블록 모아서 지우기 -> Live data 옮겨야 함 -> 부담
	==FTL==: flash translation layer
	실제 로지컬한 페이지 넘버와 피지컬 페이지 넘버가 다를 수 있음 -> 매핑 테이블 필요
	![[Pasted image 20250604234839.png]]

Disk Attachment
	Host(PC) - storage 연결
	NVMe(칩) SATA(네모, 디스크모양) -> bandwidth이 NVMe가 훨씬 빠름
	1. Host - attached storage: storage가 PC에 직접 부착
	2. NAS(Network-attached storage): 저장공간을 네트워크로
	3. SAN (storage-Area-Network): storage안에서 네트워크로 저장공간끼리 통신
	NAS, SAN은 storage가 local네트워크의 PC에 있음
	![[Pasted image 20250604235453.png]]
	NAS - 네트워크 프로토콜(LAN/WAN), 클라이언트와 서버간 통신
	SAN - storage 프로토콜, 빠르고 많은 디바이스 지원(최대 126개)

Disk Scheduling
	seek time 최소화가 목적
	Reordering -> 디스크 스케줄링의 핵심
	![[Pasted image 20250605000802.png]]
	- SSTF(starvation)
	- SCAN(양방향)
	- C-SCAN(단방향)
	- LOOK(끝까지 안감)
	- C-LOOK
	FCFS는 seek time이 큼
	![[Pasted image 20250605000908.png]]
	SSTF(Shortest Seek Time First)
	seek time이 짧은 순서대로 => 현재 헤드 포지션에서 가장 가까이 있는 것부터
	Starvation 발생(새로 들어오는 요청들이 더 가까울 시)
	![[Pasted image 20250605001026.png]]
	SCAN(양방향)
	![[Pasted image 20250605001205.png]]
	C-SCAN(단방향)
	![[Pasted image 20250605001228.png]]
	Look (SCAN에서 끝까지 안가는 버전)
	![[Pasted image 20250605001301.png]]
	C-Look(Look의 단방향 ver)
	![[Pasted image 20250605001335.png]]

Disk management
	swap-space = backing store: 쫓겨난 파일이 저장됨(Swap manager가 관리)
	![[Pasted image 20250605002006.png]]
	쫓겨난 페이지를 빈공간(swap map = 0)에 넣음

RAID structure
	Redundant Array of Independent Disks
	독립적인 디스크들의 배열(중복 저장)
	주로 서버에서 용량, bandwidth(대역폭) 부족 해결하기 위해 사용
	1. Striping: 파일을 쪼개서 여러 디스크에 분산저장
	- bit-level striping 잘게 쪼개기
	- block-level striping 두껍게 쪼개기
	2. Fault - Tolerance -> 디스크는 고장 확률이 높음(디스크 많을수록 더 높아짐)
	- mirroring(replication) 중복저장 -> 디스크 용량 많이차지(2배됨)
	- Parity(xor연산) - striping과 같이쓰임
		parity 디스크는 평상시에 안쓰임 -> Distributed parity
	![[Pasted image 20250605011359.png]]
	![[Pasted image 20250605011505.png]]
	![[Pasted image 20250605011412.png]]
	distributed parity