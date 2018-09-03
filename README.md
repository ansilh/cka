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
ruby -ryaml -rjson -e 'puts JSON.pretty_generate(JSON.parse(YAML.load(ARGF.read).to_json))' <spec.yaml
```

**Topics**

