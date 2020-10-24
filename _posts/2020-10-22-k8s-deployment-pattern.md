---
layout: post
title:  "Deployment Patterns in Kubernetes"
author: Enoch-Kim
categories: [ Tech ]
tags: [Kubernetes]
image: assets/images/k8s-deployment-pattern.png
description: "Deployment pattern 에 관해 공부하고 정리한 글입니다."
featured: false
sitemap :
  changefreq : weekly
---

Deployment Pattern이란 Kubernetes의 Deployment를 사용하여 앱을 배포할때 배포하는 방식을 말한다. 예를들어 새로운 버전이 릴리즈 됐을 때,
무중단 배포를 하기위해서 Rolling Update 방식을 사용하는게 있다.

&nbsp;

#### Rolling Update Deployment Pattern

![Rolling-Update](../assets/images/k8s-rolling-update.png)

Rolling Update Deployment는 모든 서버를 한번에 변경하는 것이 아닌 하나하나 단계적으로 변경하는 패턴이다.
Load Balancer를 통해 트래픽을 하나씩 단계적으로 새로운 서버에 트래픽을 이동시켜 무중단 배포를 가능하게 한다.
보통 update를 시작하면 replicaset을 증가시킨 후, 준비된 replicaset으로 하나하나 이동시키며 기존의 replicaset을 감소시킨다.
이러한 과정을 거치면 downtime이 대폭 감소하는 이점이 있다.
단계 중간 중간 원하는 작업을 설정하는 경우도 있다.

&nbsp;

#### Canary Deployment Pattern

![Canary-Deployment](../assets/images/k8s-canary-deployment.png)

Canary Deployment는 새로운 버전에 대한 테스트를 진행하고 싶을 때 사용하는 Deployment Pattern이다.
Rolling Update와는 다르게 replicaset 전부를 변경하는 것이 아닌, 일부의 replicaset만을 새로운 버전으로 교체하여
일부 사용자에게로부터 테스트를 받는다. 이때 특정 사용자를 정할 수 있는데, 한국에서 온 트래픽만을 새로운 버전으로 라우팅하여
테스트할 수 있다. Canary 서버(테스트 서버)에서 사용자들의 반응을 살펴 테스트 조건이 충족되면 전체 서버를 새로운 버전으로 교체한다.

&nbsp;

#### Blue / Green Deployment Pattern

![Blue-Green-Deployment](../assets/images/k8s-blue-green-deployment.png)

Blue/Green Deployment Pattern은 안전하게 downtime 없이 Deployment 할 때 사용하는 방식이다. (knative에서 사용하는 방식이다.)
기존의 Deployment를 Blue라고 할 때, Green Deployment를 새로 생성한다.
Green이 성공적으로 Deployment 됐을 시, Load Balancer는 Traffic 전부를 Green으로 옮긴다.
만약 Green에 문제가 생기면 Blue로 Traffic을 Roll Back 시킨다.
문제가 없다면 Blue Deployment를 제거한다.

이외에도 다양한 Deployment Pattern이 있으나, 추후에 업데이트...
