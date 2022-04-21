#  STEP:1 Ubuntu (베이스 이미지)
FROM ubuntu:latest

# STEP:2 Nginx 설치
RUN apt-get update && apt-get install -y -q nginx

# STEP:3 파일 복사
COPY index.html /user/share/nginx/html/

# STEP:4 Nginx 시작
CMD ["nginx", "-g", "daemon off;"]
