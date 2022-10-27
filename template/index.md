---
layout: page
title: Template Files
---

Use these files to control the output of resources

# [secret](/template/secret)

```sh
kubectl get secret/database -o go-template="$(curl https://k8sh8.com/template/secret)"
```

# [cert](/template/cert)

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/cert)" | openssl x509 -text
```

# [key](/template/key)

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/key)" | openssl rsa -text
```
