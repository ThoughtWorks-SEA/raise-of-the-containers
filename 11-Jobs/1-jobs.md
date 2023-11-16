# Jobs

### Test Job
```s
# split plane to watch pods & jobs
watch kubectl get pods -o wide
watch kubectl get jobs

# go through descriptor file regarding task being run
kubectl apply -f job.yaml

# show that the pods are in completed state

# show that the pods are still around to check logs
kubectl logs test-job

# delete job and note that pods are deleted
kubectl delete jobs test-job

# uncomment ttlSecondsAfterFinished and apply again
kubectl apply -f job.yaml

# show that the pods and jobs get deleted
```

### Parallel Job
```s
# go through descriptor file regarding parallelism and completions fields
kubectl apply -f parallel-job.yaml

# note that only 3 pods are created first to run in parallel
# before the last pod is created to complete 4 tasks
```

### Cron Job
```s
# go through descriptor file regarding schedule syntax
# the job will run every minute 
# refer to the schedule syntax https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/#schedule-syntax 
kubectl apply -f cron-job.yaml
```