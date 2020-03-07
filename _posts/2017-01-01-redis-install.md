---
title: "레디스 설치"
categories: 
  - Redis
last_modified_at: 2017-01-01T22:00:00+09:00
toc: true
---

# Redis 설치
------------------------------------------------------------
레디스를 설치하는 제안된 방법은 레디스가 작동하는 GCC 컴파일러와 libc 외에는 의존성이 없기 때문에 소스로부터 레디스를 컴파일하고 있다. 
일반적으로 사용 가능한 버전이 최신 버전이 아니므로 리눅스 배포의 패키지 관리자를 사용하여 설치하지 마십시오.
예를 들어 rpm, yum, apt-get 을 통해 설치하지 않는 것을 권장한다.

redis.io 웹 사이트 에서 최신 Redis tar ball을 다운로드 하거나 항상 최신의 안정적인 Redis 버전을 가리키는이 특수 URL을 사용할 수 있습니다 
http://download.redis.io/redis-stable.tar.gz . 

Redis를 컴파일하려면 다음 간단한 단계를 수행하십시오.
```
# wget http://download.redis.io/redis-stable.tar.gz
# tar xvzf redis-stable.tar.gz
# cd redis-stable
# make
```

이 때 make test를 입력하여 빌드가 제대로 작동했는지 테스트할 수 있지만 이는 선택 사항이다. 
Redis 배포 내의 src 디렉터리는 Redis의 일부인 다른 실행 파일로 채워진다.

* redis-server는 Redis Server 자체다.
* redis-sentinel은 Redis Sentinel 실행 파일(모니터링 및 페일오버)이다.
* redis-cli는 Redis와 대화하는 명령줄 인터페이스 유틸리티다.
* redis-benchmark는 Redis 성능을 점검하는 데 사용된다.
* redis-check-aof 및 redis-check-rdb(3.0 이하의 redis-check-dump)는 데이터 파일이 손상되는 드문 경우에 유용하다.

```
-----------------------------------------------------------
# cp src/redis-server /usr/local/bin/
# cp src/redis-cli /usr/local/bin/     
  or
# make install
-----------------------------------------------------------
# mkdir /etc/redis
# mkdir /var/redis
# cp utils/redis_init_script /etc/init.d/redis_6379
# cp redis.conf /etc/redis/6379.conf
# mkdir /var/redis/6379
```

구성 파일을 편집하여 다음 변경을 수행하십시오.
```
# vi /etc/redis/6379.conf
```
* daemonize 를 no => yes로 변경하십시오 (기본적으로 no로 설정 됨).
* 선호하는 loglevel을 설정하십시오 .
  * debug (개발/테스트에 유용한 많은 정보. )
  * verbose (유용한 정보는 거의 없지만 디버그 수준처럼 엉망진창은 아님)
  * notice (중간정도의 많은 정보, 아마도 배포할때 원할것)
  * warning (매우 중요/중요한 메시지만 기록됨)
* logfile 에 "" => "/var/log/redis_6379.log" 변경
* dir 을 ./ => /var/redis/6379로 변경 하십시오 (매우 중요한 단계!)

# Redis Run 레벨 등록
```
# chkconfig --add redis_6379
```


# Redis 시작
```
# chown -R user redis-stable
------------------------------------
$ service redis_6379 start
or
$ redis-server
-------------------------------------
```

위의 예에서 Redis는 명시적인 구성 파일 없이 시작되었으므로 모든 파라미터는 내부 디폴트를 사용한다. 
Redis를 단지 가지고 놀거나 개발을 위해 시작한다면 이것은 완벽하지만, 프로덕션 환경에서는 구성 파일을 사용해야 한다.

Redis를 시작할때 구성 파일을 사용 하려면 redis-stable/redis.conf와 같이 구성 파일의 전체 경로를 첫 번째 인수로 사용하십시오. 
Redis 소스 코드 배포의 루트 디렉터리에 포함된 redis.conf 파일을 템플릿으로 사용하여 구성 파일을 작성하십시오.

# Redis가 작동하는지 확인

외부 프로그램은 TCP 소켓과 Redis 특정 프로토콜을 사용하여 Redis와 통신합니다.
이 프로토콜은 다른 프로그래밍 언어를 위해 Redis 클라이언트 라이브러리에서 구현됩니다. 
그러나 Redis와의 해킹을 단순화하기 위해 Redis에게 명령을 보내는 데 사용할 수 있는 명령줄 유틸리티를 제공한다. 
이 프로그램을 redis-cli라고 한다.

Redis가 제대로 작동하는지 확인하기 위해 가장 먼저해야 할 일은 redis-cli를 사용하여 PING 명령을 보내는 것입니다 .
PONG이 돌아온다면 인스턴스가 제대로 실행 된 것이다.
```
$ redis-cli ping
PONG
```
redis-cli를 실행하면 해당 명령어는 localhost의 6379 포트에서 실행되는 Redis 인스턴스에 이 명령을 전송한다.
redis-cli를 사용하여 호스트와 포트를 변경할 수 있으며 --help 옵션을 사용하여 사용 정보를 확인할 수 있다.


# Redis 보안
```
# vi /etc/redis/6379.conf
requirepass라고 설정된 부분의 주석을 제거한 후 비밀번호를 입력.

# service redis_6379 stop
# service redis_6379 start

# redis-cli
127.0.0.1:6379> auth password

```

끝