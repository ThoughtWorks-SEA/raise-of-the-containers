# Secrets

### passwords
```s
# either show create secret from commandline
kubectl create secret generic mysecret --from-literal=password=mypassword

# or show create secret from file
kubectl create secret generic mysecret --from-file=password=./password.txt
echo -n "mypassword" | base64
kubectl apply -f secret.yaml

# show that secret is base64 encoded
# echo "<password string>" | base64 -D to decode
kubectl get secret mysecret -o yaml

# go through descriptor file regarding secret being mounted as volume
# and secret being injected as environment var
kubectl apply -f test-pod-secret.yaml

# either printenv to see it being injected as env var
# or go to path to see secret being mounted
kubectl exec test-pod -it -- sh
```




