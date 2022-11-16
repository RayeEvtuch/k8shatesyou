---
layout: default
---

{% raw %}

# Troubleshooting

## Get all abnormal pods

```sh
kubectl get pods --field-selector "status.phase!=Running,status.phase!=Succeeded"
```

## Connect to a service on a local port

```sh
kubectl port-forward service/blog 12345:http
```

## Get a shell on a temporary pod

```sh
kubectl run -it --rm --image alpine tmp -- /bin/sh
```

## Add a temporary container to a deployment and get a shell

```sh
kubectl patch deployment/backend -p '{"spec":{"template":{"spec":{"containers":[{"name":"tmp-container","image":"alpine","std-in":true,"tty":true}]}}}}'
kubectl exec -it deployment/backend -c tmp-container -- /bin/sh
```

```sh
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/alpine)"
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/busybox)"
```

## Add a temporary pod that mounts a PersistentVolumeClaim

```sh
kubectl run -it --rm --image=alpine tmp --override-type 'strategic' --overrides '{"spec":{"containers":[{"name":"tmp","volumeMounts":[{"name":"v","mountPath":"/mnt"}]}],"volumes":[{"name":"v","persistentVolumeClaim":{"claimName":"database"}}]}}' -- /bin/sh
```

# Blunt Force

## Remove resource limitations from a deployment

```sh
kubectl set resources deployment/backend --limits 'memory=0' --requests 'memory=0'
```

## Restart all pods for a deployment

```sh
kubectl rollout restart deployment/backend
```

## Terminate a specific container within a pod

```sh
kubectl exec backend -c database -- /bin/sh -c "kill 1"
```

## Skip finalizers for a namespace stuck in Terminating

```sh
kubectl patch ns/blog -p '{"spec":{"finalizers":[]}}' --dry-run client -o json | kubectl replace --raw "/api/v1/namespaces/blog/finalize" -f -
```

# Labels & Annotations

## Remove an annotation

```sh
kubectl annotate deployment/backend meta.helm.sh/release-name-
```

# Information

## Get all objects of all types in a namespace

```sh
kubectl -n blog get $(kubectl api-resources --namespaced true --verbs get -o name | tr '\n' ',')pods
```

## Get all non-namespaced objects in the cluster

```sh
kubectl get $(kubectl api-resources --namespaced false --verbs get -o name | tr '\n' ',')nodes
```

# Secrets

## Print a secret's decoded values

```sh
kubectl get secret/database -o go-template='{{ range $k,$v := .data }}{{ $k }}: {{ $v | base64decode }}{{ "\n" }}{{ end }}'
```

```sh
kubectl get secret/database -o go-template="$(curl https://k8sh8.com/template/secret)"
```

# Certificates

## Force cert-manager certificate renewal

```sh
kubectl patch certificate/frontend-certificate --subresource status --type merge -p '{"status":{"conditions":[{"type":"Issuing","status":"True"}]}}'
```

```sh
kubectl patch certificate/frontend-certificate --subresource status --type merge -p "$(curl https://k8sh8.com/patch/renew)"
```

## Get certificate information

```sh
kubectl get secret/frontend-certificate -o go-template='{{ index .data "tls.crt" | base64decode }}' | openssl x509 -text
```

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/cert)" | openssl x509 -text
```

## Get private key information

```sh
kubectl get secret/frontend-certificate -o go-template='{{ index .data "tls.key" | base64decode }}' | openssl rsa -text
```

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/key)" | openssl rsa -text
```

# Helm

## Force Helm ownership of an object

```sh
kubectl annotate deployment/frontend --overwrite app.kubernetes.io/managed-by=Helm meta.helm.sh/release-name=blog meta.helm.sh/release-namespace=blog
```

{% endraw  %}
