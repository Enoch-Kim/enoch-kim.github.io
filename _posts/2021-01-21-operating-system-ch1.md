---
layout: post
title:  "Operating System Chapter 1 Introduction"
author: Enoch-Kim
categories: [ Note ]
tags: [OS]
image: assets/images/operating-system-ch1/operating-system-book.png
description: "<MODERN OPERATING SYSTEMS> Chapter1을 공부하며 진행한 노트"
featured: false
sitemap :
  changefreq : weekly
---

서울시립대학교 운영체제 + 컴퓨터구조론 수업을 복습하며 정리한 노트입니다.
저만의 내용 정리라.. 안보시는 것을 추천합니다.

#### 운영체제란 무엇인가

오늘날 컴퓨터는 다음과 같이 다양한 하드웨어로 구성된다.

- Processor
- Main Memory
- Disks (Hard Disk or SSD)
- Printers

**=> OS는 이 하드웨어 컴포넌트들과 운영프로그램 사이에 한 층의 Layer를 제공해 이들을 시스템 호출로 관리할 수 있게 한다. 즉, 복잡한 하드웨어 관리를 추상화해준다.**

#### 이중모드

운영체제는 하드웨어를 운영하기위한 시스템 서비스를 제공한다.
운영체제와 대화를 하는 것은 유저가 아니라 응용 프로그램 (GUI shell or CLI shell) 즉 프로세스다.
**프로세스가 운영체제 코드를 실행하기 위해서 CPU는 최소 유저모드와 커널모드 즉 이중모드를 지원한다.**

#### 운영체제의 역할

1. 복잡한 하드웨어 관리를 추상화한다.

2. CPU 자원, 메모리, I/O device 등의 리소스를 관리해준다.
  특히 멀티프로그래밍을 위해 시간과 공간을 분할하여 이들을 관리 해준다.

#### 운영체제 역사

(컨테이너 관련 중요한 내용은 없으니 생략)

#### 컴퓨터 하드웨어

##### 1. CPU

- CPU는 메모리 소자를 상대한다. (컨트롤러들과 소통할 때도 그들의 내부 메모리 소자에 관계된다. 결국 데이터로 통신하기 때문.)

- CPU는 User Mode, Kernel Mode의 이중모드를 지원한다.

  - Kernel 모드에서만 특권(Privileged) 명령을 실행할 수 있다.
  
  - 인터럽트 콜을 하면 CPU는 커널모드로 변환되어 운영체제 코드를 수행한다.

  - 특권 명령에는 다음 것들이 있다.

    - I/O instructions

    - Set processor status

    - Access privileged memory area

- 레지스터는 User-Visible 과 User-Invisible(SPECIAL) 두가지로 나뉜다.

  - User-Visible (Data, Address, Index, Segment Pointer, Stack Pointer)

    모든 프로그램에서 접근 가능하다. 프로그래머가 최적화 작업을 할 수 있도록 제공된다.
  
  - User-Invisible (Program Counter, Instruction Register, Program Status Word Register)

    프로세서가 운영하기위하여 사용하는 레지스터. 특권 OS 루틴을 제어하기 위해 사용한다.

- 멀티프로그래밍

  CPU는 I/O 등의 시간이 걸리는 작업을 기다리지 않고, 여러개의 일을 메모리에 적재한 후 스케쥴링하여 처리한다.

- 파이프라이닝

  CPU는 크게 Fetch -> Decode -> Execute 이 3가지 동작을 반복한다.
  중요한 것은 이들에 사용되는 레지스터가 서로 다르고 한 명령어를 Decode하는 동안 다른 레지스터는 놀고 있다는 것. 그렇기 때문에 Decode 중에 다른 명령어를 미리 Fetch가 가능하다. 즉, 여러 명령어를 동시에 실행한다.
  분기등으로 파이프라이닝 하던 것이 날아가는 Hazard가 존재한다. 최신 프로세서는 이를 예측하여 피하는 기능을 제공한다.

- 슈퍼스칼라

  위의 파이프라이닝이 순차적으로 파이프라이닝하는 것이었다면 Holding Buffer를 두고, 독립성을 가진 명령어들을 순서와 상관없이 처리하는 기술. 또한 파이프라이닝을 여러개 두어 파이프라이닝보다 뛰어난 성능을 가진다. (실제 이를 경험할 수 있을지 모르겠다..)

- Interrupt

  CPU는 메모리 혹은 컨트롤러 등 다른 하드웨어와 소통하기위해 Interrupt를 사용한다. (Interrupt Driven 방식)
  자신의 일을 하고 있다가 Interrupt가 들어오면 Interrupt Service Routine으로 넘어가 처리하고 다시 돌아온다. 이를 통해 불필요한 대기를 없에고 멀티 프로그래밍도 효율적으로 할 수 있다.

- Cache

  CPU에는 Cache를 두어 메모리와 CPU 사이의 병목을 완화한다. 이는 메모리의 TLB와 역할이 유사하다.
  결국 메모리 소자 사이에 속도차를 완화해주는 역할을 한다. L1 L2 L3 캐시가 존재하는데,
  이는 CPU의 레지스터와 메모리의 속도차를 단계적으로 완화해주는 역할을 한다.
  캐시 설계에서는 다음 점들을 고려해야한다.

  - Cache Size & Block Size

    캐시는 메모리에서 데이터를 블럭단위로 가져온다. 그러므로 어느정도 블럭크기를 설정하냐에 따라 성능이 좌우된다.

  - Mapping

    - Fully Associative Mapping : 각각에 비교회로 설치 -> 가장 빠르나 비싸다..

    - Direct Mapping : 끝 두자리를 Index로 하여 하나의 비교회로로 검색 -> 싸나 매우 느리다.

    - Set Associative Mapping : 두가지 맵핑 방법을 혼합한 방식 -> 적당히 빠르고 적당히 비싸다.

  - Replacement Policy
  
    Locality of Reference(참조 지역성)에 연관. 시간 지역성과 공간 지역성이 있기 때문에 어떤 교체 정책을
    사용하는 지에 따라 캐시 성능이 좌우된다.

  - Write Policy

