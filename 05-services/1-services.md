# Services

### Create a 3 separate terminal split pane
```s
$ watch kubectl get pods -o wide      
$ watch kubectl get svc               
$ watch kubectl get endpoints 

# note that kubernetes creates a default service and endpoint for the apiserver
# this can be ignore for the demo
```

### ClusterIP
```s
# go through descriptor file regarding selectors
# note that no endpoints are created as no pods are created yet
$ kubectl apply -f dobby-svc.yaml

# go through descriptor file regarding labels
$ kubectl apply -f dobby-rs.yaml

# note that after the pods are created, the endpoints are created

# explain that ClusterIP can be accessed only from within the cluster
# hence need to ssh into Node to access dobby service
# run while loop to observe request going to different pods as service does loadbalancing
$ colima ssh
$ while true; do sleep 2; curl "http://{CLUSTER_IP}:4444/meta"; echo -e '    '$(date);done
```

### NodePort
```s
# delete the clusterIP type dobby service
$ kubectl delete svc dobby-svc

# note that replicaset won't be deleted as they are not bound like to the service

# go through descriptor file regarding ports
$ kubectl apply -f dobby-svc-nodeport.yaml

# explain that NodePort exposes services to access outside of the cluster
# hence can use localhost or can use colima ip (by running `colima list`) if --network-address is enabled
$ while true; do sleep 2; curl "localhost:30003/meta"; echo -e '    '$(date);done
```

### DNS name (coredns pods)	
```s
# create a curl pod that would be removed upon exit the container
$ kubectl run curl -it --rm --image=pstauffer/curl

# call the service via its name rather than the ip
$ curl http://dobby-svc-np:4444/meta

# note that the pod was able to resolve the name
# this is because of coredns pod, which acts as a DNS server

# show that the curl pod is pointing to a nameserver IP
$ cat /etc/resolv.conf

# show that the nameserver IP is the cluster IP of kube-dns service
$ kubectl get service -n kube-system

# show that the kube-dns service forwards traffic to some endpoints
$ kubectl get endpoints -n kube-system

# show that the endpoints correspond to the coredns pod IP
$ kubectl get pods -n kube-system -o wide
```