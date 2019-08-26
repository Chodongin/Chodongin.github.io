---
title: "SQL문 처리 순서"
categories: 
  - Oracle
last_modified_at: 2019-01-01T13:00:00+09:00
toc: true
---
**모든 과정 Server Process에 의해 이뤄진다 !**

![Alt text](/assets/images/select turn.png "Optional title")
# 1. SQL 문장 처리 과정
## 1.1 순서
### 개념

```
1. 사용자가 User Process에서 SQL문장을 수행한다. 

2. User Process가 SQL 문장을 Server Process로 전달한다.

3. Server Process는 Syntax Check( 문법 검사 ), Semantic Check(의미 검사)를 한 후 권한 검사를 하게 된다.

4. Library Cache에서 공유되어 있는 실행 계획이 있는지 검사한다.
------------------------------------Soft Parsing--------------------------------------------------

5. Library Cache에 실행계획이 있다면 바로 SQL 엔진이 SQL을 실행한다.
   Library Cache에 실행계획이 없으면 Hard Parse 단계로 넘어간다.

6. Oprimizer가 SQL문에 대한 실행계획을 생성한다.

7. Optimizer가 생성한 실행계획을 SQL 엔진이 실제 실행할 수 있는 코드(또는 프로시저 ) 형태로 포맷팅한다.
------------------------------------Hard Parsing--------------------------------------------------

8. SQL 엔진이 SQL을 실행한다.
```

