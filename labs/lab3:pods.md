# Creatinng and Exploring Pods
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.

> Imperative way: create a pod 
```
kubectl run nginx --image=nginx --port=80
```

> Explore
```
kubectl get pods
kubectl get pods -o wide
Kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
kubectl describe pod <pod-name>
kubectl get pods <pod-name> -o yaml
kubectl delete pod <pod-name>
```

> Declarative way: create a pod
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
kubectl delete pod <pod-name>
```
