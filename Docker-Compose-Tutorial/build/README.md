Build or rebuild services
```
docker-compose build
```

<br/>

Dockerfile이 있는 디렉토리의 경로나 Git 리포지토리의 URL을‘context’로 지정합니다.

ex) '/data'에 저장되어 있는 'Dockerfile–alternate'라는 이름의 Dockerfile을 빌드하고 있습니다.
```
services:
  webserver:
    build:
      context: /data
      dockerfile: Dockerfile-alternate
```

<br/>

Docker 이미지를 빌드할 때에 인수를 args로 지정할 수 있습니다.
ex) projectno=1 및 user=someday라는 값을 빌드 시의 변수로 전달하고 있습니다.
```
services:
  webserver:
    build:
      args:
        projectno: 1
        user: someday
```
