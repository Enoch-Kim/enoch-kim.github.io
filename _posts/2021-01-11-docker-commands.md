---
layout: post
title:  "Docker Command List"
author: Enoch-Kim
categories: [ Tech ]
tags: [Docker]
image: assets/images/docker-commands/docker.png
description: "Docker cli Command를 모아두기 위해 정리한 글"
featured: false
sitemap :
  changefreq : weekly
---

Docker cli를 사용하면서 보통 --help 옵션으로 찾아보지만, 자꾸 까먹어 시간 낭비를 하게된다. 공부할 겸 Docker Cli-Command를 정리해보자.

```sh
# 도커 이미지를 컨테이너로 생성하는 커맨드 (프로그램 -> 프로세스)
# 컨테이너를 생성하면서 해당 bash shell로 가져오려면
# -d 대신 -it 를 입력하면 해당 커맨드 사용하여 터미널을 입력으로 가져옴
# -p 옵션은 publish라는 뜻.. port가 아니다
docker container run --name <name> -d -p <host port : target port> <image-name:tag>

# 컨테이너를 시작하고 멈추는 코드 (삭제 전에 멈춰야함)
# 여기서 id의 앞부분만 입력해도 해당 이미지에 적용 (Prefix Matching 방식 -> 중복이면 실행 안함)
# 시작하면서 -ai 옵션을 주면 해당 컨테이너에 접속
docker start <container-id or container-name>
docker stop <container-id or container-name>

# 컨테이너의 메타 정보를 나타내는 코드
docker container inspect <container-id or container-name>

# 컨테이너의 현 상태 정보를 나타내는 코드 (리소스 등... 스트리밍 해줌 -> 간단한 모니터링에 좋음)
docker container stats <container-id or container-name>

# 컨테이너를 모두 보여주는 코드
docker ps -a
docker container ls -a

# 실행중인 컨테이너에 접속하는 코드 (exec이 실행중인 컨테이너에 커맨드 입력하는 역할)
# -i 는 터미널로 interactive하게 하는 기능 (터미널 STDIN으로 받아옴)
# -t 는 SSH가 하듯 실제 터미널에 가져오는 기능 (pseudo-tty)
# bash를 입력시 컨테이너 bash shell 가져옴 (alpine 이미지의 경우 sh를 입력하여야함)
docker container exec -it <container-id or continaer-name> bash

# 컨테이너의 port 확인
docker container port <container-name>


# 도커의 가상 네트워크 확인 
# default 네트워크인 bridge, 인터페이스 역할인 none
# 방화벽을 거치지 않고, host와 직접 연결되는 host 네트워크가 기본으로 존재.
docker network ls

# 가상 네트워크의 메타데이터 확인 (attached된 컨테이너 등등)
docker network inspect <network-name>

# 가상 네트워크 생성
# 원하는 가상 네트워크에 컨테이너 생성하려면 생성 시 --network 옵션을 주면됨
# --network=host로 할 시, host port에 직접 연결된다.
docker network create <network-name> <options>

# 네트워크에 컨테이너 연결, 연결 해제
docker connect <network-name> <container-name>
docker disconnect <network-name> <container-name>

# 컨테이너 생성 시 사용자 정의 네트워크는 alias를 동일하게 줄 수 있다.
docker run -d --net <network-name> --network-alias <alias> <image>
docker run -d --net <network-name> --network-alias <alias> <image>

# 도커 이미지를 remote에서 검색 및 가져오기 + 현재 이미지 확인
docker search <image-name>
docker pull <image-name>
docker image ls

# 도커 이미지의 image layer history를 확인
docker image history <image-name>
# 도커 이미지의 meta-data 확인
docker image inspect <image-name>

# image tag는 라벨일 뿐.. 새로운 라벨을 생성할 수 있다.
# 내 레포로 라벨을 생성한 후 내 docker hub 등에 push할 수 있다. 
# 하기 전에 docker login 필수 -> auth 정보는 cat ~/.docker/config.json 으로 확인해보자

docker image tag <source image/:tag> <target image/:tag>
docker image push <repo/image/:tag>

# 도커가 사용하고 있는 공간 확인
docker system df

# 사용하지 않는 이미지 삭제
docker image prune -a


```
