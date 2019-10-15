---
title: "Oracle 실무 Synonym ora-38818"
categories: 
  - Oracle실무
last_modified_at: 2020-01-01T23:00:00+09:00
toc: true
---
```
SQL> select oracle_username from fnd_oracle_userid 
SQL> select * from dba_users;

SQL> grant create session to <schema-user>



``` 
```
$> sqlplus apps @$AD_TOP/patch/115/sql/ADZDREG.sql <system-pwd> <apps-user> <schema-name>
or
SQL>  exec fnd_oracle_user_pkg.load_row('<schema-name>', 'CUSTOM', 'INVALID', NULL, 'N', 'B'); -- 옛날 방식
```

