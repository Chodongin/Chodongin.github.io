---
title: "Oracle Structure"
categories: 
  - Oracle
last_modified_at: 2019-07-17T13:00:00+09:00
toc: true
---


# 모르는 SQL 

1. DML
- MERGE
- EXPLAIN PLAN
- LOCK TABLE

2. DDL
- RENAME
- TRUNCATE
- AUDIT, NOAUDIT

3. TRANSACTION CONTROL LANGUAGE
- SAVEPOINT ( 부분 롤백 )
- SET TRANSACTION

4. SESSION CONTROL
- ALTER SESSION
- SET ROLE


# 데이터 동시성 및 일관성

다중 동시성이 일어났을 때 -> Lock이 걸리고 wait 상태가 되었을때
undo 세그먼트를 이용하여 트랜잭션 시작 시점의 데이터를 보여줌 
Commit -> RedorRecord 들을 Redo Log File에 저장 한 후 Commit 완료

 

# SELECT 문의 처리순서

1.	Parse 
A.	실행한 SQL 문장이 Shared Pool 에 있는 LIBRARY CACHE 에 있는지?
B.	실행한 SQL 문장이 LIBRARY CAHCE에 없으면 새로운 공간 확보
C.	Server process에서 LIBRARY CACHE에 저장한 이후에 Data Dictionary Cache에서   문법 검사, schema 검사, 권한 검사 
D.	실행 계획 생성 (Optimizer)
2.	BIND : bind variable을 이용시 
3.	EXECUTE
A.	Parse단계에서 만들어진 p코드(parse code)를 처리하는 단계
B.	어떤 데이터를 어떻게 가져올지 처리하는 단계
C.	SQL 처음 실행 시 Server Process가 데이터 파일에서 데이터를 Data Buffer Cache의 Data Block에 올린 후 Data Buffer Cache에 있는 Data Block을 가져와 PGA영역으로 읽어 들이게 된다. Data Block에는 여러 개의 행이 있는데 PGA가 여러 개의 행에서 조건에 맞는 행을 가져온다.
4.	FETCH 

Server Process가 User Process 에서 SQL문장을 받은 다음
1	Parse
1.1	Soft Parse
1.1.1	문법 검사(Syntax) : 키워드 검사라고도 하며 Oracle에서 미리 정해 놓은 키워드 부분을 검사함
1.1.2	의미 검사(Semantic) : 테이블 이름, 컬럼 이름처럼 사용자마다 다른 부분을 검사함. 해당 테이블이 있는지 Data Dictionary를 사용하게 되는데 자주 사용하는 테이블은 Dictionary Cache에 캐싱해 두어서 성능을 높인다.
1.1.3	권한 검사 : 어떤 사용사가 해당 오브젝트에 접근 할 수 있는 권한이 있는지 없는지를 확인하는 검사 
1.1.4	SQL문장이 오류가 없다면 ASCII 값으로 변경을 한 후 해당 값을 HASH 함수를 통해 HASH값으로 변경하게 된다.
1.1.5	변경된 HASH value를 Shared Pool의 Library Cache에 있는 Hash value들과 비교해서 동일한 값이 있는지 확인한다.
1.1.6	있으면 바로 Execution 단계로 진행된다. 없으면 Optimizer 단계로 간다
1.2	Hard Parse
1.2.1	Optimizer는 Data Dictionary 등을 참조하여 실행 계획을 생성하게 된다. 여러가지의 실행 계획을 만들고 실행 계획들의 비용을 계산하여 가장 효율적인 실행 계획 1가지를 선택한다.
1.2.2	그 실행 계획을 Library Cache에 등록한 후 PGA로 복사해서 가져온다
1.2.3	PGA로 복사해온 실행 계획을 가지고 실행 단계로 넘어간다.
1.3	   
