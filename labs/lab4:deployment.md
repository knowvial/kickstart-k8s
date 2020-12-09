# Creating and Exploring Deployments and Replicasets
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

## Imperative way: create a deployment 
> Run the following command to create a deployment
```
kubectl create deployment my-web-dep --image=nginx
```

> Explore deployments
```
kubectl get deploy
kubectl get deploy -o wide
kubectl describe deploy my-web-dep
kubectl get deploy -n kube-system
kubectl get pods -o wide
```
> Install command line browser. Get "pod-ip-address" from `kubectl get pods -o wide` command.
```
sudo apt-get install lynx
lynx http://<pod-ip-address>:80
```

> Delete deployment
```
kubectl delete deploy my-web-dep
```

## Declarative way: create a deployment 
> Create a deployment manifest file
```
cat <<EOF > dep.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-dep
  labels:
    app: k8s-class
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-web-app
  template:
    metadata:
      labels:
        app: my-web-app
    spec:
      containers:
      - name: ui
        image: nginx
        ports:
        - containerPort: 80
EOF
```

> Create deployment
```
kubectl apply -f dep.yml
```

> Explore deployment
```
kubectl get deploy -o wide
```

> Delete deployment
```
kubectl delete deploy my-web-dep
```

## Replicasets
> Run the following commands to explore Replicasets. 
> Get "pod-name" from `kubectl get pods` command.
```
kubectl get replicasets
kubectl get rs,deploy,pods -o wide
kubectl delete pod <pod-name>
kubectl get rs,deploy,pods -o wide
```