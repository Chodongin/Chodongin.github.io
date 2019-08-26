---
title: "Oracle Architecture 오라클 구조"
categories: 
  - Oracle
tags:
  - oracleStructure
last_modified_at: 2019-01-01T23:00:00+09:00
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
```
 사용자가 오라클 프로그램을 설치한 후에 실행시키면 오라클은  

 메모리와 디스크에 자신만의 특별한 구조를 만들게 되는데  

 이렇게 메모리와 디스크에 생성되는 구조를 
 
 오라클 용어로 Oracle Server라고 한다.  

 메모리부분에 생성되는 구조를 인스턴스(Instance)라고
 
 부르고 디스크에 있는 여러가지 파일중   

 데이터 파일, 컨트롤 파일, 리두 로그파일을 합쳐서 데이터베이스 라고 부른다.  

 정리 하면 Oracle Server는 크게 Instance와 Database로 나뉜다.
```

## 2.1 User Process? Server Process? Listener?

### 2.1.1 User Process
![Alt text](/assets/images/UserProcessServerProcss.png "Oracle 12c")


#### - 개념
```
1. 사용자가 오라클 어플리케이션 프로그램을 실행 시켰을 때 사용되는 프로세스다. 

2. 사용자가 실행시킨 SQL문을 Server Process에 전달 하고,
   그 결과를 Server Process로 부터 받는 역할을 수행 한다.
```

#### - 상세
```
```
#### - 요약
```
 User Process란 사용자가
 
 오라클 어플리케이션 프로그램(SQL*Plus, SQL Developer, 오렌지, Toad ...)을
 
 사용하여 오라클 서버에 접속할 수 있게 해주는 프로세스다.
```

### 2.1.2 Server Process

#### - 개념
  ```
  1. 오라클 서버가 User Process의 SQL문을 처리하기 위해 생성하는 프로세스이다.

  2. User Process에게서 받은 SQL문 처리를 담당 한다.

  3. User Process와 오라클 서버가 서로 상호 작용을 수행하는 중간자 역할을 한다.

  4. Dedicated Server 와 Shared Server 방식이 있다.

  5. SQL문과 기타 정보를 저장 하기 위해 자신만의 메모리 공간인 PGA를 이용한다.
  ```

#### - 상세
```
1. SQL 문장을 Server Process가 처리하는 과정

2. Parsing -> Execution -> Fetching

3. DDL/ DML 문장의 경우 Parsing -> Executing만 진행된다.

4. SGA 메모리 영역 생성은 최초의 Oracle Server Process가 요청한다. 이후에는 OS Kernel이 관리한다. 

5. 자신에게 작업을 요청한 User Process의 정보를 Session Memory 부분에 저장을 한 후 해당 SQL을 Parse 작업을 시작한다.
```

[SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)
 
#### - 요약
  ```
  추후 예정
  ```
  

### 2.1.3 Listener

#### - 개념
```
  1. 네트워크를 이용하여 클라이언트 어플리케이션에서 Server Process와 연결해주는 네트워크 관리자다.
  
  2. 새롭게 생성된 하나의 Server Process에 이 요청을 보내어 둘 사이에 세션이 수립되도록 합니다.
  
  3. 세션이 수립되고 둘이 알아서 요청과 응답을 주고 받고, 리스너는 관여하지 않는다.
```

#### - 상세
```
1. 추후 예정
```
#### - 요약
```
 추후 예정
```

### 2.1.4 Dedicated Server Process 와 Shared Server Process

#### - 개념
  ```
  1. User Process에 Server Process가 할당 되는 것을 세션이 수립 되었다고 한다.

  2. Dedicated Server Process
    2.1  1:1 접속 방식으로 User Process가 접속할때마다 Server Process가 만들어져 접속하는 방식 이다.
    
    2.2  접속 속도와 처리 속도가 빠르지만 메모리를 많이 차지한다.

    2.2. Dedicated Server Process는 Unix, Linux 계열의 OS에선 기본적으로 전용 서버 프로세스 환경이다.

  3. Shared Server Process
    3.1  1:N의 접속 방식으로 다수의 User Process는 Dispatcher Process에 접속을 하고, Dispatcher는 사용 가능한 공유 Server Process에 Route 한다.

    3.2  메모리 영역을 공유 하지만 접속 속도와 처리 속도가 Dedicated Server 방식 보다 느리다.

    3.3. Shared Server Processes를 사용하기 위해서는 별도의 환경설정이 필요하다.

    3.4. Shared Server 환경에서는 공유 서버와 전용 서버 프로세스를 동시에 사용 가능하다.
  ```

