# Dockerfile에서 VOLUME 사용하기

## 사용 예시
VOLUME vol1 <br/>
VOLUME ["vol1", "vo12"]

## docker run에 대한 결과
- VOLUME vo1
호스트에 자동으로 volume 1개가 생기고(volume 이름은 랜덤으로 생성된다) 컨테이너의 root 경로에 "vol1"이라는 폴더(volume)와 마운트가 된다.

## 사용 확장 예시
image1's Dockerfile
```
FROM ubuntu:20.04

VOLUME vol1
```

sudo docker -t volume-test . <br/>
sudo docker run -it --name volume-image-1 volume-test /bin/bash <br/>

cd vol1 <br/>
touch test.txt <br/>

ctrl+P + ctrl+Q <br/>

sudo docker run -it -volumes-from volume-image-1 --name volume-image-2 ubuntu:20.04 /bin/bash <br/>

cd vol1 <br/>
해당 경로는 이미 마운트가 되어 있어 test.txt 파일이 존재한다.
