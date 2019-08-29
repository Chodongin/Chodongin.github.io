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
1. 사용자가 어플리케이션을 통해 Oracle 서버에 접속을 하게 되면 사용자에게 User Process가 생성 된다.

2. User Process가 리스너를 통해 Oracle Server에게 Server Process를 호출하면 Oracle Server는 User Process에게 Server Process를 할당하고 
   리스너가 User Process와 할당된 Server Process를 연결(Session 수립) 시켜주는 역할을 한다.

3. User Process에서 SQL을 실행하면 해당 SQL과 기타정보를 Server Process에게 전달하고 User Process는 결과를 받을 때까지 대기상태가 된다.

4. Server Process는 자신만의 메모리 공간인 PGA영역의 Session Memory에 User Process에 대한 정보를 담고 Bind 변수가 있을경우 Private SQL Area에 변수를 담는다.

5. Server Process는 받은 SQL을 SGA의 영역에 있는 Shared Pool의 Library Cache에서 Syntax Check를 한다.

6. Semantic Check를 위해 Data Dictionary Cache를 조회하게 된다. 

7. Server Process는 SQL문들 ASCII 값으로 전환하여 Hash 함수를 수행하고 결과 값인 Hash Value를 통해  
   Shared Pool에있는 Hash Bucket에서 동일한 Hash 값을 갖고 있는 Execution Plan이 있는지 검색한다. 

8. Execution Plan이 있을경우 SQL문을 실행 처리한다. 다음은 실행계획이 없을 경우 이다.

9. Optimizer로 부터 실행계획을 받는다.

10. Row Source Generator로부터 정제된 Row Source를 받는다.

## 2.SELECT문

## 3.UPDATE문