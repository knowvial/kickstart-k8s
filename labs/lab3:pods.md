# Creating and Exploring Pods
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

## Imperative way: create a pod 
```
kubectl run nginx --image=nginx --port=80
```

> Explore and delete
```
kubectl get pods
kubectl get pods -o wide
Kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
kubectl describe pod <pod-name>
kubectl get pods <pod-name> -o yaml
```

> Delete Pod
```
kubectl delete pod <pod-name>
```

## Declarative way: create a pod
```
cat <<EOF > pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
    ports:
    - containerPort: 80
EOF

kubectl apply -f pod.yml
```

> Explore and delete
```
kubectl get pods -o wide
```

> Delete Pod
```
kubectl delete pod <pod-name>
```
