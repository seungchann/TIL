# Tech Interview - 운영체제  
> [Interview_Question_for_Beginner](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/master/OS#프로세스와-스레드의-차이) 와 [기술 면접 질문 모음](https://velog.io/@hygoogi/기술-면접-질문-모음) 을 참고하여 정리한 면접 대비 질문입니다.  

## Contents  
* 프로세스와 스레드의 차이  
* 멀티스레드  
  * 장점과 단점  
  * 멀티스레드 vs 멀티프로세스  
* 스케줄러  
  * 장기 스케줄러  
  * 단기 스케줄러  
  * 중기 스케줄러  
* CPU 스케줄러  
  * FCFS  
  * SJF  
  * SRTF  
  * Priority scheduling  
  * RR
* 동기와 비동기의 차이  
* 프로세스 동기화  
  * Critical Section  
  * 해결책  
    * Lock  
    * Semaphores  
    * 모니터  
* 메모리 관리 전략  
  * 메모리 관리 배경  
  * Paging  
  * Segmentation  
* 가상 메모리  
  * 배경  
  * 가상 메모리가 하는 일  
  * Demand Paging (요구 페이징)  
  * 페이지 교체 알고리즘  
* 캐시의 지역성  
* Locality  
* Caching line  

## 질문 목록  
### 프로세스  
* 프로세스와 스레드의 차이는 무엇인가요?  
* 교착상태란 무엇이며, 교착상태가 발생하기 위해서는 어떤 조건이 있어야 하나요?  
* 교착상태의 해결법은 무엇인가요?  
* 뮤텍스와 세마포어에 대해 설명해 보시오.  
* 컨텍스트 스위칭이란 무엇인가요?  
* 경쟁 상태란 무엇인가요?  
* 프로세스 혹은 스레드의 동기화란 무엇인가요?  
* 사용자 수준의 스레드와 커널 수준의 스레드의 차이는 무엇인가요?  
* CPU 스케쥴링이란 무엇인가요?  
* CPU 스케쥴링 방법에는 대표적으로 어떤 것들이 있나요?  
* 동기와 비동기, 블로킹과 넌블로킹의 차이느 무엇인가요?  

### 메모리  
* 프로세스에 할당되는 메모리의 각 영역에 대해서 설명해 주세요.  
* 메모리 구조의 순서가 어떻게 되는가? CPU에서 가까운 순으로 말해보시오.  
* 페이지와 세그멘테이션에 대해서 설명해 보시오.  
* 외부 단편화란? 내부 단편화란?  
* First Fit, Best Fit, Worst Fit에 대해서 설명해 보시오.  
* 페이지 교체 알고리즘 종류에는 어떤 것들이 있나요?  

## 프로세스와 스레드의 차이  
### 프로세스  
* 실행 중인 프로그램  
* 디스크로부터 메모리에 적재되어 CPU의 할당을 받을 수 있다.  
* 운영체제로부터 주소 공간, 파일, 메모리 등을 할당받으며, 이것들을 총칭하여 프로세스라고 한다.  
<img width="140" alt="프로세스 구조" src="https://user-images.githubusercontent.com/63276842/182827744-688d597e-6968-4597-bc37-eff0a329db46.png">  

* 텍스트 섹션    
* 프로그램 카운터    
* 프로세스 스택 -> 함수의 매개변수, 복귀 주소, 로컬 변수와 같은 임시 자료를 갖고 있음  
* 데이터 섹션 -> 전역 변수들을 수록  
* 힙 -> 프로세스 실행 중에 동적으로 할당되는 메모리  

### 프로세스 State  
<img width="676" alt="프로세스 state" src="https://user-images.githubusercontent.com/63276842/182834103-a334f067-4c1e-4c8f-b506-f51e3e6293f0.png">

* new: 프로세스가 생성됨 (The process is being created)  
* running: 명령들이 실행됨 (Instructions are being executed)  
* waiting: 프로세스가 이벤트 발생을 기다림 (The process is waiting for some event to occur)  
* ready: 프로세스가 프로세스에 할당되기를 기다림 (The process is waiting to be assigned to a processor)  
* terminated: 프로세스가 실행을 마침 (The process has finished execution)  

### 프로세스 제어 블록 (PCB)  
* PCB는 특정 프로세스에 대한 중요한 정보를 저장하고 있는 운영체제의 자료구조이다.  
* 운영체제는 프로세스를 관리하기 위해 프로세스의 생성과 동시에 **고유한 PCB** 를 생성한다.  
* 프로세스 전환이 발생하면, 진행하던 작업을 저장하고 CPU를 반환해야 한다.  
* 이 때, 작업의 진행 상황을 모두 PCB에 저장하게 된다.  
* 그리고 다시 CPU를 할당받게 되면 PCB에 저장되어있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을 수행한다.  

### PCB에 저장되는 정보  
* 프로세스 식별자 (Process ID, PID): 프로세스 식별번호  
* 프로세스 상태 : new, ready, running, waiting, terminated 
* 프로세스 카운터 : 프로세스가 다음에 실행할 명령어의 주소  
* CPU 레지스터  
* CPU 스케쥴링 정보 : 프로세스의 우선순위, 스케줄 큐에 대한 포인터 등  
* 메모리 관리 정보 : 페이지 테이블 또는 세그먼트 테이블 등과 같은 정보를 포함  
* 입출력 상태 정보 : 프로세스에 할당된 입출력 장치들과 열린 파일 목록  
* 어카운팅 정보 : 사용된 CPU 시간, 시간제한, 계정번호 등  

### 스레드 (Thread)  
* 스레드란?  
  * 프로세스의 실행 단위  
  * 한 프로세스 내에서 동작되는 여러 실행 흐름  
  * 명령들을 실행하는 CPU utilization의 basic unit이다.  
* 프로세스 내의 **주소 공간이나 자원** 을 공유할 수 있다.  
* 스레드는 **스레드 ID, 프로그램 카운터, 레지스터 집합, 그리고 스택** 으로 구성된다.  
* 같은 프로세스에 속한 다른 스레드와 **코드, 데이터 섹션, 그리고 열린 파일이나 신호와 같은 운영체제 자원** 을 공유한다.  
* 멀티 스레딩 : 하나의 프로세스를 다수의 실행 단위로 구분하여, 자원을 공유하고 자원의 생성과 관리의 중복성을 최소화하여 수행 능력을 향상 시키는 것  
* 각각의 스레드는 독립적인 작업을 수행해야 하기 떄문에 **각자의 스택과 PC 레지스터 값** 을 갖고 있다.  

### 스택을 스레드마다 독립적으로 할당하는 이유  
* 스택은 함수 호출 시 **전달되는 인자, 되돌아갈 주소값 및 함수 내에서 선언하는 변수** 등을 저장하기 위해 사용되는 메모리 공간이다.  
* 따라서 스택 메모리 공간이 독립적 -> 독립적인 함수 호출이 가능하다. -> 독립적인 실행 흐름이 추가된다.  
* 스레드의 정의에 따라 독립적인 실행 흐름을 추가하기 위한 최소 조건으로 독립된 스택을 할당한다.  

### PC Register 를 스레드마다 독립적으로 할당하는 이유  
* PC 값은 스레드의 명령어가 어디까지 수행하였는지를 나타낸다.  
* 스레드는 CPU 를 할당받았다가 스케줄러에 의해 다시 선점당한다.  
* 그렇기 때문에 명령어가 연속적으로 수행되지 못하고, 어느 부분까지 수행했는지 기억할 필요가 있다.  
* 따라서 PC 레지스터를 독립적으로 할당한다.  

## 멀티 스레드  
<img width="636" alt="Multithreaded Process" src="https://user-images.githubusercontent.com/63276842/182835280-05407253-4bb5-4cf7-967a-1ed676884626.png">  

### 멀티 스레딩의 장점  
* 프로세스를 이용하여 동시에 처리하던 일을 **스레드** 로 구현할 경우 **메모리 공간과 시스템 자원 소모** 가 줄어들게 된다.  
* 스레드 간의 통신이 필요한 경우 -> 별도의 자원을 이용하는 것이 아니라, 전역 변수의 공간 또는 동적으로 할당된 공간인 **Heap 영역** 을 이용하여 데이터를 주고받을 수 있다.  
* 프로세스 간 통신 방법에 비해 스레드 간의 통신 방법이 훨씬 간단하다.  
* 스레드의 context switch 는 프로세스 context switch 와는 달리 **캐시 메모리를 비울 필요**가 없기 때문에 더 빠르다. (공유된 자원으로 인해 캐시 적중률이 더 높다.)  
* 시스템의 throughput 이 향상되고 자원 소모가 줄어들며 자연스럽게 프로그램의 응답 시간이 단축된다.  

### 멀티 스레딩의 문제점  
* **멀티 프로세스 기반** 일 때는 프로세스 간 공유하는 자원이 없기 때문에 동일한 자원에 동시에 접근하는 일이 없었다.  
* **멀티 스레딩** 을 기반으로 프로그래밍 할 때는 이 부분을 신경써줘야 한다.  
* 서로 다른 스레드가 **데이터와 힙** 영역을 공유하기 때문에 어떤 스레드가 다른 스레드에서 사용중인 변수나 자료구조에 접근하여 엉뚱한 값을 읽어오거나 수정할 수 있다.  
* **멀티 스레딩** 환경에서는 동기화 작업이 필요하다.  
* 동기화를 통해 작업 처리 순서를 컨트롤 하고 공유 자원에 대한 접근을 컨트롤 한다.  
* 이로 인해 병목현상이 발생하여 성능이 저하될 가능성이 높다.  
* **과도한 락** 으로 인한 병목현상을 줄여야 한다.  

### 멀티 스레드 vs 멀티 프로세스  
* **멀티 스레드** 는 **멀티 프로세스** 보다 적은 메모리 공간을 차지하고, context switching이 빠른다는 장점이 있다.  
* **멀티 스레드** 에서는 오류로 인해 하나의 스레드가 종료되면 전체 스레드가 종료될 수 있다.   
* **멀티 스레드** 에서는 동기화 문제를 안고 있다.  
* **멀티 프로세스** 방식은 하나의 프로세스가 죽더라도 다른 프로세스에는 영향을 끼치지 않고 정상적으로 수행된다는 장점이 있다.  
* **멀티 프로세스** 방식은 **멀티 스레드** 방식에 비해 많은 메모리 공간과 CPU 시간을 차지한다는 단점이 있다.  
* 이 두가지는 동시에 여러 작업을 수행한다는 점에서 같지만, 적용해야 하는 시스템에 따라 적합 / 부적합이 구분된다.  
* 따라서 대상 시스템의 특징에 따라 적합한 동작 방식을 선택하고 적용해야 한다.  

## 스케줄러  
### 프로세스를 스케줄링하기 위한 Queue  
* Job Queue : 현재 시스템 내에 있는 모든 프로세스의 집합  
* Ready Queue : 현재 메모리 내에 있으면서 CPU 를 잡아서 실행되기를 기다리는 프로세스의 집합  
* Device Queue : Device I/O 작업을 대기하고 있는 프로세스의 집합  
* 각각의 Queue 에 프로세스들을 넣고 빼주는 스케줄러에도 크게 **세 가지 종류**가 존재한다.  

### 장기스케줄러 (Long-term scheduler or job scheduler)  
* 메모리는 한정되어 있는데 많은 프로세스들이 한꺼번에 메모리에 올라올 경우, 대용량 메모리 (일반적으로 디스크)에 임시로 저장된다.  
* 이 pool 에 저장되어 있는 프로세스 중 어떤 프로세스에 메모리를 할당하여 ready queue로 보낼지 결정하는 역할을 한다.  
* 메모리와 디스크 사이의 스케줄러를 담당  
* 프로세스에 memory (및 각종 리소스) 를 할당 (admit)  
* Degree of Multiprogramming (실행중인 프로세스 수) 제어  
* 프로세스의 상태 : new -> ready (in memory)  
> 메모리에 프로그램이 너무 많이 올라가도, 너무 적게 올라가도 성능이 좋지 않은 것이다. 참고로 time sharing system 에서는 장기 스케줄러가 없다. 그냥 곧바로 메모리에 올라가 ready 상태가 된다.  

### 단기스케줄러 (Short-term scheduler or CPU scheduler)  
* CPU 와 메모리 사이의 스케줄링을 담당.  
* Ready Queue 에 존재하는 프로세스 중 어떤 프로세스를 running 시킬지 결정.  
* 프로세스에 CPU 를 할당 (scheduler dispatch)  
* 프로세스의 상태 : ready -> running -> waiting -> ready  
* CPU scheduling decisions 이 다음과 같은 상태에서 일어날 수 있다.  
  * `0`. Is admitted  
  * `1`. Switches from **running to waiting state** (ex: I/O requests)    
  * `2`. Switches from **running to ready state** (ex: when an interrupt occurs)  
  * `3`. Switches from **waiting to ready state** (ex: I/O completion)  
  * `4`. Terminates  
* `1`과 `4`의 경우 **새로운 프로세스**를 **ready queue** 에서 골라야만 한다.  
* 따라서, `1`과 `4`에서의 Scheduling은 **nonpreemptive** 하다.  
  * 한번 CPU가 프로세스에 할당되었다면, 프로세스는 block 되거나 종료되기 전까지 실행된다.  
* `0`, `2`, `3`의 Scheduling는 **preemptive** 하다.  
  * 프로세스는 **CPU** 로 부터 강제로 제거된다. ex) time-slicing  