#### - 상세
```
  추후 예정
```
 
#### - 요약
```
  추후 예정
```
***

## 2.2 SGA와 PGA 
오라클이 사용하는 메모리는 크게 두가지가 존재한다.

| 종류 | 내용 |
|:---:|:---|
| **SGA(System Global Area)** | 모든 사용자가 공유 가능하여 사용 |  
| **PGA(Program Global Area)** | 사용자마다 공유하지 않고 개별적으로 사용 |  
  
### 2.2.1 SGA( System Global Area )
![Alt text](/assets/images/sga.png "Oracle 12c")
#### - 개념
```
  1. 인스턴스가 시작될 때 할당되는 공유 메모리 영역

  2. 실제 작업들이 수행되는 공간(서버 프로세스들과 백그라운드 프로세스들이 공유하는 공간)

  3. 크게 Shared Pool, Data Buffer Cache, Redo Log Buffer, Large Pool, Java Pool, Streams Pool 로 나뉜다.
```

| 항목 | 목적 |
|:---:|:---|
| **Shared Pool** | SQL의 빠른 파싱(Parsing) , 공유 Data를 저장하기 위해 마련된 메모리 공간. |  
| **Data Buffer Cache** | 데이터 블록의 빠른 엑세스(재사용 포함) |  
| **Redo Log Buffer** | 변경 사항을 로그로 기록하여 장애 발생시 복구 |  
| **Java pool** | 오라클 JVM에 접속해 있는 모든 세션에서 사용하는 자바코드를 위한 메모리 영역이다. |  
| **Large Pool** | MTS(MultiThreaded Server)사용시 UGA(User Global Area)영역을 large pool에 할당하며 백업 및 복구작업과 병렬쿼리 수행시 메세지 버퍼를 위해 사용된다. |  
| **Streams Pool** | 오라클 스트림(데이터 동기화) 기능 사용시에 필요한 데이터 전송 버퍼를 위해 사용된다. |

#### - 상세

> SGA의 관리

``` js
SGA의 변경 
SQL> ALTER SYSTEM SET parameter_name = 변경값;

파라메터 파일을 spfile을 쓸경우 SGA_MAX_SIZE 파라메터에서 정한 값 이하까지 동적을 변경 할 수 있다. 
```

``` js
SGA 확인
SQL> SELECT *FROM V$SGA;
SQL> SELECT *FROM V$SGASTAT;
SQL> SELECT *FROM V$SGA_DYNAMIC_COMPONENTS;
SQL> show parameter 명령어
SQL> show sga
```
  
 
#### - 요약
```
  추후 예정
```
### 2.2.3 Shared Pool
#### - 개념
```
1. SQL을 수행하는 과정에서 파싱의 역할을 수행한다.
  *파싱 : SQL을 수행 전 수행할 수 있는 SQL인지 검증하고 분석하는 단계
```
#### 구성
```
1. 라이브러리 캐시  
SQL실행에 관련된 모든 객체에 대한 정보 관리  
서버 프로세스가 SQL을 작업할 때 사용하는 작업 공간  
Shared SQL Area : SQL문장, 파스트리, 실행계획 저장  
PL/SQL Procedure : PL/SQL문장, 컴파일된 프로그램 및 프로시저 저장  
Control Structures (Lock, Libray Cache Handles)  
2. 데이터 딕셔너리 캐시(Row Cache)   
테이블과 뷰에 대한 정보,구조,사용자등에 대한 시스템 메타 정보 관리  
3. 예약영역(Reserved Area)   
BIG OBJECT(SHARED_POOL_RESERVED_MIN_ALLOC에 설정된 크기 이상)를 위한 여분의 메모리 ( 기본설정 : 4400bytes)  
SHARED_POOL_RESERVED_SIZE : SHARED_POOL_SIZE * 5%   
4. 시스템영역(Permanent Area)  
Process, Session, Enqueue, Transaction 등의 목록 정보  
```




