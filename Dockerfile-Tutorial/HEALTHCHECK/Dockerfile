FROM centos:7 

RUN touch /etc/yum.repos.d/nginx.repo && echo -e '[nginx]\nname=nage repo\nbaseurl=http://nginx.org/packages/centos/7/$basearch/\ngpgcheck=0\nenabled=1' > /etc/yum.repos.d/nginx.repo 

RUN yum -y install nginx curl 

HEALTHCHECK --interval=10s --timeout=3s CMD curl -f http://127.0.0.1/ || exit 1 

CMD ["nginx", "-g", "daemon off;"]

