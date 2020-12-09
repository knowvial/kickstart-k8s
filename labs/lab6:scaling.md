# Scaling deployments
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

## Create a deployment to scale 
> Create a deployment manifest with 2 replicas
```
cat <<EOF > dep-scale.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app
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
kubectl apply -f dep-scale.yml
```

## Imperative way: Manual Scaling out and in 
> Run the following command to scale out replicas from 2 to 10
```
kubectl get rs,pods -o wide
kubectl scale deployment/my-web-app --replicas=10
kubectl get rs -o wide -w
```

> Run the following command to scale in replicas from 10 to 2
```
kubectl scale deployment/my-web-app --replicas=2
kubectl get pods -o wide -w
```

## Declarative way: Manual Scaling out and in
> Create a service manifest file
```
cat <<EOF > dep-manual-scaling.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app
  labels:
    app: k8s-class
spec:
  replicas: 10
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

> Scale up with manifest file
```
kubectl apply -f dep-manual-scaling
```

> Delete deployment
```
kubectl delete deploy my-web-dep
```

--- 
# Autoscaling
![Autoscaling](images/lab6-1.png)
## Install Metrics Server
```
minikube addons list
minikube addons enable metrics-server

git clone https://github.com/linuxacademy/metrics-server.git
kubectl create -f metrics-server/deploy/1.8+/

kubectl convert -f https://raw.githubusercontent.com/linuxacademy/metrics-server/master/deploy/1.8%2B/metrics-server-deployment.yaml | kubectl create -f -

kubectl top pods
```
## Pod resource requests and limits
> Check node resources available
> User "node-name" from `kubectl get nodes` command
> Check for the CPU and memory available on the node
```
kubectl get nodes
kubectl describe node <node-name>
```

> Create deployment with resource requests and limits
```
cat <<EOF > dep-auto-scale.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-web-app
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
        resources:
          requests:
            memory: "200Mi"
            cpu: "100m"
          limits:
            memory: "400Mi"
            cpu: "200m"
EOF
```

> Imperative - Horizontal pod autoscaling
```
kubectl get pods -o wide
kubectl autoscale deployment.v1.apps/my-web-auto --min=1 --max=10 --cpu-percent=20
kubectl get hpa
```

> Innstall seige for load testing our web app
```
sudo apt install siege

```