##### 2. Disk

  기계적인 장치로 작동하여 충격에 약함 -> 백업 방식이 중요.
  디스크를 읽는 속도는 헤드가 위치를 찾는 시간이 병목 지점이기 때문에 이를 최적화하며 File System이 설계됨.
  즉, File System은 디스크의 발전과 연관있다.

##### 3. BUS

버스는 시스템, 데이터, 주소 버스가 존재.
버스는 하나가 아니라 CPU와 메모리 사이에 그들 만의 버스가 존재하고, 이와 같이실제 필요한 곳에 추가로 있다.
버스간의 속도가 다를 수 있는데, 이를 중재하기 위해 Bridge가 존재한다.

##### 4. MEMORY

메모리 주소와 I/O 주소로 구분된다. I/O 주소의 경우 포트라고 부른다.
가상 메모리 개념이 매우 중요. (내용이 너무 많아 나중에 따로..)

##### 5. I/O Devices

Device에는 Device 컨트롤러가 존재하고 이 컨트롤러가 인터럽트를 통해 CPU에게 요청한다.
Interrupt Controller 칩이 동시에 오는 인터럽트를 관리하여 병목을 방지한다.

#### Operating System Concepts

##### 1. Process

- 프로세스는 실행의 주체이다. 즉, 프로그램의 인스턴스를 생성하여 자원 할당 및 구동을 하는 주체이다.

- 프로세스는 부모 자식 관계가 있다.

##### 2. File System

- Windows는 여러 파일 시스템 트리가 존재하나 Unix 계통은 오직 하나의 파일 시스템 트리를 가진다.
  새로운 파일 시스템의 경우 하위 시스템에 마운트하여 사용한다.

- **Unix에선 모든 것을 파일로 본다.** 디바이스도 파일이고 모든게 파일이다.
  그리고 **아이노드가 곧 파일**이다.

- 이후 것은 나중에 다시 다룸

##### 3. System Call

- 운영체제는 시스템 콜을 통해 동작한다. 프로세스가 시스템 콜을 하면 커널모드로 변환되어 운영체제 코드를 수행.

##### 4. Others

(후에 자세히 공부하니 생략..)

#### Operating System 발전 과정

1. Monolithic Systems

    방대한 운영체제 코드를 한 곳에 모아두고 사용함 -> 의존성이 높아 유지보수가 힘들어 버려짐

2. Layered Systems

    OSI 7계층 마냥 계층화하여 운영하도록 설계 -> 계층화로 구분하는게 불가능에 가까워 버려짐

3. MicroKernels

    카네기 대학에서 제안하고 구현한 모델. 스티브 잡스가 넥스트에서 마크라고하는 운영체제에 사용했음.
    마이크로 커널을 두고, 나머지 기능을 서버 프로세스 형태로 구동한다.
    의존성이 매우 적어지나 문제는 메세지를 통해 주고받아 성능이 떨어진다.

4. Client Server Model

    마이크로 커널 모델을 네트워크로 확장하여 구현한 분산 운영체제의 일반적인 형테.
    네트워크를 통해서 전달한다. 느려서 안씀..

5. Virtual Machine

    - 소프트웨어적으로 PC를 애뮬레이트 한다.

      애뮬레이트 된 PC에서 다시 운영체제를 설치하고 동작시킨다.
      여러 운영체제를 지원하기 위해서 서버 하드웨어에 여러 가상 머신을 설치하여 각 운영체제를 올리고 실행시킨다.
      AWS EC2와 Google Computing Engine이 이에 해당.

    - 가상 머신의 어려운 점은 CPU 모드 변경이다.

      - Native or Bare Metal Type

        처음에는 하드웨어 위에 하이퍼바이저를 설치 후, 그 위에 가상 머신들을 올려 하이퍼바이저가 커널 모드를 실행하고 os들이 유저모드를 실행하게 했다. 문제는 이렇게 할 시 유저모드인 윈도우즈나 리눅스에서 입출력 시 인터럽트를 걸어 하이퍼바이저에게 넘겨 하이퍼바이저가 수행하도록 한다. 성능면에서 Hosted Type 보다 뛰어나나 설치 및 구성이 불편하다는 단점이 있다.

      - Hosted Type

        Host OS 위에 하이퍼바이저를 설치한 후, 그 위에 Guest OS를 설치하는 방식. 기존의 컴퓨터 환경을 그대로 사용하기 때문에 설치 및 구성이 편리. 그러나 Layer가 추가되는 만큼 성능이 떨어진다. 우리가 흔히 쓰는 가상 머신이 이에 해당한다.

6. Container

    이 책에 다뤄지지 않은 컨테이너 방식. (~~제발 최소한 Docker 사용합시다.~~)
    후에 Docker를 블로그에 작성하며 다루겠다.

Introduction이라 그런지 얉고 넓게 다뤄져있다.
