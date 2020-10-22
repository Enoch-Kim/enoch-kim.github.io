---
layout: post
title:  "QWICKLABS Kubernetes in Google Cloud"
author: Enoch-Kim
categories: [ Tech ]
tags: [GCP, Kubernetes, QwickLabs]
image: assets/images/qwick-lab-k8s-in-google.png
description: "QwickLabs의 Kubernetes in Google Cloud Review"
featured: true
hidden: true
---

Google의 QwickLabs의 Kubernetes in Google Cloud 퀘스트를 진행한 후기.
퀘스트의 Prerequisite으로 Google Cloud Essential을 요구하는 것으로 보아 Kubernetes의 Introductory Course인 것 같았다.

1. GSP055(Introduction to Docker) : Docker의 기본을 공부하는 코스. Docker가 무엇인지 간단히 말해주며, Docker Container를 build, run, debug하는 방법과 Docker image를 Docker Hub와 Google Container registry(gcr)에서 가져오고 등록하는 것을 실습한다. (Docker Hub에 등록하는 실습도 하면 더 좋았을 것 같은데 아쉬웠다.)

    gcr에 Image Push

    ```sh
    docker push gcr.io/[project-id]/[image]:[version]
    ```

2. GSP100(Kubernetes Engine: Qwik Start) : Google Kubernetes Engine(GKE)를 기본적으로 사용하는 방법을 공부하는 코스. GKE에서 클러스터를 어떻게 생성하고 설정하는지 알려준다.

    gcloud 명령어(Kubernetes 명령어는 기록할 필요없으니 스킵)

    - Default Zone 설정

    ```sh
    gcloud config set compute/zone [ZONE NAME]
    ```

    - 클러스터 생성

    ```sh
    gcloud container clusters create [CLUSTER NAME]
    ```

    - Google Credential 인증

    ```sh
    gcloud container clusters get-credentials [CLUSTER NAME]
    ```

    - 클러스터 삭제

    ```sh
    gcloud container clusters delete [CLUSTER NAME]
    ```

3. GSP021(Orchestrating the Cloud with Kubernetes) : 접근 Level이 Expert라서 듣지 못했다 ㅠㅠ

4. GSP053(Managing Deployments Using Kubernetes Engine) : k8s의 기본 Deployment 방식을 공부하고 실습하는 코스다.

    [Heterogeneous Deployment](../k8s-heterogeneous-deploy)를 설명한 후 이러한 환경에서 실행하는 Canary Deployment와 Blue-Green Deployment 두가지의 [Deployment Pattern](../k8s-deployment-pattern)을 소개한다.

    gcloud 및 k8s 명령어

    - Deployment의 replicaset을 감소시키는 명령어

        - Rolling Update

        ```sh
        kubectl scale deployment [Deployment Name] --replicas=[Desired Num]
        ```

        - Rolling Update Deployment Pattern

        ```sh
        kubectl edit deployment [Deployment Name]
        # next, edit image version and save by :wq
        ```

        - Check Rolling Update Status

        ```sh
        kubectl rollout history deployment [Deployment Name]
        ```

        - Rolling Update 중 문제가 생겼을 시 중지 및 확인

        ```sh
        kubectl rollout pause deployment [Deployment Name]
        kubectl rollout status deployment [Deployment Name]
        # verify on Pod
        kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
        ```

        - Resume Paused Rolling Update

        ```sh
        kubectl rollout resume deployment [Deployment Name]
        ```

        - Rollback Update

        ```sh
        kubectl rollout undo deployment [Deployment Name]
        # verify
        kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'
        ```

    - Canary Deployment Pattern

        - yaml 파일에서 Canary 명시 (track: canary)

        ```yaml
        apiVersion: apps/v1
        kind: Deployment
        metadata:
        name: hello-canary
        spec:
        replicas: 1
        selector:
            matchLabels:
            app: hello
        template:
            metadata:
            labels:
                app: hello
                track: canary
                # Use ver 1.0.0 so it matches version on service selector
                version: 1.0.0
            spec:
            containers:
                - name: hello
                image: kelseyhightower/hello:2.0.0
                ports:
                    - name: http
                    containerPort: 80
                    - name: health
                    containerPort: 81
        ```

        - Session Affinity (기존의 사용자는 기존의 앱으로 명시)

        ```yaml
        kind: Service
        apiVersion: v1
        metadata:
        name: "hello"
        spec:
        sessionAffinity: ClientIP
        selector:
            app: "hello"
        ports:
            - protocol: "TCP"
            port: 80
            targetPort: 80
        ```

    - Blue/Green Deployment Pattern

        - Blue Deployment를 띄워둔 후 Sevice를 연결

        - 새로 Green Deployment를 생성한 후 준비가 되면 새로운 Service 연결

        - Blue 에서 Green으로 트래픽 이동

        - Rollback 하고 싶다면 Blue의 Service를 재등록.. 쉽죠?

5. GSP051(Continuous Delivery with Jenkins in Kubernetes Engine)

    [Jenkins](https://www.jenkins.io/)를 사용하여 Continuous Delivery Pipeline을 구축하는 코스.
    Jenkins는 오픈소스 자동화 서버이자 CI(Continuous Integration) Tool이다.
    Jenkins는 앱의 빌드, 테스트, 배포 파이프라인을 유연하게 관리할 수 있게 한다.

    이 코스에서는 Helm도 사용하는데 Helm은 오픈소스 Kubernetes Package Manager로,
    저장소에 k8s application을 저장하고 관리할 수 있도록 돕는다.

    이 코스를 따라하면서 Jenkins 겉핥기만 된 것 같다 나중에 따로 Jenkins를 공부해 사용해봐야겠다.
