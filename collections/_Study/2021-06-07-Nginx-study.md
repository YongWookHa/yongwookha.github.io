---
layout: post
title: Nginx로 웹서빙
subtitle: 로드밸런싱, 포트포워딩
tags: [STUDY, DEVELOP]
cover-img: /assets/img/nginx.jpg
comments: true
---

API로 개발되는 서비스를 바깥과 연결하는 관문으로 nginx를 자주 사용합니다. 조금만 배우면 사용이 간단하고, 강력한 기능을 제공하기 때문입니다. nginx는 웹 서비스와 관련된 매우 다양한 기능들을 제공하지만, 이번 포스트에서는 제가 일부 프로젝트에서 이용했던 `로드밸런서`와 `포트포워딩`을 정리해봅니다.

## NGINX 설치

- 명령어를 이용한 설치  
```bash
sudo apt-get install nginx  # ubuntu
sudo yum install nginx  # centos
```

- 설정 파일 찾기  
```bash
sudo find / -name nginx.conf
```  
`/etc/nginx/nginx.conf` 가 결과로 나왔다고 가정하면, `/etc/nginx` 경로에는 `conf.d`라는 폴더가 함께 있을겁니다. `nginx.conf`는 `conf.d`폴더 안에 있는 `*.conf`를 모두 참조합니다. 우리는 이곳에 우리가 custom할 설정을 추가합니다.

```
/etc/nginx
├─ conf.d
│  ├─ my.conf
│  └─ ...
├─ nginx.conf
├─ proxy_params
└─ ...
```

## Load Balancing

![](https://www.dropbox.com/s/nrmvm5t9nx2zda1/nginx-load-balancer-overview.png?raw=1)

로드밸런싱은 여러 서버 인스턴스가 있을 때, 클라이언트의 요청을 적절히 분배해주는 역할을 합니다. 이를 통해 무중단 서비스가 가능하며, 서비스의 확장이나 변경에 대해서도 유연한 시스템을 구축할 수 있습니다. 클라이언트의 요청을 각 서버에 분배하는 방법으로는 OS에서 CPU time slice 할당 방법으로 배웠던 `Round Robin`(default)이 있고, 가장 접속 수가 적은 서버에 연결하는 `Least-connected`와 hash를 통해 IP별로 접속 서버를 고정하는 `ip-hash` 방법이 있습니다.

```  
# /etc/nginx/conf.d/my.conf

upstream my_server {
    # least_conn, ip_hash;  # 요청 분배 방법 명시 (default: round_robin)

    # 서버 주소 나열
    server 192.168.0.25:22222;
    # server 192.168.0.25:22222 weight=2; (요청을 더 많이 처리할 서버 설정 가능)
    server 192.168.0.26:33333;
    server 192.168.0.26:44444;
}

server {
    listen 11111;

    location / {
        proxy_pass http://my_server;
    }
}

```  

## Port Forwarding

docker등을 이용해서 내부 IP에서 서비스가 구동되는 경우, nginx를 이용해서 외부 접근 포트를 바인딩하여 내부 컨테이너의 특정 포트로 연결할 수 있습니다. 마찬가지로 `/etc/nginx/conf.d`에 별도의 설정 파일 `my.conf`를 만들어 포트포워딩을 한다고 가정합니다.

```
# /etc/nginx/conf.d/my.conf

server {
    listen 11111;
    proxy_pass 192.168.0.25:22222;
}
```

매우 간단한 위의 설정만으로도 외부의 11111 포트를 내부의 192.168.0.25.22222로 포워딩할 수 있습니다.