| 항목 | 내용 |
|:----:|:--------------------------------|
| **소프트 파싱** | 검색 단계에서 기존에 동일한 SQL이 수행된 걸 확인하고 해당 SQL의 파싱 정보를 재사용 |  
| **하드 파싱** | 기존에 동일한 SQL이 수행되었지만 메모리가 부족하여 LRU 알고리즘에 의해 버려지거나 수행된 적이 없는 SQL로 다시 파싱을 수행 |  

[SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)

#### - 상세

 
#### - 요약

| 목적 | 목적 달성을 위한 요소 |
|:---:|:-----------------------------------|
| **파싱(SQL Parsing)을 효과적으로 수행** | - 이전에 수행된 SQL에 대해서 소프트 파싱 유도  - 하드 파싱 시 적은 자원 사용 |  

  ***
   

### 2.2.3 Database Buffer Cache
![Alt text](/assets/images/sga.png "Oracle 12c")
#### - 개념
  ```
  1. Datafile 들로부터 읽은 Data Block의 복사본을 담고 있는 SGA의 한 영역
  
  2. 인스턴스에 동시에 연결된 모든 사용자 프로세스는 Database Buffer Cache에 대한 엑세스를 공유한다.

  3. 인스턴스가 시작될 때 할당되는 공유 메모리 영역

  4. 물리적인 I/O를 최소화하기 위해 최근에 사용한 블록에 대한 정보를 저장하는 메모리의 일정 영역 

  5. LRU List와 LRUW List(Dirty List) 두가지 List로 구성 되어있다.
  ```
  
#### - 상세
![Alt text](/assets/images/buffer_cache_hashtable.png "Oracle 12c")
```
  1. 블록단위 I/O

  - 메모리 버퍼 캐시에서 버퍼 블록을 액세스 할때 블록 I/O

  - 데이터파일에 저장된 데이터 블록을 DB 버퍼 캐시로 적재할때 블록 I/O

  - 캐시에서 변경된 블록을 다시 데이터파일에 저장할 때 블록 I/O 
      single block I/O : 인덱스를 경유해 테이블 액세스시.
      multi block I/O : Full Table Scan 시 
      => 옵티마이져가 인덱스를 이용해 테이블을 액세스할 지, Full Table Scan을 할 지를
         결정하는 판단 기준은 읽어야 할 레코드 수가 아니라 블록 갯수이다. 
  
  2. 버퍼 캐시 구조
    해시테이블 구조로 관리 됨. 
    해싱(hasing)알고리즘 <= 주소록에 비유
    입수시점이 같지 않으므로 해시 버킷내에서는 정렬상태를 유지 하지 않는다.(스캔방식으로 탐색) 
    해시 체인은 Shared Pool 내에 존재, 버킷(Bucket) ==> 체인(Chain) ==> 헤더(Header)의 구조.
    
    - 해시 테이블(Hash table)은 여러개의 해시 버킷으로 구성.
    
    - 블록의 주소(Database Block Address)와 블록클래스에 대해 해시함수 적용한 결과를 이용하여 버킷을 찾아간다.

    - 해시 버킷에는 같은 해시 값을 갖는 버퍼 헤더들이 체인 형태 있다.

    - 버퍼 헤더는 버퍼에 대한 메타 정보, 버퍼 메모리 영역의 실제 버퍼에 대한 포인터 값을 가지고 있다. 
      => 찾고자 하는 데이터블록 주소를 해시값으로 변환해서 해당 해시 버킷에서 체인을 따라 스캔하다가 찾아지면 바로 읽고,
         찾지 못하면 디스크에서 읽어 해시 체인에 연결한 후 읽는다.

Hash table -> Hash bucket -> buffer header chain -> buffer header -> buffer body -> block header -> block body

  3. Database  Buffer Cache의 3가지 상태
    
    -	Pinned Buffer : A사용자가 현재 사용하고 있는 Buffer 블록, 다른 사용자는 사용할 수 없음.
    
    -	Dirty Buffer : 작업이 진행되지 않지만 A사용자가 변경 한 후 아직 데이터 파일에 변경된 내용을 저장 하지 않은 Buffer. 
                      다른 사용자는 사용할 수 없음.

    - Free Buffer : 사용가능 Buffer
   
   * 데이터 블록 주소(DBA, Data Block Address) : DB 버퍼 캐시 내에서 데이터 블록을 해싱하기 위해 사용되는 키 값
   
  5. Latch : 같은 리소스에 대한 액세스를 직렬화(Serialization)하여 리소스를 보호 하기 위한 Lock 메커니즘
     - 동시에 여러 프로세스가 넓은 범위의 인덱스나 넓은 범위의 테이블에 대해 스캔을 수행할 경우.
       즉, 비효율적인 SQL문이 실행 될 경우 latch: cache buffers chains 이벤트를 대기하게 된다. 

     * cache Buffers Chains Latch : 두개 이상의 프로세스가 같은 해시 체인으로 진입해 새로운 버퍼 블록을                                  
                                    연결/해제하는 작업이 동시에 작업할 때 발생하는 문제를 방지하기 위한 래치
```