<img width="569" alt="Time-scheduling" src="https://user-images.githubusercontent.com/63276842/183124975-f3c70ecd-7e6b-4d96-a5a4-18163c57ff66.png">  

### 중기스케줄러 (Medium-term scheduler or Swapper)  
* 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫒아냄 (swapping)  
* 프로세스에게서 memory 를 deallocate  
* degree of Multiprogramming 제어  
* 현 시스템에서 메모리를 너무 많은 프로그램이 동시에 올라가는 것을 조절하는 스케줄러  
* 프로세스의 상태 : ready -> suspended  

### process state - suspended  
* Suspended (stopped) : 외부적인 이유로 프로세스의 수행이 정지된 상태로 메모리에서 내려간 상태를 의미한다.  
* 프로세스 전부 디스크로 swap out 된다.  
* Blocked 상태는 다른 I/O 작업을 기다리는 상태이기 때문에 스르로 ready state 로 돌아갈 수 있지만, 이 상태는 외부적인 이유로 suspending 되었기 때문에 스스로 돌아갈 수 없다.    

## CPU 스케줄러  
* **Ready queue** 에 있는 프로세스들이 CPU 를 할당받을지를 결정한다.  

### FCFS Scheduling : First-Come, First-Served Scheduling  
* 특징  
  * 먼저 온 고객을 먼저 서비스해주는 방식, 즉 먼저 온 순서대로 처리.  
  * 비선점형 (Non-preemptive) 스케줄링  
  * 일단 CPU 를 잡으면 CPU burst 가 완료될 때까지 CPU 를 반환하지 않는다.  
  * 할당되었던 CPU 가 반환될 때만 스케줄링이 이루어진다.  
