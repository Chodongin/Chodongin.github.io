---
title: "Oracle Architecture 오라클 구조"
categories: 
  - Oracle
last_modified_at: 2019-07-16T13:00:00+09:00
toc: true
---

#  1. Oracle 구조 이미지


## 1.1 Oracle 9i 구조
![Alt text](/assets/images/9iStructure.png "Oracle 9i")

## 1.2 Oracle 10g 구조
![Alt text](/assets/images/10gStructure.png "Oracle 10g"){: width = "100%"}

## 1.3 Oracle 11g 구조
![Alt text](/assets/images/11gStructure.png "Oracle 11g")

## 1.3 Oracle 12c 구조
![Alt text](/assets/images/12cStructure.png "Oracle 12c")

***
***

#  2.Oracle 구조 파헤치기
![Alt text](/assets/images/memory.png "Oracle 12c")
## 2.0 오라클 전체 구조
> 사용자가 오라클 프로그램을 설치한 후에 실행시키면 오라클은 메모리와 디스크에 자신만의 특별한 구조를 만들게 되는데
> 이렇게 메모리와 디스크에 생성되는 구조를 오라클 용어로 Oracle Server라고 한다.
> 메모리부분에 생성되는 구조를 인스턴스(Instance)라고 부르고 디스크에 있는 여러가지 파일중 
> 데이터 파일, 컨트롤 파일, 리두 로그파일을 합쳐서 데이터베이스 라고 부른다.

## 2.1 User Process? Server Process? Lustener?

### 2.1.1 User Process
![Alt text](/assets/images/UserProcessServerProcss.png "Oracle 12c")

#### - 개념
```
1. 사용자가 오라클 어플리케이션 프로그램을 실행 시켰을 때 사용되는 프로세스다. 

2. 사용자가 오라클 서버에 접속할 때마다 User Process가 생성 된다.

3. 사용자가 실행시킨 SQL문을 Server Process에 전달 하고, 그 결과를 Server Process로 부터 받는 역할을 수행 한다.
```

#### - 상세
```
```
#### - 요약
 ```
  User Process란 사용자가 오라클 어플리케이션 프로그램(SQL*Plus, SQL Developer, 오렌지, Toad ...)을 사용하여  
  오라클 서버에 접속하게 되면 오라클 서버는 사용자마다 User Process를 생성한다. 
 ```


### 2.1.2 Server Process

#### - 개념
  ```
  1. 오라클 서버가 User Process의 SQL문을 처리하기 위해 생성하는 프로세스이다.

  2. User Process에게서 받은 SQL문 처리를 담당 한다.

  3. User Process와 오라클 서버가 서로 상호 작용을 수행하는 중간자 역할을 한다.

  4. Dedicated Server 와 Shared Server 방식이 있다.

  ```

#### - 상세
```
1. SQL 문장을 Server Process가 처리하는 과정

2. Parsing -> Execution -> Fetching

3. DDL/ DML 문장의 경우 Parsing -> Executing만 진행된다.
```

[SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)
 
#### - 요약
  Server Process란 
  

### 2.1.3 Listener

#### - 개념
리스너란 클라이언트 어플리케이션이 네트워크를 통해 접속 요청을 보냈을 때 이를 받아 서버 프로세스와 연결해주는 프로세스입니다. 리스너는 새롭게 생성된 하나의 서버 프로세스에 이 요청을 보내어 둘 사이에 세션이 수립되도록 합니다. 그 이후로는 둘이 알아서 요청과 응답을 주고 받고, 리스너는 관여하지 않습니다. 이러한 원격 접속에는 리스너가 꼭 필요합니다. 만약 오라클 서버와 클라이언트 어플리케이션이 같은 머신 안에 있다면 즉, 로컬 접속이라면 굳이 리스너를 통하지 않고 직접 접속 요청을 보낼 수 있습니다. 아래 그림은 리스너를 통한 세션 수립과 일반 로컬에서의 세션 수립을 나타낸 그림입니다.
  ```
  1. 오라클 서버가 User Process의 SQL문을 처리하기 위해 생성하는 프로세스이다.
  2. User Process에게서 받은 SQL문 처리를 담당 한다.
  3. User Process와 오라클 서버가 서로 상호 작용을 수행하는 중간자 역할을 한다.
  4. Dedicated Server 와 Shared Server 방식이 있다.
  ```

#### - 상세
```
1. SQL 문장을 Server Process가 처리하는 과정
2. Parsing -> Execution -> Fetching
3. DDL/ DML 문장의 경우 Parsing -> Executing만 진행된다.
4. Dedi
```
 [SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)
 
