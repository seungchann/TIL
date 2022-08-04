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