* 문제점  
  * convoy effect : 소요시간이 긴 프로세스가 먼저 도달하여 효율성을 낮추는 현상이 발생한다.  
<img width="654" alt="FCFS" src="https://user-images.githubusercontent.com/63276842/183233205-6c5778cd-122d-49bf-8a30-5c7e6bd2934f.png">

### SJF Scheduling : Shortest-Job-First Scheduling  
* 특징  
  * 다른 프로세스가 먼저 도착했어도 CPU burst time 이 짧은 프로세스에게 선 할당  
  * 비선점형 (Non-Preemptive) 스케줄링  
* 문제점
  * starvation    
  * 효율성을 추구하는게 중요하지만 특정 프로세스가 지나치게 차별받으면 안된다.  
  * 이 스케줄링은 극단적으로 CPU 사용이 짧은 job 을 선호한다.  
  * 그래서 사용 시간이 긴 프로세스는 거의 영원히 CPU 를 할당받을 수 없다.  
<img width="607" alt="SJF" src="https://user-images.githubusercontent.com/63276842/183233457-b90dbff8-8d26-4d64-bd49-80344c349fa3.png">

### SRTF Scheduling : Shortest-Remaining-Time-First  
* 특징  
  * 새로운 프로세스가 도착할 때마다 새로운 스케줄링이 이루어진다.  
  * 선점형 (Preemptive) 스케줄링  
    * 현재 수행중인 프로세스의 남은 burst time 보다 더 짧은 CPU burst time 을 가지는 새로운 프로세스가 도착하면 CPU 를 뺏긴다.  
