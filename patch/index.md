---
layout: page
title: Patch Files
---

Use these files to quickly patch resources

# [alpine](/patch/alpine)

Add a temporary container to a deployment

```sh
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/alpine)"
```

# [busybox](/patch/busybox)

Add a temporary container to a deployment

```sh
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/busybox)"
```
