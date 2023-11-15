# Pod commands

### Create Pod
```s
# create a separate terminal split pane for demo
$ watch kubectl get pods -o wide

# go through the descriptor file
# explain the status changes
$ kubectl apply -f dobby.yaml

# explain the different events
$ kubeclt describe pod dobby
```

### Pod Lifecycle
```s
# tail the logs in a separate pane
$ kubectl logs -f dobby

# trigger pod to restart by calling dobby app's /control/crash endpoint
# either use port-forwarding
$ kubectl port-forward dobby 4444:4444
$ curl -i -X PUT localhost:4444/control/crash
# or ssh into colima node
$ colima ssh
$ curl -i -X PUT 10.42.0.44:4444/control/crash

# notice STATUS and RESTART count, and logs
```

### Manage Pod (Env Var)
```s
# delete dobby pod first as changing configuration
$ kubectl delete pod dobby

# add ENV variable to pod descriptor yaml
$ kubectl apply -f dobby-with-version.yaml

# show how to exec into the pod
$ kubectl exec dobby -it -- bash
    $ echo $VERSION
```

### Multi container Pod (Shared Volume)
```s
# go through the descriptor file regarding volume mounts
# in particular, go through the command that appends date
$ kubectl apply -f multi-container.yaml

# notice READY column 2/2 - whereas other pods are 1/1

# show that two containers are displayed
$ kubectl describe pod multi-container

# get into node
$ colima ssh
# show the timestamp
$ date

# get into one of the container
$ kubectl exec multi-container -it -- bash   
# default to 1st
$ cat /usr/share/nginx/html/index.html

# get into one specific container 2nd
$ kubectl exec multi-container -c 2nd -it -- bash
$ cat /html/index.html
```