---
layout: page
title: Patch Files
---

Use these files to quickly patch resources

# [alpine](/patch/alpine)

```sh
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/alpine)"
```

# [busybox](/patch/busybox)

```sh
kubectl patch deployment/backend -p="$(curl https://k8sh8.com/patch/busybox)"
```
