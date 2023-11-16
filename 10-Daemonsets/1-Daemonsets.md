# Daemonsets

### One Pod on each node
```s
# split plane to watch pods
watch kubectl get pods -o wide

# go through descriptor file
kubectl apply -f dobby-daemonsets.yaml

# go through descriptor file regarding match labels
# show that pod doesn't get created
kubectl apply -f dobby.yaml
```