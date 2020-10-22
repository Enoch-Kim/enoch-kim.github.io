---
layout: post
title:  "Heterogeneous Deployment"
author: Enoch-Kim
categories: [ Tech ]
tags: [Kubernetes]
image: assets/images/heterogeneous-multi-cloud.svg
description: "What is Heterogeneous Deployment??"
featured: true
hidden: true
---

**Google Cloud 공식문서의 글을 정리한 글이다.**
[Heterogeneous](https://cloud.google.com/solutions/heterogeneous-deployment-patterns-with-kubernetes)란 Production에서 k8s를 사용할 때 발생하는 상황인데, 동일한 상황이 아닌 서로 다른 지역 혹은 인프라를 가진 곳에서 동일한 앱을 deployment하는 경우를 말한다. 예를들어 한 곳에서 운영하나 Compute, Networking, Storage를 모두 사용하기에 리소스가 부족한 경우, 전 세계가 사용하는 서비스를 런칭하는 경우, 웹 내에서의 트래픽 패턴으로 트래픽 관리가 안될 때 등등의 상황을 뜻한다.

### Multi Cloud

Multi Cloud Deployment는 가장 대표적인 Heterogeneous Deployment 상황이다. Multi Cloud Deployment의 사례는 다음과 같다.

- On-Premises 혹은 Public Cloud 환경에서 트랙픽을 지정하여 사용하는 경우, 장기적인 이전같은 상황에서 필요할 때 트래픽 방향을 클라우드 인프라로 돌려둘 수 있다. 즉, 트래픽 방향을 제어할 수 있다.

- 단일 환경의 취약성을 벗어난 고가용적인 deployment가 가능하다. 예를들어 각각의 상황에 맞는 deployment 환경을 구성하고 그에 따라 트래픽을 조절하거나 스케일링을 하는 등의 작업이 가능하다.

- 물리적으로 가까운 위치로 트래픽을 제어할 수 있다. 예를들어 미국에서 접속하는 사용자에게는 us zone으로, 한국에서 사용하는 사용자에게는 ko zone으로 연결할 수 있다.

### Directing traffic

DNS Traffic Management Service를 통해 DNS 쿼리에 따라 각각의 deployment로 splitting 할 수 있다. 일반적으로 퍼센트로 나눠 트래픽을 관리한다.
custom health check를 통해 고가용성의 DNS mechanism을 구성할 수 있다. 만약 환경이 unhealthy 하게 되면, Status를 Unhealthy로 변경한다. 그렇게 되면 DNS mechanism은 해당 환경으로 보내던 트래픽을 다른 healthy한 환경으로 이동시켜 서비스를 원할하게 유지시키는 것이다.

만약 응답성이 중요한 상황에서는 사용자에게 가장 가까운 위치로 트래픽을 보내도록 DNS mechanism을 설정할 수 있다. NS1, Dyn, Akamai 등과 같은 DNS infrastructure service provider를 사용하는 것도 좋은 방법이다.

`Qwick Lab을 공부하다 찾아본 내용이라 실제 아키텍처 구성에 관한 공부가 미흡하다. 추후에 찾아서 정리할 필요있음.`
<!-- TODO: Heterogeneous 환경에서 아키텍처 구성 및 운용 정리 -->
