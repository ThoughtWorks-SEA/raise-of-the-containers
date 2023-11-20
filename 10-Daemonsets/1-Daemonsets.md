# Daemonsets

### One Pod on each node
```s
# split plane to watch pods
watch kubectl get pods -o wide

# go through descriptor file
kubectl apply -f dobby-daemonsets.yaml

# go through descriptor file regarding match labels
# show that pod get created but immediately terminated
kubectl apply -f dobby.yaml

# (OPTIONAL) could show that the reverse is not true

# delete the daemonset first
kubectl delete ds dobby-daemon-set

# create the single pod
kubectl apply -f dobby.yaml

# store the resulting descriptor file somewhere
kubectl describe pod dobby-pod > dobby-pod.yaml

# create the daemonset
kubectl apply -f dobby-daemonsets.yaml

# note that the original pod gets deleted
# then replaced with a daemonset created pod

# store the resulting descriptor file somewhere
kubectl describe pod dobby-daemon-set-xxx > dobby-pod-ds.yaml

# compare the two files in IDE
# note that daemonsets add some additional labels and tolerations to the pod
# this ensures that each node always have one copy of the pod
# probably why the original pod gets evicted and replaced with one create by daemonset controller
# this behaviour is different from replicaset as rs doesn't evict existing matching pods

```