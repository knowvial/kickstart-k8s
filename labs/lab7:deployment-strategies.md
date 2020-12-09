# Deployment Strategies
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

## Rolling Updates 
> Create a deployment and scale to 4 replicas.
```
kubectl create deployment my-web-dep --image=nginx:1.18.0 
kubectl scale deployment/my-web-dep --replicas=4
```

> Let's try upgrading nginx - but with a wrong version
```
kubectl set image deployment/my-web-dep nginx=nginx:broken --record
kubectl rollout status deployment/my-web-dep
```

> Check the rollout status and rollback
```
kubectl rollout history deployment/my-web-dep
kubectl rollout undo deployment/my-web-dep
```

> Upgrade and rollaback to a specific version
```
kubectl set image deployment/my-web-dep nginx=nginx:1.19.5 --record

kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'

kubectl rollout undo deployment/my-web-dep --to-revision=0
```

> Pause and resume
```
kubectl rollout pause deployment/my-web-dep

kubectl get pods -o jsonpath --template='{range .items[*]}{.metadata.name}{"\t"}{"\t"}{.spec.containers[0].image}{"\n"}{end}'

kubectl rollout resume deployment/my-web-dep
```