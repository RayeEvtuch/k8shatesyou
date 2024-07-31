---
layout: page
title: Patch Files
---

Use these files to quickly patch resources

# [alpine](/patch/alpine)

Add a temporary container to a deployment

```sh
kubectl patch deployment/backend -p "$(curl https://k8sh8.com/patch/alpine)"
```

```json
{% include_relative /alpine %}
```

# [busybox](/patch/busybox)

Add a temporary container to a deployment

```sh
kubectl patch deployment/backend -p "$(curl https://k8sh8.com/patch/busybox)"
```

```json
{% include_relative /busybox %}
```

# [renew](/patch/renew)

Force cert-manager certificate renewal

```sh
kubectl patch certificate/frontend-certificate --subresource status --type=merge -p "$(curl https://k8sh8.com/patch/renew)"
```

```json
{% include_relative /renew %}
```
