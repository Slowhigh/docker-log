# Deploy a registry server

`registry`를 배포하려면 먼저 host에 Docker를 설치해야 합니다. `registry`는 [registry](https://hub.docker.com/_/registry) 이미지의 인스턴스이고 Docker 내에서 실행됩니다.

본문의 주제는 `registry`를 배포하는 것과 설정하는 것에 대한 기본적인 정보를 제공합니다. 전체 구성 옵션 목록은 [configuration reference](https://docs.docker.com/registry/configuration/)를 참조하세요.

<br>

## Run a local registry
다음과 같은 명령을 사용하여 `registry` 컨테이너를 실행하세요.
```bash
$ docker run -d -p 5000:5000 --restart=always --name registry registry:2
```
이제 registry를 사용할 준비가 되었습니다.

<br>

## Copy an image from Docker Hub to your **local** registry
Docker Hub에서 이미지를 pull하여 `registry`에 push할 수 있습니다. 다음 예제는 Docker Hub에서 `ubuntu:16.04`를 pull하여 `my-ubuntu`와 같이 tag를 다시 설정하고 `local registry`에 push합니다. 마지막으로, local에서 `ubuntu:16.04`와 `my-ubuntu` 이미지를 삭제하고 `local registry`에서 `my-ubuntu` 이미지를 pull합니다.

```bash
$ docker pull ubuntu:16:04

$ docker tag ubuntu:16:04 localhost:5000/my-ubuntu

$ docker push localhost:5000/my-ubuntu

$ docker image remove ubuntu:16:04
$ docker image remove localhost:5000/my-ubuntu

$ docker pull localhost:5000/my-ubuntu
```

<br>

## Copy an image from Docker Hub to your **remote** registry
다음 예제는 Docker Hub에서 `ubuntu:16.04`를 pull하여 `my-ubuntu`와 같이 tag를 다시 설정하고 `remote registry`에 push합니다. 마지막으로, local에서 `ubuntu:16.04`와 `my-ubuntu` 이미지를 삭제하고 `remote registry`에서 `my-ubuntu` 이미지를 pull합니다.

### Host(Remote) PC 
Host PC의 IP는 `192.168.0.10`으로 가정합니다.
아래와 같은 명령어로 Host PC에서 `registry`를 실행합니다.
```bash
$ docker run -d --restart=always -p 5000:5000 -v /data/registry:/var/lib/registry/Docker/registry/v2 --name registry registry:latest
```

### Client(Local) PC
아래와 같이 Local PC에서 Docker Hub로부터 `ubuntu:16.04`를 pull하여 `my-ubuntu`와 같이 tag를 다시 설정하고 `remote registry`에 push합니다.
```bash
$ docker pull ubuntu:16:04

$ docker tag ubuntu:16:04 192.168.0.10:5000/my-ubuntu

$ docker push 192.168.0.10:5000/my-ubuntu

$ docker image remove ubuntu:16:04
$ docker image remove 192.168.0.10:5000/my-ubuntu

$ docker pull 192.168.0.10:5000/my-ubuntu
```

`Get https://192.168.0.10:5000/v2/: http: server gave HTTP response to HTTPS client`라는 error가 발생한다면?

- **[windows OS]** `Docker Desktop -> 설정 -> Docker Engine`에 아래와 같이 `"insecure-registries"`의 목록에 `"192.168.0.10:5000"`를 추가하고 재시작합니다.
    ```
    {
      ...
      },
      "insecure-registries": [
          ...
          "192.168.0.10:5000"            <--- this
      ]
    }
    ```

- **[linux OS]** `/etc/docker/daemon.json`에 아래 내용을 추가 후 Docker를 재시작합니다. (만약 파일이 없다면 생성합니다.)
    ```
    {
        ...
        "insecure-registries": ["<host ip>:5000"]
        ...
    }
    ```

<br>

## Stop a local registry
```bash
# registry 컨테이너를 중지합니다.
$ docker container stop registry

# registry 컨테이너를 중지하고 삭제합니다.
$ docker container stop registry && docker container rm -v registry
```

## Customize the published port
5000 포트번호를 이미 사용중이라면 아래와 같이 host 포트번호를 5001로 변경할 수 있습니다. 추가적으로, `registry`는 컨테이너 내에서 기본적으로 5000 포트번호로 수신 대기합니다.
```bash
$ docker run -d -p 5001:5000 --name registry registry:2
```
`registry` 컨테이너 내에서 기본적으로 수신 대기하는 포트번호(5000)를 변경하고 싶으면 아래와 같이 환경 변수 `REGISTRY_HTTP_ADDR`를 활용합니다.
```bash
$ docker run -d -e REGISTRY_HTTP_ADDR=0.0.0.0:5001 -p 5001:5001 --name registry registry:2
```



```bash
# $ mkdir certs


# $ openssl genrsa -des3 -out certs/domain.key
# $ cat certs/domain.key
# 
# $ openssl req -new -x509 -key certs/domain.key -out certs/domain.crt
# $ cat certs/domain.crt
# 
# $ docker secret create domain.key certs/domain.key
# $ docker secret create domain.crt certs/domain.crt
# $ docker secret ls




# Input: 1234, 1234
# $ openssl genrsa -des3 -out domain.key
# Generating RSA private key, 2048 bit long modulus (2 primes)
# ...................................................................+++++
# ...............+++++
# e is 65537 (0x010001)
# Enter pass phrase for domain.key:
# Verifying - Enter pass phrase for domain.key:

# Input: KR, Seoul, Jung-gu, Someday, SW, www.someday.com, sw@someday.com, [Enter], [Enter]
# $ openssl req -new -key domain.key -out domain.csr
# Enter pass phrase for domain.key:
# You are about to be asked to enter information that will be incorporated
# into your certificate request.
# What you are about to enter is what is called a Distinguished Name or a DN.
# There are quite a few fields but you can leave some blank
# For some fields there will be a default value,
# If you enter '.', the field will be left blank.
# -----
# Country Name (2 letter code) [AU]:KR
# State or Province Name (full name) [Some-State]:Seoul
# Locality Name (eg, city) []:Jung-gu
# Organization Name (eg, company) [Internet Widgits Pty Ltd]:Someday
# Organizational Unit Name (eg, section) []:SW
# Common Name (e.g. server FQDN or YOUR name) []:www.someday.com
# Email Address []:sw@someday.com

# Please enter the following 'extra' attributes
# to be sent with your certificate request
# A challenge password []:
# An optional company name []:

# $ cp domain.key domain.key.origin
# $ openssl rsa -in domain.key.origin -out domain.key

# $ openssl x509 -req -days 3650 -in domain.csr -signkey domain.key -out domain.crt
# Signature ok
# subject=C = KR, ST = Seoul, L = Jung-gu, O = Someday, OU = SW, CN = www.someday.com, emailAddress = sw@someday.com
# Getting Private key


# 참고: openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /d/tmp/nginx.key -out /d/tmp/nginx.crt -subj "/CN=my-nginx/O=my-nginx"
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout domain.key -out domain.crt -subj "/C=KR/ST=Seoul/L=Jung-gu/O=SomeDay/OU=SW/CN=www.someday.com/emailAddress=sw@someday.com"


$ docker secret create domain.key domain.key
$ docker secret create domain.crt domain.crt


$ docker node ls
$ docker node update --label-add registry=true docker-desktop
$ docker node inspect docker-desktop

$ docker service create \
  --name registry \
  --secret domain.crt \
  --secret domain.key \
  --constraint 'node.labels.registry==true' \
  --mount type=volume,src=registry-repo,dst=/var/lib/registry \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/run/secrets/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/run/secrets/domain.key \
  --publish published=443,target=443 \
  --replicas 1 \
  registry:2

$ docker run -d \
  --restart=always \
  --name registry \
  -v "$(pwd)"/certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  -p 443:443 \
  registry:2

```