* 문제점  
  * starvation  
  * 새로운 프로세스가 도달할 때마다 스케줄링을 다시하기 때문에 CPU burst time (CPU 사용시간)을 측정할 수가 없다.  
<img width="692" alt="SRTF" src="https://user-images.githubusercontent.com/63276842/183233701-db2a0d09-9b20-4971-935b-dcdecea8feb2.png">

### Priority Scheduling  
* 특징  
    * 우선순위가 가장 높은 프로세스에게 CPU 를 할당하는 스케줄링이다.  
    * 우선순위랑 정수로 표현하게 되고, 작은 숫자가 우선순위가 높다.  
    * 선점형 스케줄링 (Preemptive)  방식  
      * 더 높은 우선순위의 프로세스가 도착하면, 실행중인 프로세스를 멈추고 CPU 를 선점한다.  
    * 비선점형 스케줄링 (Non-Preemptive) 방식  
      * 더 높은 우선순위의 프로세스가 도착하면 **ready Queue** 의 **Head** 에 넣는다.  
* 문제점
  * starvation  
  * 무기한 봉쇄 (Indefinite blocking)  
  * 실행 준비는 되어있으나 CPU 를 사용못하는 프로세스를 CPU 가 무기한 대기하는 상태  
* 해결책  
  * aging  
  * 아무리 우선순위가 낮은 프로세스라도 오래 기다리면 우선순위를 높여주자.  