#### - 요약
```
  추후 예정
```


(4) 캐시버퍼 LRU 체인

두 종류의 LRU(Least Recently Used) 리스트 사용

Dirty 리스트* : 캐시 내에서 변경됐지만, 아직 디스크에 기록되지 않은 Dirty버퍼 블록들을 관리.LRUW(LRU Write)리스트 라고도 함.
LRU 리스트* : 아직 Dirty 리스트로 옮겨지지 않은 나머지 버퍼블록들을 관리함. 
  4. Cache Buffer Chain
    
    - 각 Database Buffer는 Data Block Address(DBA)가 Hash Function에 의해 해시되어 Hash Table에 할당되어 관리되는데, 이를 Cache Buffer Chain List라 한다.

    - 즉 Cache Buffer Chain List란 양방향의 링크된 리스트로, 인스턴스가 시작될 때 할당되는 Hash Table로 구성된다.

    -  Hash Table 안의 Bucket은 각 Database Block Buffer들의 Header 정보를 가지며, 이 각 Buffer들은 LRU List 나 
       LRUW List(Dirty List)의 한 가지에 속한다. 

    - 각 해시 체인은 래치(Latch)에 의해 보호된다.
### LRU List와 LRUW List
![Alt text](/assets/images/dbbufferlrulist.png "Oracle 12c")
#### LRU List(Least Recently Used) 
```
  1. 최근에 읽혀진 Datafile Block 을 Buffer Cache에 보관하고, 새로운 Block이 파일에서 읽혀질 필요가 있으면 사용한지 가장 오래된 버퍼들부터 메모리에서 없어지도록 

     관리하기 위한 Buffer Cache 내의 Block List 관리 방법.
  
  2. LRU(Least Recently Used) list의 버퍼들은 Free buffer, Pinned Buffer, Dirty Buffe중 하나로 존재하게 된다.

    -메인 리스트 : 사용된 버퍼들의 리스트, Hot 영역과 Cold 영역으로 나눕니다. 
    -보조 리스트 : 미사용된 버퍼들이나, DBWR에 의해 기록된 버퍼들의 리스트(Free list)

```
#### LRUW(LRU Write) List = Dirty list
```
  1. 수정되어 디스크에 반영되어야 할 블록들의 리스트.
     
  2. LRUW에 모인 Dirty Buffer는 DBWR에 의해 디스크로 쓰여지고 나면 이 버퍼들이 차츰 다른 테이블의 내용들이 읽혀 짐에 따라
   
     LRU의 tail부분인 LRU(Lease Recent Used)로 이동되게 된다.

  3. 만약 사용자가 대량의 데이터를 질의하여 버퍼가 필요한데 빈 버퍼가 없다면 사용된 빈도가 제일 작은 블록을
  
     찾기 위해 LRU 알고리즘에 의해 LRU list의 맨 끝인 tail 부분부터 검색하기 시작 한다.

     Free로 Mark 되어 다시 사용될 수 있도록 LRU List의 끝부분에 위치하게 됩니다.
  
  4. 처음엔 버퍼가 Free 상태인데 사용자가 질의를 해서 디스크에서 테이블의 내용을 읽으면 이 블록들은

     LRU에서 가장 최근에 읽은 것 이므로 HEADER 부분(MRU, Most recently used)에 위치하게 됩니다.
      
      -메인 리스트 : 변경된 버퍼들의 리스트
      -보조 리스트 : 현재 DBWR에 의해 기록 중인 버퍼들의 리스트
```


