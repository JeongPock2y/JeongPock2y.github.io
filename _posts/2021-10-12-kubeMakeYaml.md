---
layout: single
title:  "Yaml만들고 replicaset에대하여"
---
# 10*12

`Replication Container`  에서는  pod 여러개에  여러사용자와   로드밸런싱으로  여러개  가능

yml  파일에는  apiVersion  ,  Kind   ,  metadata ,   spec  이렇게 4가지 로컬지시자를  가지고있다

메타데이터안에   파일의 정보   ex>   name  labels 등등 

spec에  상세 정보   에는   pod 의 template  

`replica`  와    `template`는 형제자매 (Siblings) 관계이다

kubectl create - 파일 또는 표준 입력에서 리소스를 생성

 pod 및  replication container  등을 설정해 주었다면   위 명령으로  리소스 생성

```yaml
ex>  kubectl  create -f rc-definition.yml
   
     kubectl  get  replicationcontroller

     kubectl  get  pod     
```

 

![Untitled](10%2012%208be4cfed14b0411aada062160d2c07fb/Untitled.png)

### pod 만들어지는 순서

1. `Scheduler`는 API서버를 감시하면서 할당되지 않은 `Pod`이 있는지 체크
    
    unassigned
    
2. `Scheduler`는 할당되지 않은 `Pod`을 감지하고 적절한 `노드`에 할당 (minikube는 단일 노드)
    
    node
    
3. 노드에 설치된 `kubelet`은 자신의 노드에 할당된 `Pod`이 있는지 체크
4. `kubelet`은 `Scheduler`에 의해 자신에게 할당된 `Pod`의 정보를 확인하고 컨테이너 생성
5. `kubelet`은 자신에게 할당된 `Pod`의 상태를 `API 서버`에 전달

### Yaml

[제목 없음](https://www.notion.so/a4ec8ade9f8d4f2b81e02f489b77b193)

```yaml
# Pod 생성
kubectl apply -f echo-pod.yml

# Pod 목록 조회
kubectl get pod

# Pod 로그 확인
kubectl logs echo
kubectl logs -f echo

# Pod 컨테이너 접속
kubectl exec -it echo -- sh
# ls
# ps
# exit

# Pod 제거
kubectl delete -f echo-pod.yml
```

# **ReplicaSet**

> Pod을 단독으로 만들면 Pod에 어떤 문제(서버가 죽어서 Pod이 사라졌다던가)가 생겼을 때 자동으로 복구되지 않습니다. 이러한 Pod을 정해진 수만큼 복제하고 관리하는 것이 ReplicaSet입니다.
> 

## **스케일 아웃**

ReplicaSet을 이용하면 손쉽게 Pod을 여러개로 복제할 수 있습니다.

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: echo-rs
spec:
  replicas: 4
  selector:
    matchLabels:
      app: echo
      tier: app
  template:
    metadata:
      labels:
        app: echo
        tier: app
    spec:
      containers:
        - name: echo
          image: ghcr.io/subicura/echo:v1

```

이전에 작성한 것과 차이점은 `replicas: 4` 입니다. 바로 실행해봅니다.

`kubectl apply -f echo-rs-scaled.yml

# Pod 확인
kubectl get pod,rs`
