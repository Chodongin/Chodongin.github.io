---
title: "오라클 전체 개요"
categories: 
  - Oracle
last_modified_at: 2019-01-01T22:00:00+09:00
toc: true
---

![시작과 종료](/assets/images/startup.png "Optional title")

# SQL 처리
## 1. 공통
1. 사용자가 어플리케이션을 통해 Oracle 서버에 **접속**을 하게 되면 사용자에게 **User Process가 생성** 된다.

2. User Process가 리스너를 통해 Oracle Server에게 Server Process를 호출하면 Oracle Server는 User Process에게 **Server Process를 할당하고** 
   **리스너가** User Process와 할당된 Server Process를 **연결(Session 수립)** 시켜주는 역할을 한다.

3. User Process에서 SQL을 실행하면 **해당 SQL과 기타정보를 Server Process에게 전달**하고 User Process는 결과를 받을 때까지 대기상태가 된다.

4. Server Process는 자신만의 메모리 공간인 **PGA영역의** Session Memory에 **사용자에 대한 정보를 담고** Bind 변수가 있을경우 Private SQL Area에 **Bind 변수를 담는다.**

5. Server Process는 받은 SQL을 SGA의 영역에 있는 Shared Pool의 **Library Cache**에서 **Syntax Check**를 한다.

6. **Semantic Check**를 위해 **Data Dictionary Cache를 조회**하게 된다. 

7. Parsing Tree를 만드는 동안 테이블이 삭제되면 안되므로 **아주 잠깐** 테이블에 **TM Lock을 수행**한다.

8. Server Process는 SQL문들 **ASCII 값**으로 전환하여 Hash 함수를 수행하고 결과 값인 **Hash Value**를 통해  
   Library Cache에있는 **같은 Hash 값**을 갖고 있는 Hash Bucket을 찾고 Hash List를 읽어 해당 Hash Bucket에서   
   원하는 값을 갖고 있는 Cursor의 위치를 찾아 **Execution Plan**이 있는지 **검색**한다. 

9. Execution Plan이 **있을경우** SQL문을 **실행** 처리한다. **없을 경우** **실행 계획을 생성**한다. 다음은 실행계획이 없을 경우 이다.

10. **Optimizer**가 Data Dictionary를 참고해서 **실행계획**을 생성 한다.

11. **Row Source Generator**로부터 **Row Source Tree**를 생성한다.

12. Library Cache에 새로운 공간을 확보하고 Optimizer로부터 받은 실행 계획을 등록한다.

13. **Execute(실행)**이란 단계는 하드 디스크의 **데이터 파일**에서 데이터가 들어 있는 블록을 찾아 **Database Buffer Cache**로 **복사**해 오는 과정을 말한다.

## 2.SELECT문

14. **Fetch** 과정을 수행한다.

## 3.UPDATE문

15. SQL 수행하기 전에 리두 로그 버퍼에 작업에 대한 로그를 기록한다. - Write Ahead Loging(Log Ahead) 기법 

16. 실제 SQL을 수행 하기 전에 해당 데이터들에 TX락이라는 로우 레벨 락을 건다.

17.  Undo Blcok을 Data Buffer Cache에 적재한다.

18. Update를 수행하고자 하는 Data Block들을 Data Buffer Cache에 적재한다.

19. 

