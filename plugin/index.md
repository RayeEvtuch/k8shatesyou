---
layout: page
title: Plugin Files
---

{% raw %}

Add these files to your PATH to be able to use them as kubectl plugins

# [decode](/plugin/decode)

```sh
kubectl decode secret/database
```

```sh
#! /bin/sh

kubectl get $@ -o go-template='{{ range $k,$v := .data }}{{ "====\n== " }}{{ $k }}{{ "\n====\n" }}{{ $v | base64decode }}{{ "\n" }}{{ end }}'
```

{% endraw  %}