#### - 요약
  Server Process란 
  ㅇ

### 2.1.4 Dedicated Server Process 와 Shared Server Process

#### - 개념
  ```
  클라이언트 어플리케이션이 오라클에 접속하는 순간 해당 어플리케이션의 전용 서버 프로세스가 하나 할당 되고 1대1로 요청과 응답을 주고 받게 됩니다. (Shared server를 구성한다면 1대다 관계가 성립될 수 있습니다) 이를 세션(Session)이 수립되었다고 하고 한 세션 안에서 여러 요청과 응답이 오갈 수 있습니다. 서버 프로세스는 받은 SQL을 Parsing하고 필요 시 최적화를 수행하며, 해당 SQL을 실행하면서 필요한 블록을 읽습니다. 그 후 읽은 데이터를 정렬해서 결과 set을 만들고 이를 어플리케이션에 반환 해줍니다. 클라이언트 어플리케이션이 필요한 모든 처리를 끝내고 오라클 접속을 끝내는 순간 세션이 종료되고 서버 프로세스도 종료되게 됩니다. 

(1) 1:1 접속 방식으로 User Process가 접속할때마다 Server Process가 만들어져 접속하는 방식 이다.

(2) 장점 : 접속 속도가 빠르다.

(3) 단점 : 메모리를 많이 차지 한다. ( 비용이 많이 든다.

B. Shared Server(9i) = MTS [ Muti Thread Server(8i) ]

                    9i 이상부터                                                   8i 이하까지

   (1) n대 m방식으로 User Process가 Server Process에 접속 시 Server Process는  

        여러개 만들어져 있서 그 중 하나에 접속하는 방식 이다.

        예) 은행에서 번호표 뽑고 기다리는 방식이다.

   (2) 장점 : 메모리 공유

   (3) 단점 : 접속 속도가 Dedicated Server방식 보다 느리다.

   (4) User Process가 Dispater에 접속하면 Dispater는 사용할 수 있는 Server Process를  

        찾은 후, 다시 Dispater에 알려주는 User Process가 Server Process에 

        접속하게 된다. 
[출처] 03. [Oracle/오라클] 오라클 프로세스의 종류 (Oracle DBMS Architecture)|작성자 kdi0373

  1. Dedicated Server Process에선 사용자와 Server Process의 관계는 1:1의 관계이다

  2. Dedicated Server Process는 Unix, Linux 계열의 OS에선 기본적으로 전용 서버 프로세스 환경이다.

  3. Shared Server Process 에선 사용자와 Server Process의 관계는 1:N의 관계이다

  4. 다수의 User Process는 Dispatcher Process에 접속을 하고, Dispatcher는 사용 가능한 공유 Server Process에 Route 한다.

  5. Shared Server Processes를 사용하기 위해서는 별도의 환경설정이 필요하다.

  6. Shared Server 환경에서는 공유 서버와 전용 서버 프로세스를 동시에 사용 가능하다.
  ```

#### - 상세
```
- Note --------------------------------------------------------------------------
오라클 용어로 커넥션과 세션의 차이를 이해하는 것은 중요하다.
커넥션(connection)은 클라이언트 프로세스와 오라클 인스턴스 간의 물리적 경로일 뿐이다(예:사용자와 인스턴스 간의 네트워크 커넥션).
다른 한편으로 세션(session)은 클라이언트 프로세스가 SQL을 실행할 수 있는, 데이터베이스에 존재하는 논리적인 개체(entity)다.
다수의 독립적인 세션이 단 한 개의 커넥션과 관련을 가질 수도 있고, 커넥션으로부터 독립적으로 존재할 수도 있다.
---------------------------------------------------------------------------------
1. SQL 문장을 Server Process가 처리하는 과정
2. Parsing -> Execution -> Fetching
3. DDL/ DML 문장의 경우 Parsing -> Executing만 진행된다.
4. Dedi
```
 [SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)
 
#### - 요약
  Server Process란 
  ㅇ



인스턴스 : 메모리 구조(SGA, PGA) + 해당 인스턴스와 연관된 백그라운드 프로세스
 · SGA(System Global Area) : 인스턴스가 시작될 때 할당되는 공유 메모리 영역, 실제 작업들이 수행되는 공간
