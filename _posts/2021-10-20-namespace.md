# namespace  10/20

**Cluster**

란, 물리 또는 가상 머신들이 묶여서 하나의 시스템처럼 동작하는 집합을 의미합니다.

물리 클러스터 안에서 논리적으로 하나의 시스템처럼 동작하는 집합을

**Namespace**

라고 합니다.

pods들을 감싸는  포장지의 이름 느낌?

작은 혹은 혼자 쓸땐 상관없는데  기업단위로 늘어나면  이  namespace 가필요함!

pod들의 이름이 갖을때도  `namespace` 

별로 구분해줘야하기때문에!

kubectl get pods —namespace=(namespace name)

change default namespace

→  k config set-context $(kubectl config current-context) —namespace=dev    

dev 쪽 pods들을  기본값으로 만들때

그러면  정보확인시

k get pods —namespace=dev  에서   k get pods 로 됨

```yaml
root@controlplane:~# alias k="kubectl"
root@controlplane:~# k get namespace

k get ns --no-headers | wc -l
10

이렇게도  확인가능
```

```yaml
k run redis --image=redis --dry-run-client -o yaml > pod.yml

이렇게 생성해서   vi pod.yaml 로 보고  해도 상관없지만

kubectl run redis --image=redis -n finance
```

해당  이름이 어느  ns인지 알아보려면

```yaml
root@controlplane:~# k get pods --all-namespaces | grep blue 
```

아래 명령어로 현재 만들어진 서비스 정보들을 확인

```yaml
$ kubectl get services
```

```
[root@nasa-master ~]# kubectl get service
NAME         TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
kubernetes   ClusterIP   10.96.0.1        <none>        443/TCP          23d
nasaecho     NodePort    10.109.171.229   <none>        8080:30880/TCP   5m3s
```

**`Cluster IP` : 서비스에 클러스터 IP (내부 IP)를 할당합니다.**

**`Load Balancer` : 외부 IP를 가진 로드밸런서를 할당합니다.**

**`Node Port` : 클러스터 IP 뿐만 아니라 노드의 IP 및 포트를 통해서 접근을 할 수 있습니다.**

 service 의 정보

```yaml
root@controlplane:~# k describe service
Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
provider=kubernetes
Annotations:       <none>
Selector:          <none>
Type:              ClusterIP
IP Families:       <none>
IP:                10.96.0.1
IPs:               10.96.0.1
Port:              https  443/TCP
TargetPort:        6443/TCP
Endpoints:         10.39.154.3:6443
Session Affinity:  None
Events:            <none>
```

외부접속

 k expose ~~

## Imperative vs  Declarative

`imperative`  는 순차   how             명령

`Declarative` 는 원하느상태를 명시 what       서술

  -  Terraform  , Ansible  

중간에 수정   변경등에는  declarative   가 유리함

```yaml
kubectl create deployment redis-deploy --image=redis --replicas=2 -n dev-ns
```

kubectl create 는 포드 이외의 다양한 객체를 생성하는 데 사용되며 kubectl run 은 버전 1.18에 따라 포드만 생성하는 데 사용됩니다.

declarative     → 선언형