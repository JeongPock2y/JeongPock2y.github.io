# 10/20

pod 는 하나의  컨테이너만을 갖지않아도 된다  2개 이상도 가능!

pods  → po

replicasets → rs

Kind : Deployment

```yaml
k create -f deployment-definition.yml

파일 생성

 k get deployments
생성된  파일의 
 kubectl get replicaset

```

- **레플리카셋(Replicaset) 이란?**

레플리카셋은 실행되는 파드 개수에 대한 가용성을 보증 하며 지정한 파드 개수만큼 항상 실행될 수 있도록 관리 합니다. 즉 5개의 파드를 항상 실행 하도록 설정하면 이후 파드 1개가 삭제될 경우 다시 파드 1개가 실행되어 5개를 유지할 수 있도록 해줍니다.

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: test-replicaset
spec:
  template:
    metadata:
      name: test-replicaset
      labels:
        app: test-replicaset
    spec:
      containers:
      - name: test-replicaset
        image: nginx
        ports:
        - containerPort: 80
  replicas: 3
  selector:
    matchLabels:
      app: test-replicaset
```

- apiVersion apps/v1 → 쿠버네티스의 apps/v1 API를 사용 합니다.
- kind: ReplicaSet → ReplicaSet의 작업으로 명시 합니다.
- metadata.name → Replicaset 이름을 설정 합니다.
- spec.template.metadata → 어떤 파드를 실행할지에 대한 정보를 하위에 설정 합니다.
- spec.template.metadata.name → 생성될 파드의 이름을 지정 합니다.
- spec.template.metadata.labels.app:test-replicaset → 식별하는 레이블이 앱 컨테이너이며 test-replicaset 으로 식별
- spec.spec → 이 하위의 옵션들은 컨테이너에 대한 설정을 합니다. 위 코드에선 컨테이너 명, 이미지, 포트를 지정 했습니다.
- replicas → 파드의 개수를 몇개 유지할 것 인지 설정 합니다. 기본값은 1 입니다.
- selector → 어떤 레이블의 파드를 선택하여 관리할지에 대한 설정 입니다. 앱 컨테이너의 test-replicaset 레이블을 식별하여 해당되는 파드들을 관리하며, 이 필드가 없을 경우 spec.template.metadata.labels.app 에 적은 내용들을 기본값으로 사용 합니다.

정리 

 apiVersion :    말그대로  api 버전

kind  :  이 리소스의 종류

metadata :   라벨  주석  이름등  리소스의 부사 정보

spec :  리소스 생성을 위한 자세한 정보들        

           그래서 그안에 containers 항목을 작성하여 하위에 image 등등 사용 커                  

         이미지지정    

> yaml 파일에서 - 는   여러개의 항목을 정의할 수 있음을 의미!
> 

```yaml
이미 보셨겠지만, YAML 파일을 만들고 편집하는 것은 조금 어렵습니다. 특히 CLI에서는 더욱 그렇습니다. 검사하는 동안 브라우저에서 터미널로 YAML 파일을 복사하여 붙여넣기가 어려울 수 있습니다. kubectl run 명령을 사용하면 YAML 템플릿을 생성하는 데 도움이 될 수 있습니다. 그리고 때로는 YAML 파일을 전혀 만들지 않고도 kubectl run 명령만으로 빠져나갈 수도 있습니다. 예를 들어 특정 이름과 이미지를 사용하여 포드 또는 배포를 생성하라는 메시지가 표시되면 kubectl run 명령을 실행하면 됩니다.

아래 명령 집합을 사용하고 이전 연습 테스트를 다시 시도하지만 이번에는 YAML 파일 대신 아래 명령을 사용해 보십시오. 앞으로 할 수 있는 한 많이 사용해보세요.

참조(검사를 위해 이 페이지를 예약합니다. 매우 편리할 것입니다.)

https://kubernetes.io/docs/reference/kubectl/conventions/

NGINX 포드 만들기

kubectl 실행 nginx --image=nginx

POD 매니페스트 YAML 파일(-o YAML)을 생성합니다. 생성 안 함(---dry-run)

kubectl run nginx --image=nginx --dry-run=client -o yaml

배포 생성

kubectl 배포 생성 --image=nginx nginx

배포 YAML 파일(-o YAML)을 생성합니다. 생성 안 함(---dry-run)

kubectl 배포 생성 --image=nginx nginx --dry-run=client -o yaml

배포 YAML 파일(-o YAML)을 생성합니다. 4개의 복제본(--dry-run)으로 작성 안 함(--dry-run)

kubectl 배포 생성 --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.l

파일을 파일로 저장하고 필요한 내용(예: 복제본 추가)을 변경한 다음 배포를 만듭니다.

kubectl 생성 -f nginx-deployment.cl

수술실

k8s 버전 1.19 이상에서는 --replicas 옵션을 지정하여 4개의 복제본이 있는 배포를 만들 수 있습니다.

kubectl 배포 생성 --image=nginx nginx --dry-run=4 --client -o yaml > nginx-deployment.l
```