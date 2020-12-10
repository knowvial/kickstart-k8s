# Creating and Exploring Pods
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

## Imperative way: create a pod 

> Run the following command to create a Pod.
```
kubectl run nginx --image=nginx --port=80
```

> Explore and delete. Get "pod-name" from `kubectl get pods` command.
```
kubectl get pods
kubectl get pods -o wide
Kubectl logs <pod-name>
kubectl exec -it <pod-name> -- /bin/sh
kubectl describe pod <pod-name>
kubectl get pods <pod-name> -o yaml
```

> Delete the Pod
```
kubectl delete pod <pod-name>
```

## Declarative way: create a pod
> Create a pod manifest file.
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
```

> Create pod using the above manifest file.
```
kubectl apply -f pod.yml
```

> Explore
```
kubectl get pods -o wide
```

> Delete the Pod
```
kubectl delete pod <pod-name>
```