### Round Robin  
* 특징  
  * 현대적인 CPU 스케줄링  
  * 각 프로세스는 동일한 크기의 할당 시간 (time quantum)을 갖게 된다.  
  * 할당 시간이 지나면 프로세스는 선점당하고 **ready queue** 의 제일 뒤에 가서 다시 줄을 선다.  
  * `RR` 은 CPU 사용시간이 랜덤한 프로세스들이 섞여있을 경우에 효율적  
  * `RR` 이 가능한 이유는 프로세스의 context 를 save 할 수 있기 때문이다.  
  * Time-sharing system 을 위해 설계되었다.  
  * Time quantum은 대게 10-100 milliseconds 정도이다.  
* 장점  
  * `Response time` 이 빨라진다.  
  * n 개의 프로세스가 **ready queue** 에 있고 할당시간이 q (time quantum) 인 경우 각 프로세스는 q 단위로 CPU 시간의 1/n 을 얻는다.  
  * 즉, 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.  
  * 프로세스가 기다리는 시간이 CPU 를 사용할 만큼 증가한다.  
  * 공정한 스케줄링이라고 할 수 있다.  
* 주의할 점  
  * 설정한 `time quantum` 이 너무 커지면 `FCFS` 와 같아진다.  
  * 또 너무 작아지면 스케줄링 알고리즘 목적에는 이상적이지만, 잦은 context switch 로 overhead 가 발생한다.  
  * 그렇기 때문에 적당한 `time quantum` 을 설정하는 것이 중요하다.  
<img width="692" alt="RR" src="https://user-images.githubusercontent.com/63276842/183234551-1ae0272c-cd59-4cb3-bbb6-9384e378b846.png">

### Multilevel Queue Scheduling  
### Multilevel Feedback Queue  