| 순번 | 항목 | 내용 | 발생 주체 또는 발생 지점 |
|:-:|:-----|:-------:|:------|
|1| **Syntax Check** | 키워드 검사라고도 하며 SELECT, FROM, WHERE 같이 Oracle에서 미리 정해놓은 키워드 부분을 검사한다. | Library Cache 및 Data Dictionary Cache |
|2| **Semantic Check** | SQL문의 오브젝트가 및 컬럼의 존재 여부를 검사한다. | Data Dictionary Cache |
|3| **권한 검사** | 어떤 사용자가 해당 오브젝트에 접근 할 수 있는 권한이 있는지 없는지를 확인한다. | Data Dictionary Cache |  
|4| **검색** | Shared Pool에서 동일한 Hash 값을 갖는 실행 계획이 있는지 검사한다.  | Library Cache |
|5| **Optimization** |  SQL을 가장 빠르고 효율적으로 수행하고자 하는 실행 계획을 생성한다. | [Optimizer](https://chodongin.github.io/oracle/oracle-%EC%98%B5%ED%8B%B0%EB%A7%88%EC%9D%B4%EC%A0%80/) |
|6| **Row Source Generation** | 옵티마이저가 생성한 실행계획을 실행 가능한 코드 또는 프로시저형태로 포맷팅 하는 작업이다.  Row-Source는 레코드 집합을 반복 처리하면서 사용자가 요구한 최종 결과집합을 실제적으로 생성하는데 사용되는 제어 구조를 말한다. | Row Source Generator |

# SELECT 문의 처리순서

1.	Parse 
- A.	실행한 SQL 문장이 Shared Pool 에 있는 LIBRARY CACHE 에 있는지?
- B.	실행한 SQL 문장이 LIBRARY CAHCE에 없으면 새로운 공간 확보
- C.	Server process에서 LIBRARY CACHE에 저장한 이후에 Data Dictionary Cache에서   문법 검사, schema 검사, 권한 검사 
- D.	Data Dictionary등을 참조하여 실행 계획 생성 (Optimizer)
2.	BIND : bind variable을 이용시 
3.	EXECUTE
- 실행하게 되면 SQL 엔진은 Row Source Generator가 생성 한 Row Source Tree의 각 행 소스를 실행합니다. DML 처리에서 유일한 필수 단계입니다.
- Parse단계에서 만들어진 p코드(parse code)를 처리하는 단계
- 어떤 데이터를 어떻게 가져올지 처리하는 단계
- SQL 처음 실행 시 Server Process가 데이터 파일에서 데이터를 Data Buffer Cache의 Data Block에 올린 후 Data Buffer Cache에 있는 Data Block을 가져와 PGA영역으로 읽어 들이게 된다. Data        Block에는 여러 개의 행이 있는데 PGA가 여러 개의 행에서 조건에 맞는 행을 가져온다.
4.	Fetch 
- 메모리에 올라온 Block에는 원하는 데이터와 원하지 않는 데이터가 같이 있다. 하지만 데이터의 I/O 최소 단위는 Block이므로  
  그 Block에서 사용자가 원하는 데이터만 골라내야 한다. 그 과정을 Fetch라고 한다.
- 정렬등의 추가 작업이 있을 경우 PGA 공간 에서 Sort 작업이 이루어지며 Fetch 과정에서 완료해서 데이터를 보내주게 된다.

# Parse 
**Parsing 단계에서 SQL문장을 다른 프로세스에서도 처리할 수 있는 데이터 구조로 하나 하나 분리하는 작업이다.**
1.	Parse
- 1.1	Soft Parse
  - 1.1.1	문법 검사(Syntax) : 키워드 검사라고도 하며 Oracle에서 미리 정해 놓은 키워드 부분을 검사함
  - 1.1.2	의미 검사(Semantic) : 테이블 이름, 컬럼 이름처럼 사용자마다 다른 부분을 검사함. 해당 테이블이 있는지 Data Dictionary를 사용하게 되는데 자주 사용하는 테이블은 Dictionary Cache에      캐싱해 두어서 성능을 높인다.
  - 1.1.3	권한 검사 : 어떤 사용사가 해당 오브젝트에 접근 할 수 있는 권한이 있는지 없는지를 확인하는 검사 
  - 1.1.4	SQL문장이 오류가 없다면 ASCII 값으로 변경을 한 후 해당 값을 HASH 함수를 통해 HASH값으로 변경하게 된다.
  - 1.1.5	변경된 HASH value를 Shared Pool의 Library Cache에 있는 Hash value들과 비교해서 동일한 값이 있는지 확인한다.
  - 1.1.6	있으면 바로 Execution 단계로 진행된다. 없으면 Optimizer 단계로 간다
- 1.2	Hard Parse
  - 1.2.1 Soft Parse가 실패를 하였을 경우에만 실행 된다.
  - 1.2.2	Optimizer는 Data Dictionary 등을 참조하여 실행 계획을 생성하게 된다. 여러가지의 실행 계획을 만들고 실행 계획들의 비용을 계산하여 가장 효율적인 실행 계획 1가지를 선택한다.
  - 1.2.3	그 실행 계획을 Library Cache에 등록한 후 PGA로 복사해서 가져온다
  - 1.2.4	PGA로 복사해온 실행 계획을 가지고 실행 단계로 넘어간다.

# 검색 과정 - Library Cache에서 Hash Value를 찾는 과정
1. Server Process는 전달 받은 SQL문을 ASCII 값으로 전환하여 해쉬 함수를 수행 한다.
2. 해쉬 함수를 수행하고 결과 값인 Hash Value를 받는다.
3. 실행 계획이 있는지 검사 하기 위해 Hash Value값에 해당하는 Hash Bucket을 읽는다.  
   ** 해쉬 Bucket은 해쉬 함수를 수행하여 추출될 수 있는 모든 결과에 대해 하나씩 존재한다.  
   ** 따라서 어떠한 SQL이던 해쉬 함수를 수행하게 되면 하나의 해쉬 Bucket이 선택된다.
4. Hash Bucket에 있는 Hash value를 비교하여 실제 데이터가 저장되어 있는 공간인 공유 커서를 찾는다.
  4.1 커서(Cursor)  
  커서란 메모리에 임의의 데이터를 저장하기 위해 만드는 임시 저장 공간. 공유 커서, 세션 커서, 어플리케이션 커서가 있다.  
    4.1-1 공유 커서
      SQL 문장의 실행계획과 관련 정보를 보관하고 있다가 재활용 하다가 Hard Parse의 부담을 줄여 SQL 문장의 수행속도를 빠르게함
      부모 커서(Parent Cursor)와 자식 커서(Child Cursor)로 나뉘게 된다.
      부모 커서에는 SQL 문장 자체에 대한 값이 있다.
      자식 커서에는 Optimizer Mode 정보, 사용자 정보에 대한 값이 있다.
      동일한 SQL문을 수행해도 서로 다른 사용자가 서로 다른 계정으로 DB에 로그인 했다면 Parent Cursor는 같으나 Child Cursor가 일치하지 않아  커서 공유를 할 수 없다. 
      그래서 오라클은 '어떤 커서에 어떤 데이터가 들어 있다' 라는 정보가 있는 Hash list로 관리 한다.
      > Hash list?
      + 1. 체인 구조로 되어 있다. (Hash value값의 Hash list에는 n번째 커서에는 n번째 실제 데이터와 n+1번째 위치값이 있다.)
      + 2. Library Cache의 내용을 저장하고 있는 Hash list는 1개 밖에 없다.
      + 3. Library Cache Latch를 통해 순서를 정한다.
      + 4. Hash List에 SQL과 실행계획 이 없을 경우 Hard Parse 과정을 수행 한 후 실행계획을 받아서 Library Cache에 신규로 등록한 후 다음   단계로 넘어간다.
      + 5. 새로 SQL과 실행계획을 등록하려면 Library Cache Latch를 획득한 상태에서 Shared Pool Latch이 필요하다.

  - 2.1.2 세션 커서
  - 2.1.3 어플리케이션 커서


3. Optimizer 
  data dictionary를 참조해서 실행 계획을 생성한다.
- 3.1 Rule Based Optimizer(RBO) 
  - 3.1.1 미리 정해져 있는 규칙을 사용해서 실행 계획을 세움
  - 3.2.2 RBO 규칙
  | 순위 | 접근경로 | 
  | ---|:---:|
  | 1 | Single row by ROWID | 
  | 2 | Single row by cluster join |
  | 3 | Single row by hash cluster key with unique or primary key |
  | 4 | Single row by unique or primary key  |

 SQL문이 Soft Parse를 시도했는데 실패했을 경우 서버 프로세스가 RBO를 찾아가서 실행 계획을 요청 했다면 RBO는 해당 SQL문 테이블을 꺼내 놓고
 15번 방법부터 하나씩 대입해서 적당한 방법을 요구한다.
 15가지의 규칙 안에서만 행동해야 되기 떄문에 규칙안에서 실행 계획을 세워야 하기 때문에 문제가 발생 될 수 있다.
 RBO는 10g R2버전 이후로 지원하지 않는다.

- 3.2 Cost Based Optimizer(CBO)
  - 3.2.1 실행 계획을 세울 때 데이터 딕셔너리 정보(DBA_TABLES OR USER_TABLES)를 보고 판단을 하게 된다.
  - 3.2.2  


4. Row Source Generator

Row Source Generator는 Optimizer로부터 최적의 실행 계획을 수신하고 나머지 데이터베이스에서 사용할 수 있는 반복 실행 계획을 생성하는 소프트웨어다.

반복 계획은 SQL 엔진에 의해 실행될 때 결과 집합을 생성하는 이진 프로그램이다. 그 계획은 단계들의 조합의 형태를 취하고 있다. 각 단계는 행 집합을 반환한다. 다음 단계에서는 이 세트의 행을 사용하거나 마지막 단계는 SQL 문을 실행하는 응용 프로그램으로 행을 반환한다.

행 소스는 행을 반복적으로 처리할 수 있는 제어 구조와 함께 실행 계획의 단계별로 반환되는 행 집합이다. 행 소스는 조인 또는 그룹화 작업의 테이블, 보기 또는 결과가 될 수 있다.

행 소스 생성기는 행 소스 트리를 생성하며, 이 트리는 행 소스 집합이다. 행 원본 트리는 다음 정보를 보여준다.

SQL문에서 참조하는 테이블 순서

SQL문에 언급된 각 테이블에 대한 액세스 방법

SQL문의 Join 작업의 영향을 받는 테이블에 대한 Join 방법

필터, 정렬 또는 집계와 같은 데이터 작업한다.

# 참고
[오라클 DOC](https://docs.oracle.com/database/121/TGSQL/tgsql_sqlproc.htm#TGSQL175)