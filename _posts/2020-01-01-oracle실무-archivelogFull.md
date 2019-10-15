---
title: "Oracle 실무 Archive log Full"
categories: 
  - Oracle 실무
last_modified_at: 2020-01-01T23:00:00+09:00
toc: true
---



# Oracle 10g Archive Log 파일 관리

# 증상
```
SQL> alter database open;
alter database open
*
ERROR at line 1:
ORA-16038: log 2 sequence# 618 cannot be archived
ORA-19809: limit exceeded for recovery files
ORA-00312: online log 2 thread 1:
'/home01/oracle/ocs/oradata/OCSD/onlinelog/o1_mf_2_1k7xomh4_.log'
ORA-00312: online log 2 thread 1:
'/home02/oracle/ocs/infra/flash_recovery_area/OCSD/onlinelog/o1_mf_2_1k7xormv_.log'
```
# Archive Log 파일을 정리
1) db_recovery_file_dest_size를 증가시켜 주면 된다. 바로 반영됩니다.
    SQL> alter system set db_recovery_file_dest_size=xG;

2) Stop using the db_recovery_file_dest by unsetting the parameter.
   ( This assumes you never really wanted to use this option )

3)  rman repository/Controlfile 에서 엔트리들을 삭제한다.

 아래 명령으로 최대치 까지의 로그 사용량 확인
```
SQL> conn / as sysdba
SQL> SELECT * FROM V$RECOVERY_FILE_DEST;
NAME                                      SPACE_LIMIT  SPACE_USED  SPACE_RECLAIMABLE NUMBER_OF_FILES
----------- ---------- ----------------- -------------------------------------------------------------
/data/oracle/flash_recovery_area    4070572032      4051497472                  0                     103

```
-- 다음과 같이 조치한다.
-- RMAN 사용해서 ARCHIVE LOG 삭제

```
Recovery Manager: Release 10.2.0.2.0 - Production on Fri Nov 2 19:40:19 2012
 
Copyright (c) 1982, 2005, Oracle.  All rights reserved.
 
RMAN> connect target /
 
connected to target database: ORACLE3 (DBID=642158494)
 
-- 로그 리스트 조회
RMAN> LIST ARCHIVELOG LIKE '%.arc';
 
using target database control file instead of recovery catalog
 
List of Archived Log Copies
Key     Thrd Seq     S Low Time  Name
------- ---- ------- - --------- ----
1       1    1100    A 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1100_7gzshjn0_.arc
2       1    1101    A 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1101_7h0vvp2c_.arc
3       1    1102    A 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_21/o1_mf_1_1102_7h2cx5pv_.arc
 
-- 전부 삭제
RMAN> DELETE ARCHIVELOG LIKE '%.arc';
```

- 만료된 로그 삭제(만료되지 않은 로그는 존재 유지)
```
RMAN> LIST ARCHIVELOG LIKE '%.arc';
 
List of Archived Log Copies
Key     Thrd Seq     S Low Time  Name
------- ---- ------- - --------- ----
1       1    1100    A 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1100_7gzshjn0_.arc
2       1    1101    A 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1101_7h0vvp2c_.arc
 
RMAN> crosscheck archivelog all;
released channel: ORA_DISK_1
allocated channel: ORA_DISK_1
channel ORA_DISK_1: sid=198 devtype=DISK
validation failed for archived log
archive log filename=/app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1100_7gzshjn0_.arc recid=1 stamp=770380421
validation failed for archived log
archive log filename=/app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1101_7h0vvp2c_.arc recid=2 stamp=770415627
Crosschecked 2 objects
 
RMAN> delete expired archivelog all;
 
released channel: ORA_DISK_1
allocated channel: ORA_DISK_1
channel ORA_DISK_1: sid=198 devtype=DISK
 
List of Archived Log Copies
Key     Thrd Seq     S Low Time  Name
------- ---- ------- - --------- ----
1       1    1100    X 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1100_7gzshjn0_.arc
2       1    1101    X 20-DEC-11 /app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1101_7h0vvp2c_.arc
 
Do you really want to delete the above objects (enter YES or NO)? yes
deleted archive log
archive log filename=/app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1100_7gzshjn0_.arc recid=1 stamp=770380421
deleted archive log
archive log filename=/app/oracle/flash_recovery_area/ORACLE3/archivelog/2011_12_20/o1_mf_1_1101_7h0vvp2c_.arc recid=2 stamp=770415627
Deleted 2 EXPIRED objects
 
RMAN> LIST ARCHIVELOG LIKE '%.arc';
specification does not match any archive log in the recovery catalog
```

# 하나씩 지우는게 번거러울 경우 아래와 같이 한번에 지우는 방법도 있음.
1. os 상에서 우선 파일을 삭제한다.
```
   # cd $ORACLE_BASE/flash_recovery_area/$ORACLE_SID/archivelog/yyyy_mm_dd
   # rm -r $ORACLE_BASE/flash_recovery_area/$ORACLE_SID/archivelog/'삭제할 디렉토리명'
```

2. RMAN을 실행한다.
```
  # $ORACLE_HOME/bin/rman
  RMAN>connect target /
  RMAN>crosscheck archivelog all; -> marks the controlfile that the archives have been deleted
  RMAN>delete expired archivelog all; -> deletes the log entries identified above
```