# 베이스 이미지 설정
FROM ubuntu:16.04

# Nginx 설치
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get -y install nginx

# 포트 지정
EXPOSE 80

# 서버 실행
CMD ["nginx", "-g", "daemon off;"]
