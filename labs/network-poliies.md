# Restricting access across namespaces
> Create a namespace
```
kubectl create ns np-test
```
> Create target pod that needs to be secured
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: np-nginx
  namespace: np-test
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx
EOF
```
> Create client pod that needs to access the above pod
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: np-busybox
  namespace: np-test
  labels:
    app: client
spec:
  containers:
  - name: busybox
    image: radial/busyboxplus:curl
    command: ['sh', '-c', 'while true; do sleep 5; done']
EOF
```
> Check if both pods are created correctly and if you are able to ping one pod from another.
```
kubectl get pods -n np-test -o wide
NGINX_IP=<np-nginx Pod IP>
kubectl exec -n np-test np-busybox -- curl $NGINX_IP
```

> Cretae a network policy that blocks all access to the target pod.
```
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-networkpolicy
  namespace: np-test
spec:
  podSelector:
    matchLabels:
      app: nginx
  policyTypes:
  - Ingress
  - Egress
EOF
```

> Create a label for namespace. A namespace label will be used to apply network policy. 
```
kubectl label namespace np-test team=np-test
```

> Add ingress and egress to allow the right permissions
```
cat <<EOF | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: my-networkpolicy
  namespace: np-test
spec:
  podSelector:
    matchLabels:
      app: nginx
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          team: np-test
    ports:
    - port: 80
      protocol: TCP
EOF
```
> Now check if the communication is enabled with the new network policy
```
kubectl get pods -n np-test -o wide
NGINX_IP=<np-nginx Pod IP>
kubectl exec -n np-test np-busybox -- curl $NGINX_IP
```


---
> Following is an example with blocking, allowing, ingress and egress
``` 
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: test-network-policy
  namespace: default
spec:
  podSelector:
    matchLabels:
      role: db
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: myproject
    - podSelector:
        matchLabels:
          role: frontend
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```