# Volumnes


## emptyDir

```s
$ kubectl create -f empty-dir.yaml

# create 2 verticle split pane for showcase 2 container commands side by side

$ kubectl exec empty-dir -c c1 -it -- bash
$ tail -f /usr/share/nginx/html/index.html

$ kubectl exec empty-dir -c c2 -it -- bash
$ tail -f /html/index.html

$ kubectl delete pod empty-dir

# apply again to showcase all data gone after restart
$ kubectl create -f empty-dir.yaml
$ kubectl exec empty-dir -c c1 -it -- bash
$ tail -f /usr/share/nginx/html/index.html
```


## hostPath

# check if directly exists on host machine or not

```s
$ kubectl create -f host-path.yaml
$ kubectl describe pod host-path
# notice failture as the directory on host machine does not exists

$ kubectl delete pod host-path

# create directly on node

$ kubectl create -f host-path.yaml
$ kubectl describe pod host-path

# on host machine
$ cd /home/vagrant/nginx/html
$ tail -f index.html


# delete and recreate pod to be lucky on same machine
$ kubectl delete pod host-path
$ kubectl apply -f host-path.yaml

```

# PV & PVC

```s
$ kubectl get pv
$ kubectl get pvc

# /home/vagrant/k8svolumes/mypv
# goto each node and create folder and file
$ echo "Colima" > index.html

$ kubectl apply -f pvc.yaml
$ kubectl apply -f pv.yaml
$ kubectl get pv

$ kubectl apply -f pod.yaml
$ kubectl port-forward pod/mywebserver 8080:80
# browser http://localhost:8080/
$ kubectl get pv
$ kubectl get pvc


$ kubectl delete pod mywebserver
$ kubectl get pv
$ kubectl get pvc

# delete all to go in reverse order
$ kubectl delete pod mywebserver
$ kubectl delete pvc mypvc
$ kubectl delete pv mypv3

# try and create PVC first
$ kubectl apply -f pvc.yaml
# Notice Pending status as there is no PV available
$ kubectl apply -f pv.yaml
# request fullfilled

#Try deleting pv first it would't because of Finalizers
k delete pv
kubectl patch pv mypv3 -p '{"metadata": {"finalizers": null}}'
k get pv
k get pvc 
k delete pvc mypvc

```

# Storage Class
```s
cd storage-class
$ kubectl apply -f pvc-sc.yaml
$ kubectl get sc
$ kubectl describe pv pvc
```

# digital ocean
```s


```


