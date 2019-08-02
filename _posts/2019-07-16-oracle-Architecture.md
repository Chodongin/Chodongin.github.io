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

## 2.1 User Process? Server Process? Lustener?

### 2.1.1 User Process
![Alt text](/assets/images/UserProcessServerProcss.png "Oracle 12c")

#### 개념
```
* 사용자가 오라클 어플리케이션 프로그램을 실행 시켰을 때 사용되는 프로세스다. 
* 사용자가 오라클 서버에 접속할 때마다 User Process가 생성 된다.
* 사용자가 실행시킨 SQL문을 Server Process에 전달 하고, 그 결과를 Server Process로 부터 받는 역할을 수행 한다.
```

#### 상세
```
```
#### 요약
```
> User Process란 사용자가 오라클 어플리케이션 프로그램(SQL*Plus, SQL Developer, 오렌지, Toad ...)을 사용하여 
> 오라클 서버에 접속하게 되면 오라클 서버는 사용자마다 User Process를 생성한다.
```

### 2.1.2 Server Process

#### 개념
* 오라클 서버가 User Process의 SQL문을 처리하기 위해 생성하는 프로세스이다.
* User Process에게서 받은 SQL문 처리를 담당 한다.
* User Process와 오라클 서버가 서로 상호 작용을 수행하는 중간자 역할을 한다.
* Dedicated Server 와 Shared Server 방식이 있다.

#### 상세
* SQL 문장을 Server Process가 처리하는 과정
* Parsing -> Execution -> Fetching
* DDL/ DML 문장의 경우 Parsing -> Executing만 진행된다.
  [SQL 처리 순서](https://chodongin.github.io/oracle/oracle-SQL%EC%B2%98%EB%A6%AC-%EC%88%9C%EC%84%9C/)
#### 요약
> Server Process란 
> 



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