# Services

# create a 3 separate terminal split pane with watch on pods
```s
$ watch kubectl get pods -o wide        #00f7ff
$ watch kubectl get svc                 #69f675
$ watch kubectl get endpoints           #ffeb00
```

### ClusterIP

```s
$ $ kubectl apply -f ../04-replicasets/dobby-rs.yaml
$ kubectl apply -f dobby-svc.yaml
$ kubectl describe svc dobbysvc
$ kubectl describe endpoints dobbysvc

# increase # of replicas to 3, followed by 4 and upto 6
$ kubectl apply -f ../04-replicasets/dobby-rs.yaml
$ kubectl describe svc dobbysvc
$ kubectl describe endpoints dobbysvc

# to access via ClusterIP from Node and access dobby in loop and observe request going to different pods
$ colima ssh
$ while true; do sleep 2; curl "http://{CLUSTER_IP}:4444/meta"; echo -e '    '$(date);done

# to access via DNS name, get inside containers
$ kubectl apply -f ../03-pods/hello-node.yaml
$ kubectl exec hello-node -it -- sh
$ wget -qO- dobbysvc:4444/meta
$ while true; do sleep 2; wget -qO- "http://dobbysvc:4444/meta"; echo -e '    '$(date);done
$ ping dobbysvc

```

### NodePort
```s
$ kubectl apply -f ../04-replicasets/dobby-rs.yaml
$ kubectl apply -f dobby-svc-nodeport.yaml
# Can use localhost or can use colima ip (192.168.106.10) id network is enabled
$ while true; do sleep 2; curl "localhost:30003/meta"; echo -e '    '$(date);done
# run on another node using same port
```