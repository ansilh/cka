# K8S CKA Exam preperation guide


**Practice Lab**

1.Install cluster using kubeadm (will setup via hardway later)
 - one master & 4 workers
 - weave net
 - metric server
 - dashboard
 - metallb

2.Gateone Web Terminal 
 - Only for lab (you have to deal with this in actual exam)

3.Get familier with the structure of kubernetes.io so that you can navigate easily during exam

**Preperation**

1.tmux

```
tmux list-sessions
tmux detach -s <>
tmux attach -t <>

C-b % - vertical split
C-b " - horizontal split
C-b Pageup/Down ( quit q) - Scroll
C-b c - New session
C-b n - Next session
C-b p - Previous session 
C-b : - Command line
 setw synchronize-panes on - Trun on command execution on all panes
 setw synchronize-panes off - Trun on command execution on all panes
```

2.Enable bash completion 
```
source <(kubectl completion bash)
```
3.Open kubernetes.io in second tab for reference

4.YAML to JSON one liner
```
$ ruby -ryaml -rjson -e 'puts JSON.pretty_generate(JSON.parse(YAML.load(ARGF.read).to_json))' <spec.yaml
$ python -c 'import sys, yaml, json; y=yaml.load(sys.stdin.read()); print json.dumps(y,indent=4,sort_keys=True)'  <spec.yaml
```

**Topics**
* Security

$kubectl auth can-i create pod --as ansil --namespace shopping-app

Three APIs can be applied to set who and what can be queried 
```
- SelfSubjectAccessReview
- LocalSubjectAccessReview
- SelfSubjectRuleReview
```
reconcile ?

- Resource version is backed via modifiedIndex parameter in etcd database
* Annotations is another type of metadata in an object used by componetnts outside of kubernetes
```
$ kubectl annotate pod nginx descreption="Nginx v1" -n shopping-app
$ kubectl annotate --overwrite pods descreption="Nginx v1 old" -n shopping-app
$ kubectl annotate pods descreption- -n shopping-app
```
* Kubectl verbose output
```
$kubectl --v=10 get pods nginx
```
verbosity ranges from 0 to 9 
* Imparative commands 
```
kubectl run --restart=Always # creates a Deployment
kubectl run --restart=Never # creates bare pod
kubectl run --restart=OnFailure # creates a Job.
```
* List containers 
```
runc --root /run/containerd/runc/k8s.io/ list
runc --root /run/containerd/runc/k8s.io/ ps 0181be912592e680746df7fa9cc8b1e2b1630c784748dbe1ae3898330d484902
``` 
* Display custom columns
```
kubectl get runtimeclasses -o=custom-columns=NAME:.metadata.name,HANDLER:.spec.runtimeHandler,TIMESTAMP:.metadata.creationTimestamp
```
* Add insecure registry
```
sudo vi /etc/containerd/config.toml
```
>Append below contents
```
[plugins.cri.registry]
      [plugins.cri.registry.mirrors]
        [plugins.cri.registry.mirrors."172.168.3.164:5000"]
          endpoint = ["http://172.168.3.164:5000"]
        [plugins.cri.registry.mirrors."docker.io"]
          endpoint = ["https://registry-1.docker.io"]
```

* `containerd` commands 

```
sudo crictl --runtime-endpoint /run/containerd/containerd.sock ps
```
>Output
```
CONTAINER ID        IMAGE               CREATED             STATE               NAME                      ATTEMPT             POD ID
0abf4e2740686       367cdc8433a45       7 hours ago         Running             coredns                   5                   8b196d3060730
1e06da2fc70b4       f9aed6605b814       7 hours ago         Running             kubernetes-dashboard      5                   4bbdffab4c122
4bd47862023ae       367cdc8433a45       7 hours ago         Running             coredns                   5                   c0f287f17d72f
aaf4eb209416c       490d921fa49cc       7 hours ago         Running             install-cni               5                   658a3557ea6c8
32d903f2c3655       ac09b0327d10b       7 hours ago         Running             calico-etcd               5                   8101e57c60047
233ccadb714e1       ed5e65eb295ed       7 hours ago         Running             speaker                   3                   244c72b21fdb6
583deb7cb82d0       4e9be81e3a594       7 hours ago         Running             calico-node               7                   658a3557ea6c8
889a91c36cafa       22c16a9aecce9       7 hours ago         Running             calico-kube-controllers   6                   ed599e207cad3
```
```
ubuntu@k8s-master-01:/localdocker$ sudo crictl --runtime-endpoint /run/containerd/containerd.sock images
```
>Output

```
IMAGE                                              TAG                 IMAGE ID            SIZE
172.168.3.164:5000/tagtest                         latest              47b19964fb500       32.4MB
docker.io/calico/cni                               v3.3.2              490d921fa49cc       24.2MB
docker.io/calico/kube-controllers                  v3.3.2              22c16a9aecce9       14.8MB
docker.io/calico/node                              v3.3.2              4e9be81e3a594       20.5MB
docker.io/coredns/coredns                          1.2.2               367cdc8433a45       11.9MB
docker.io/istio/examples-bookinfo-details-v1       1.8.0               21f25915c88ef       94.3MB
docker.io/istio/examples-bookinfo-productpage-v1   1.8.0               a151027e867ac       50.8MB
docker.io/istio/examples-bookinfo-ratings-v1       1.8.0               10d548a159602       88.4MB
docker.io/istio/examples-bookinfo-reviews-v1       1.8.0               bb692bde5c114       310MB
docker.io/istio/examples-bookinfo-reviews-v3       1.8.0               7e51add28c44c       310MB
docker.io/istio/mixer                              1.0.5               582d5c76010e5       16.6MB
docker.io/istio/proxy_init                         1.0.5               6eb1005cb3877       55.1MB
docker.io/istio/proxyv2                            1.0.5               e393f805ceac9       128MB
docker.io/metallb/speaker                          v0.7.3              ed5e65eb295ed       11.2MB
k8s.gcr.io/kubernetes-dashboard-amd64              v1.10.1             f9aed6605b814       44.9MB
k8s.gcr.io/pause                                   3.1                 da86e6ba6ca19       317kB
quay.io/coreos/etcd                                v3.3.9              ac09b0327d10b       13.3MB

```
