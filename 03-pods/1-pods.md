# Pod commands

## Pod basic commands

create a separate terminal split pane with watch on pods
```s
$ watch kubectl get pods -o wide
```

### Applicaiton overview
1. Nginx                
2. Hello World NodeJS   (one that we used during Docker module)
3. Dobby                (built by TW Chennai team, Krishana and others)

```s
# Show demo on how to access dobby endpoints from nginx container

$ kubectl get pods
$ kubectl apply -f nginx.yaml
$ kubectl apply -f dobby.yaml

$ kubectl get pods -o wide
$ kubeclt describe pod dobby
$ kubectl logs dobby

$ Use port-forward to access the API endpoints of dobby
$ kubectl port-forward dobby 4444:4444
$ curl -i -X PUT localhost:4444/control/crash
# or Access from the Node
$ colima ssh
$ curl -i -X PUT 10.42.0.44:4444/control/crash

# notice STATUS and RESTART count in watch pane

# add ENV variable to POD and see the change
$ kubectl apply -f dobby-with-version.yaml
$ kubectl exec dobby -it -- bash
    $ echo $VERSION
```

### create multi container pod
```s
$ kubectl apply -f multi-container.yaml
# notice READY column 2/2 - where as other pods are 1/1
$ kubectl describe pod multi-container

# get into node
$ curl -i 172.17.0.7   # see the timestamp

# get into one of the container
$ kubectl exec multi-container -it -- bash   
# default to 1st
$ cat /usr/share/nginx/html/index.html

# get into one specific container 2nd
$ kubectl exec multi-container -c 2nd -it -- bash
$ cat /html/index.html

```