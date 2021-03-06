---
title:  "[JAVA] Thread(스레드)"
excerpt: "Java Thread"
toc: true
toc_sticky: true
header:
  teaser: "/assets/images/191230_java/thread_state.png"

categories:
  - Study
tags:
  - Java
last_modified_at: 2019-12-30T08:17:20

---

## 1. Thread

 스레드는 프로세스의 내부에서 실행되는 작업 단위, 흐름 단위를 뜻하며, OS에 의해 관리되는 작업 및 태스크이다.

 1. JVM(Java Virtual Machine)에 의해 하나의 프로세스가 발생하고 main() 안에 실행문들이 하나의 스레드이다.

 2. main() 이외의 또 다른 스레드를 만들려면 Thread 클래스를 상속하거나 Runnable 인터페이스를 구현한다. 
    * JDK에서 지원하는 java.lang.Thread 클래스를 확장하여 run() 메소드 오버라이드
    * Runnable 인터페이스는 구현할 메소드가 run() 하나 뿐인 함수형 인터페이스이다.
      extends Thread ==> implements Runnable
  
 3. 다중 스레드 작업 시에는 각 스레드 끼리 정보를 주고받을 수 있어 처리 과정의 오류를 줄일 수 있다.
    * 프로세스끼리는 정보를 주고 받을 수 없다.

## 2. Multi-Threading

 하나의 프로세스 내 여러 개의 스레드가 다수의 작업을 동시에 처리.

  * 멀티 프로그래밍 : IDLE(유휴)상태일 때 다른 프로세스를 처리.
  * 멀티 프로세싱 : 다중코어라고 생각하면 됨. 하나의 프로세스를 여러개의 CPU가 동시에 처리. (병렬로 처리)
  * 멀티 태스킹 : 여러 개의 태스크(Task, OS에서 처리하는 작업 단위)가 빠르게 처리되어 동시에 처리되는 것처럼 보이는 것.

 장점
  1. 메모리 공유로 인한 시스템 자원 소모가 줄어든다.
  2. 동시에 두가지 이상의 활동을 하는 것이 가능해진다.

 단점
  1. 서로 자원을 공유하여 소모하므로 충돌의 가능성이 존재한다.
  2. 코딩이 난해해지므로 버그발생확률이 높아진다.

## 3. Thread의 생명 주기

![스레드상태](https://yeollog.github.io/assets/images/191230_java/thread_state.png "스레드상태"){: .align-center}
출처 : <https://coding-factory.tistory.com/279>

  1. Runnable (준비상태)

  스레드가 실행되기 위한 준비단계. CPU에 할당되어 있지 않고 대기하고 있는 상태.
  start() 메소드를 호출하면 run() 메소드에 설정된 스레드가 Runnable 상태로 진입한다.

  2. Running (실행상태)
  
  CPU에 할당 받아 있는 상태. 준비상태에 있는 여러 스레드 중 우선 순위를 가진 스레드가 결정되면
  JVM이 자동으로 run() 메소드를 호출하여 실행상태로 진입한다.

  3. Dead (종료상태)
  
  실행상태에서 스레드가 모두 실행되고 난 후 완료상태.

  4. Blocked (지연상태)
  
  CPU로부터 할당을 상실한 상태. 후에 특정 메소드를 실행하여 다시 준비상태로 전환한다.
  wait() 메소드에 의해 지연상태가 된 스레드는 notify() 메소드가 호출되면 준비상태로 간다.
  sleep() 메소드에 의해 지연상태가 된 스레드는 지정된 시간이 지나면(Time-out) 준비상태로 간다.