# Jobs
- processing tasks that only need to be executed once (not periodic)

```bash
kubectl get pods -n demo-namespace -l job=demo-job -w
# open another terminal window:
kubectl apply -f 13-job.yaml
# status is initially Pending, then Running, and finally, after processing the command, Completed
kubectl delete job demo-job
```

# CronJobs
- periodic tasks that need to be executed at specific times
- command date; echo Hello from Kubernetes CronJob every minute
- The CronJob creates a Job that executes the command and terminates after completion
- The CronJob is then executed again at the next scheduled time.

```bash
kubectl get pods -l job-name=demo-cron -w
# open another terminal window:
kubectl apply -f 13-cronjob.yaml
```

- Job is executed every minute. With the previously executed kubectl get command

# ┌───────────── Minute (0 - 59)
# │ ┌───────────── Hour (0 - 23)
# │ │ ┌───────────── Day of month (1 - 31)
# │ │ │ ┌───────────── Month (1 - 12)
# │ │ │ │ ┌───────────── Day of week (0 - 6) (Sunday to Saturday)
# * * *  

```bash
kubectl delete cronjob demo-cron
```
