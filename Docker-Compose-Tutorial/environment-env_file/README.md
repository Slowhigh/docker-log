## 컨테이너 환경변수 지정

#### yml 배열 형식 또는 해시 형식

```
# 배열 형식으로 지정
environment:
  - HOGE=fuga
  - FOO=bar

# 해시 형식으로 지정
environment:
  HOGE: fuga
  FOO: bar

```

#### envfile 파일지정으로
```
env_file: envfile

```
<br />

파일의 경로는 상대 경로 또는 절대 경로를 사용합니다.

```
env_file:
  - ./envfile1
  - ./app/envfile2
  - /tmp/envfile3
```
