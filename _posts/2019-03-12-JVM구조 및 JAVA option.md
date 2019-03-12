---
layout: post

title: "JVM 메모리 구조와 JAVA Option값 정리"

tags:

- JVM
- Java
- tip
- bioinfomatics

category:

- JVM
- Java
- tip
- bioinfomatics

---
* toc
{:toc}

bioinfomatic를 하다보면 Java로 프로그램을 돌리는 경우가 종종 있다.

그때 자주 발생했던 메모리 문제 한번 정리해보았다.



**WAS(Web Application server)**상에서 JAVA 기반 어플리케이션을 구동시키다 보면 **OOM(Out Of Memory)** 관련 에러를 심심치 않게 볼 수 있다. 그래서 메모리 최적화 까지는 아니더라도,메모리 관련 옵션이 어떤 의미인지 정리해두고 조금이나마 메모리 사용 구조에 대한 이해를 하고자 한다.

  ![1552371953610](https://user-images.githubusercontent.com/23732754/54179175-5d93ba80-44db-11e9-97a9-45337f145aca.png)

​     

**JVM** **메모리 구조**

**· Heap = Edn + Survivor + Old**

**· Non-Heap = Perm**

​     

메모리는 우선, **Heap**과 **Non-Heap** 으로 나뉜다. 

​     

GC(Garbage Collecter)와 Heap영역의 기본 동작원리 : 

GC는 Eden과 ss1을 클리어 (살아있는 녀석은 ss2로), 다음 GC는 Eden과 ss2를 클리어 (살아있는 녀석은 ss1으로) 시키는 방법으로 동작

​     

Heap영역 : new 연산자로 생성된 객체와 배열을 저장하는 영역으로 GC 대상이 되는 영역

Eden : new 키워드를 통해서 객체가 처음 생성되는 공간

Survivor : GC가 수행될 때 살아있는 객체는 survivor영역으로 이동된다. (임시피난소)

Old : survivor에서 일정시간 참조되는 객체들이 이동되는 공간

​     

Non-Heap영역 : 스택, 클래스 area, method area 등의 heap영역을 제외한 녀석들

Permanent : Class 메타정보, Method 메타정보, Static Object, 상수화된 String Object, 

​            Class관련 배열 메타정보, JVM내부 객체와 JIT 컴파일러 최적화 정보 등

​     

​     

**JVM Option** **정리**

| **-Xms**                           | 초기 Heap Size (init, default 64m)                           |
| ---------------------------------- | ------------------------------------------------------------ |
| **-Xmx**                           | 최대 Heap Size (Max,  default 256m)                          |
| **-XX:PermSize**                   | 초기 PermSize                                                |
| **-XX:MaxPermSize**                | 최대 PermSize                                                |
| **-XX:NewSize**                    | 최소 new size (객체가 생성되어 저장되는 초기공간   Size-Eden+Survivor 영역) |
| **-XX:MaxNewSize**                 | 최대 new Size                                                |
| **-XX:SurvivorRatio**              | New/Survivor영역 비율   (n으로 지정시 Eden : Survivor = 1:n) |
| **-XX:NewRatio**                   | Young Gen과 Old Gen의 비율   (n으로 지정시 Young : Old = 1:n) |
| **-XX:+DisableExplicitGC**         | System.gc() 콜을 무시                                        |
| **-XX:+UseConcMarkWeepGC**         | 표준 gc가 아니나 Perm Gen영역도   gc하는 Concurrent Collertor를 사용 |
| **-XX:+CMSPermGenSweepingEnabled** | Perm gen영역도 GC의 대상이 되도록 지정                       |
| **-XX:+CMSClassUnloadingEnabled**  | 클래스 데이터도 GC의 대상이 되도록 지정                      |

​     

 **Xms, Xmx**를 동일하게 세팅하는 이유

​     

Xms로 init 메모리를 잡고, committed 도달할 때 까지 Used용량이 점차 증가하는데, committed에 도달 시 메모리 추가 할당 시 시스템 부하발생 (WAS가 몇 ms 멈춤)

메모리 용량은 init <used <committed <max 

​     

init와 max사이에서 used 메모리가 committed까지 사용하게 되면, 신규 메모리 공간을 요구하는데 이 때 약 1초가량 jvm이 메모리 할당 중 멈춰버리는 경우가 있다. 그래서 Xms와 Xmx를 동일하게 주고 메모리를 확보한 상태에서 jvm을 기동시키곤 한다. 
 
 
 
