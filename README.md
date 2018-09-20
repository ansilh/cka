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
