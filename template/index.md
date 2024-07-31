---
layout: page
title: Template Files
---

Use these files to control the output of resources

# [secret](/template/secret)

```sh
kubectl get secret/database -o go-template="$(curl https://k8sh8.com/template/secret)"
```

```jinja
{% raw %}{{ range $k,$v := .data }}{{ $k }}: {{ $v | base64decode }}{{ "\n" }}{{ end }}{% endraw %}
```

# [cert](/template/cert)

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/cert)" | openssl x509 -text
```

```jinja
{% raw %}{{ index .data "tls.crt" | base64decode }}{% endraw %}
```

# [key](/template/key)

```sh
kubectl get secret/frontend-certificate -o go-template="$(curl https://k8sh8.com/template/key)" | openssl rsa -text
```

```jinja
{% raw %}{{ index .data "tls.key" | base64decode }}{% endraw %}
```
