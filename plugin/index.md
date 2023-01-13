---
layout: page
title: Plugin Files
---

{% raw %}

Add these files to your PATH to be able to use them as kubectl plugins

# [decode](/plugin/kubectl-decode)

```sh
kubectl decode secret/database
```

```sh
#! /bin/sh

kubectl get $@ -o go-template='{{ range $k,$v := .data }}{{ "====\n== " }}{{ $k }}{{ "\n====\n" }}{{ $v | base64decode }}{{ "\n" }}{{ end }}'
```

# [cert](/plugin/kubectl-cert)

```sh
kubectl cert secret/frontend-certificate
```

```sh
#! /bin/sh

kubectl get $@ -o go-template='{{ index .data "tls.crt" | base64decode }}' | openssl x509 -text
```

# [key](/plugin/kubectl-key)

```sh
kubectl key secret/frontend-certificate
```

```sh
#! /bin/sh

KEY="$(kubectl get $@ -o go-template='{{ index .data "tls.key" | base64decode }}')"
if expr "$KEY" : "-----BEGIN RSA PRIVATE KEY-----$" > /dev/null; then
    echo "$KEY" | openssl rsa -text
elif expr "$KEY" : "-----BEGIN EC PRIVATE KEY-----$" > /dev/null; then
    echo "$KEY" | openssl ec -text
else
    echo "$KEY"
fi
```

{% endraw  %}