(서버 프로세스들과 백그라운드 프로세스들이 공유하는 공간)
 

 
· PGA(Program Global Area) : 서버 프로세스, 백그라운드 프로세스 자신이 사용하는 데이터와 제어 정보를 저장하기 위한 갖는 자체적인 메모리 영역. 다른 프로세스들과 공유하지 않음
1) Database Buffer Cache
- 디스크의 데이터파일로부터 메모리로 데이터 복사.
- 오라클의 주 작업공간
- 모든 user는 데이터 버퍼 캐시에 대한 액세스 공유
- `User Process`가 특정 데이터를 사용해야 할 경우 맨 처음 데이터 버퍼 캐시에서 찾음.
캐시에서 발견(캐시 적중)하지 못할(캐시 실패) 경우 디스크에서 데이터 블록을 캐시 버퍼로 복사함.
- Database Buffer Cache의 상태
 . Pinned Buffer : 다른 사용자가 사용중. 새로운 사용자는 사용할 수 없음
 . Dirty Buffer : 내용이 변경되었으나 아직 데이터 파일에 저장하지 않은 상태. 새로운 사용자는 사용할 수 없음
 . Free Buffer : 사용할 수 있는 상태
- LRU(Least Recently Used) List 방식으로 관리 
2) Redo Log Buffer
- 데이터의 변경사항을 기록해 놓은 메모리 공간(순환 버퍼) 
  (리두 로그에 대한 자세한 내용은 이전 블로그 글 참조) 

[SQL(오라클,mysql)] - 오라클] 리두 로그(Redo Log) 

[SQL(오라클,mysql)] - 오라클] 리두 로그 파일(Redo Log File) 

3) Shared Pool (공유풀) = 라이브러리 캐시 + 데이터 딕셔너리 캐시 + 서버결과 캐시

가. Library Cache
  - soft parse 때 사용
  - SQL에 대한 분석정보, 실행계획(Execution Plan) 등 저장
  - 컴파일된 PL/SQL 코드 저장

나. Data Dictionary Cache
  - 구문분석 또는 옵티마이저가 실행계획을 세울 때 사용되는 Dictionary들이 캐쉬
  - Data Dictionary : 데이터베이스의 스키마, 유저, 객체 등 데이터베이스 운영에 필요한 정보를 관리하는 별도의 시스템 객체

다. Server Result Cache
  - 11g부터 사용
  - 쿼리 결과값을 캐쉬
  - 디폴트는 사용하지않음
  - 사용하려면 설정을 바꿔주거나 쿼리문에 /*+result-cashe*/ 포함

라. Reserved Pool
  - Shared Pool에 5kb 넘는 오브젝트 적재시 사용
  - Shared_Pool_Size의 5% ~ 50%
 - v$shared_pool_reserved 뷰에서 request_failures 값이 증가하면 Reserved pool 값이 부족한 상태임

* 공유 풀 : 데이터 버퍼 캐시 말고 프로세스간 공유해야 할 데이터가 임시로 저장되는 공간.
특히 중요한건 데이터 딕셔너리에 대한 캐시가 저장되는 데이터 딕셔너리 캐시와
파싱된 SQL 정보가 저장되는 라이브러리 캐시임

* 공유풀은 LRU 방식으로 관리

* 데이터베이스 버퍼 캐시는 블록 단위로 저장되고 관리되는데 반해서 공유풀은 가변 크기

 
4) Large Pool
- 대규모 메모리 할당을 위한 메모리 공간
 . I/O 서버 프로세스
 . 오라클 백업 및 복원
 . Parallel Query 작업
 . Advanced Queuing

* 라지풀은 사용되고 나면 바로 해제(LRU 리스트 불필요)
 
5) Java Pool
- JVM 의 세션별 Java코드 및 데이터 보관
 
 
 
PGA(Program Global Area)

서버 프로세스가 데이터 및 제어 정보를 저장하기 위해 사용하는 전용(Private) 메모리 공간
서버 프로세스만 접근 가능(공유하지 X)
1) Stack Space
- SQL문장에 바인드 변수를 사용했을 때 해당 바인드 변수를 저장하는 공간
 
2) User Global Area
- 커서에 대한 런타임 정보 저장
- 세션에 대한 제어 정보 저장
- SQL 처리를 위한 공간(정렬, 해쉬 등)
 
* Shared Server 환경에서는 여러 클라이언트 유저가 서버 프로세스를 공유함. 그래서 이 경우 UGA는 SGA(쉐어드 풀 또는 라지 풀)로 이동하고 PGA는 Stack Space만 사용


#참고 링크
## 꿈꾸는 개발자, DBA http://www.gurubee.net/lecture/1081