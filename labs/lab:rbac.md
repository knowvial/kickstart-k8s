# Role based access control
> Create a service account
```yaml
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: mysa
automountServiceAccountToken: false
EOF
```
> Create a pod that we want to control access to
```yaml
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: test-sa-pod
spec:
  serviceAccountName: mysa
  automountServiceAccountToken: false
  containers:
  - name: test-sa-pod-cn
    image: busybox
    command: ['sh', '-c', 'echo "Hello, Kubernetes!" && sleep 3600']
  restartPolicy: OnFailure
EOF
```

> Create a role
```yaml
cat <<EOF | kubectl apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: sa-rb-pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
EOF
```

> Create a role binding
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: sa-rb
  namespace: default
subjects:
- kind: ServiceAccount
  name: mysa
  namespace: default
roleRef:
  kind: Role
  name: sa-rb-pod-reader
  apiGroup: rbac.authorization.k8s.io
```

> Get the private token for the service account 
```
TOKEN=$(kubectl describe secrets "$(kubectl describe serviceaccount default -n default| grep -i Tokens | awk '{print $2}')" -n default | grep token: | awk '{print $2}')
```

> Add the token to ~/.kube/config