#### 버퍼 탐색 순환
**LRU 보조 -> LRU 메인 -> LRUW 메인 -> LRUW 보조순으로 순환하며 버퍼를 탐색한다.**

1. 보조 리스트의 버퍼가 모두 사용된 경우에는,
2. 메인 리스트의 cold 영역에서 free 버퍼를 다시 찾게 됩니다. 
 
 이렇게 free buffer 를 찾다가 특정 개수만큼 찾았는데 free buffer를 못찾게 되면
scan을 멈추고 DBWR에게 Dirty Buffer를 내려쓰라고 요청을 하게 되는 것입니다.
그럼 Dirty Buffer가 Free Buffer로 바뀌게 되어 LRU List의 보조 리스트에 추가가 되는 것입니다.
인스턴스가 최초로 구동된 때는 모든 버퍼들은 LRU List 의 보조 리스트 에서 관리됩니다.


### 2.2.8 PGA ( Program Global Area )
![Alt text](/assets/images/pga.png "Oracle 12c")
**PGA DETAIL**  
![Alt text](/assets/images/pgaDetail.png "Oracle 12c")

#### - 개념
  ```
  1. PGA는 각 Process들이 개별적으로 사용하는 메모리 공간이다.

  2. Server Process의 데이터와 제어 정보를 갖고있는 메모리 영역이다.
  
  3. Server Process가 시작되었을 때 오라클이 Server Process에 생성한다.

  4. 각각의 Server Process마다 PGA가 하나씩 있고 Server Process가 독자적으로 사용하는 메모리 영역이다.
  
  5. User Process가 리스너를 통해 Server Process를 호출하면 Server Process가 PGA 공간을 할당해 SQL 작업공간을 생성한다.

  6. 주로 정렬관련 작업등이 이루어진다.

  ```

Program Global Areas (PGA)



#### - 상세
```
  1. Server Process가 사용하는 PGA는 크게 SQL Work Area, Private SQL Area, UGA로 나뉜다.
    1.1 Private SQL Area
      1.1.1 Server Process는 자신에게 작업을 요청한 User Process의 정보를 Session Memory 부분에 저장을 한 후 해당 SQL을 Parse 작업을 시작한다.
      1.1.2 만약 Bind 변수등이 있을 경우 해당 Bind 변수 값을 Private SQL Area에 보관을 하고 Query의 실행 상태정보와 Query를 수행하면서 
            임시로 정보를 저장해야 하는 경우 이 공간을 사용하게 된다.
            * Bind 변수란? 사용자가 입력한 값에 따라 달라지는 SQL WHERE문에 들어오는 조건 값 
    1.2 SQL Work Area
      1.2.1 Sort관련 작업(Sort Area)이나 Hash 관련 작업이 있을 경우 이 곳에서 작업을 수행하게 되는 공간이다.

  3. 오라클 서버에서 동작하는 모든 Process들 
     즉, Server Process뿐만 아니라 Background Process들도 전부 각각의 PGA를 가지고 각자의 용도에 맞게 사용하고 있다.
  
  4. 보통 SQL Work Area, Private SQL Area, UGA로 나뉘는 구조는 Server Process의 PGA를 보여준다.
     모든 PGA가 해당 구조를 갖고 있는 것은 아니다.
```

![Alt text](/assets/images/PGAdetail2.png "Oracle 12c")

#### - 요약
```
  추후 예정
```

### 2.2.9 PGA의 관리

#### - 개념

