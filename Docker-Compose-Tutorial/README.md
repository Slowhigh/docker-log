# Docker Compose

#### 현재 디렉토리 이외의 장소에 docker-compose.yaml 파일이 있는 경우 -f 옵션으로 파일 경로를 지정
```
$ docker-compose -f ./sample/docker-compose.yaml up
```

#### 서브 명령 다음에 컨테이너명을 지정하면 해당 컨테이너만을 조작할 수 있습니다.
```
$ docker-compose stop webserver
```


#### 서브 명령 다음에 컨테이너명을 지정하지 않으면 모든 컨테이너가 실행되고, 컨테이너명을 지정하면 지정한 컨테이너만 실행됩니다.
```
$ docker-compose up

$ docker-compose up webserver

$ docker-compose up webserver redis
```

#### 컨테이너 백그라운드로 실행
```
$ docker-compose up -d
```

#### Docker 이미지 빌드
```
$ docker-compose up --build
```

#### 컨테이너 개수 지정(server_a 컨테이너 10개 실행, server_b 컨테이너 20개 실행)
```
$ docker-compose up --scale server_a=10 --scale server_b=20
```

#### 컨테이너 상태 확인
```
$ docker-compose ps

$ docker-compose ps webserver

$ docker-compose ps webserver redis
```

#### 컨테이너에서 명령 실행
```
$ docker-compose run server_a /bin/bash
```

#### 컨테이너 시작/정지/재시작/일시 정지/재개
```
$ docker-compose start
$ docker-compose start webserver
$ docker-compose start webserver redis

$ docker-compose stop
$ docker-comppse stop webserver
$ docekr-compose stop webserver redis

$ docker-compose restart
$ docker-compose restart webserver
$ docker-compose restart webserver redis

$ docker-compose pause
$ docker-compose pause webserver
$ docker-compose pause webserver redis

$ docker-compose unpause
$ docker-compose unpause webserver
$ docker-compose unpause webserver redis

```

#### 공개 포트 확인
```
$ docker-compose port webserver 80
```


#### 구성 확인
```
$ docker-compose config
```


## 컨테이너 강제 정지/삭제
```
$ docker-compose kill -s SIGINT

$ docker-compose rm
```

|시그널|설명|
|----|-------------------------------|
|SIGHUP|프로그램 재시작|
|SIGINT|키보드로 인터럽트. Ctrl + c 로 송신할 수 있다.|
|SIGQUIT|키보드에 의한 중지. Ctrl + \ 로 송신할 수 있다.|
|SIGTERM|프로세스 정상 종료|
|SIGKILL|프로세스 강제 종료|
|SIGSTOP|프로세스 일시 정지|
지원하는 시그널의 종류는 kill -l 명령으로 확인할 수 있습니다.












