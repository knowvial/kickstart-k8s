# Creating and Exploring Pods
> Go to https://www.katacoda.com/courses/kubernetes/launch-single-node-cluster. Enter the following commands in the terminal.
Please use CTRL+click (on Windows and Linux) or CMD+click (on MacOS) to open the following links in a new browser tab.

> Imperative way: create a deployment 
```
kubectl create deployment my-web-dep --image=nginx
```

> Explore deployments
```
kubectl get deploy
kubectl get deploy -o wide
kubectl describe deploy <deploy-name>
kubectl get deploy -n kube-system
kubectl get pods -o wide
```
> Install command line browser
```
sudo apt-get install lynx
lynx http://<pod-ip-address>:80
```

> Delete deployment
```
kubectl delete deploy my-web-dep
```