| 항 목 | 필수여부 | 설정값 | 내 용 |
|:---:|:--:|:----:|:------------------|
| **WORKAREA_SIZE_POLICY** | X | AUTO  (Default 값) |  MANUAL과 AUTO로 설정할 수 있으며 AUTO로 설정했을 경우에는 PGA_AGGREGATE_TARGET 파라메터를 이용하여 PGA의 크기를 설정하겠다는 뜻이다. MANUAL로 설정할 경우는 SORT_AREA_SIZE 파라메터를 이용하여 정렬 공간을 설정하게 된다. 오라클 10g부터는 기본 값이 AUTO로 설정된다. |
| **PGA_AGGREGATE_TARGET** | O | 물리  메모리의  10%~20% | 모든 세션의 PGA 크기의 합을 설정하는 파라메터이며, PGA_AGGREGATE_TARGET 파라메터에서 지정한 크기까지 오라클이 PGA를 자동으로 관리한다. |
| **PGA_AGGREGATE_LIMIT** | O | Default 사용 | 오라클 12c 부터 등장했다. PGA_AGGREFATE_TARGET 파라메터로는 실제 PGA의 사용량을 제한하지 못하며 PGA_AGGREGATE_LIMIT 파라메터로 실제 PGA의 크기를 제한할 수 있다. |
| **_SMM_MAX_SIZE** | X | Default 사용 | 하나의 서버 프로세스가 사용 가능한 최대 SQL 작업 영역의 크기를 지정하며 각각의 서버 프로세스는 해당 값을 초과 하여 SQL 작업 영역을 할당 받을 수 없다. 해당 값은 PGA_AGGREATE_TARGET 파라메터 값에 의해 자동으로 설정된다. |
| **_SMM_PX_SIZE** | X | Default 사용 | 하나의 병렬 실행에 속한 병력 SLAVE 프로세스들이 사용 가능한 최대 SQL 작업 영역의 크기를 의미하며 해당 값은 전체 총합을 의미한다. 해당 파라메터     P 또한 PGA_AGGREGATE_TIRGET 파라메터 값에 의해 자동으로 할당된다. |
| **_SMM_PX_SIZE** | X | Default 사용 | 하나의 서버 프로세스가 사용 가능한 최대 PGA 크기를 지정 하며 각각의 서버 프로세스는 해당 값을 초과하여 PGA 영역을 할당 받을 수 없다. 해당 값은 PGA_AGGREATE_TARGET 파라메터 값에 의해 자동으로 설정된다. |

#### - 상세
> 해당 파라미터 중에 가장 중요한 PGA_AGGREGATE_TARGET 파라메터에 대해서..

WORKAREA_SIZE_POLICY를 AUTO로 설정한 경우

| 항 목 | 내 용 |
|:---:|:------------------|
| **제한 값이 아니며 단순 지표 값** | PGA_AGGREGATE_TARGET 파라메터는 PGA의 총합을 제한하는 파라메터는 아니다. 해당 값에 의해 각각의 서버 프로세스에 대한 PGA의 크기를 할당하게 되며 해당 값은 목표 값이 된다. 최대 설정 값이 아니며 하나의 서버 프로세스에서 할당 되는 PGA의 크기에 대한 단순 지표를 제공한다. |
| **다른 파라메터 무시** | 해당 파라메터 값에 의해 SORT_AREA_SIZE 파라메터 및 HASH_AREA_SIZE 파라메터는 무시된다. |

> 평소 사용되는 PGA의 크기는 V$PROCESS 동적 성능 뷰에서 다음 컬럼 값을 조회하여 확인할 수  있다.

| 컬 럼 | 내 용 |
|:---:|:------------------|
| **PGA_USER_MEM** | 프로세스가 현재 사용하는 PGA의 크기 |
| **PGA_ALLOC_MEM** | 프로세스에 할당된 PGA의 크기 (사용 완료 후 시스템 메모리에 반환되지 않는 메모리 포함)
| ***PGA_MAX_MEEM** | 프로세스가 사용한 최대 PGA의 크기 |

#### - 요약




















/////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


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


#참고
[꿈꾸는 개발자, DBA](http://www.gurubee.net/lecture/1081)
[오라클 PGA DOC](https://docs.oracle.com/en/database/oracle/oracle-database/19/admin/managing-memory.html#GUID-24312D78-522D-4C08-8934-820BE18D34C7)
[DB버퍼캐시](http://wiki.gurubee.net/pages/viewpage.action?pageId=3902380)
