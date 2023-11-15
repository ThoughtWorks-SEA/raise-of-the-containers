# ConfigMaps


### Create ConfigMaps as Env Var
```s
# go through descriptor file
$ kubectl apply -f dobby-config-map.yaml
$ kubectl describe cm dobby-config

# go through descriptor file regarding the env
# note that only two env var are referenced
$ kubectl apply -f dobby-pod.yaml

# access the pod to show that only two env vars are injected 
$ kubectl exec dobby -it -- sh
$ env

# change a value in dobby-config-map.yaml and apply it
# access the pod and show that corresponding env var value did not change
# explain that env var will not change beyond when the pod first start up

# go through pod descriptor file regarding the env
# note that it references the full configmap
$ kubectl apply -f dobby-full-configmap-as-env.yaml

# access the pod to show that all env vars are injected 
$ kubectl exec dobby -it -- sh
$ env
```

### Create ConfigMaps as Vol Mounts
```s
# create configmap from file instead of via descriptor yaml
$ kubectl create configmap test-config-from-file --from-file test.properties
$ kubectl describe configmap test-config-from-file

# go through pod descriptor file 
$ kubectl apply -f test-pod-configmap-as-volume.yaml

# access the pod to see that the config file is mounted on the specified path
kubectl exec test-pod -it -- sh
cd /etc/config
cat test.properties

# change a value in test.properties
# updating configmap is tricky if created from file
# have to create and replace the original configmap
$ kubectl create configmap test-config-from-file --from-file test.properties -o yaml --dry-run=client | kubectl replace -f -

# access the pod and show that corresponding value did change
# it may take up to a few seconds for the volume to sync
kubectl exec test-pod -it -- sh
cd /etc/config
cat test.properties
```




