# helmfile_readfile_issue

## Issue Description

The new version of Helmfile (`v1.0.0-rc.0`) escapes backslashes in JSON files when rendering templates. This behavior is undesirable for example when adding dashboard JSON files to Grafana, as it modifies the content by escaping double quotes within the JSON strings.

## Steps to Reproduce

### Version 0.163.1

```shell
helmfile_0.163.1_linux_amd64/helmfile template -f helmfile.yaml.gotmpl
```

The rendered output will be:

```yaml
apiVersion: v1
data:
  testjson: '{ "test": "test{\"escapecheck\"}" } '
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/instance: jsonescapingtest
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: jsonescapingtest-raw
    app.kubernetes.io/version: 1.0.0
    helm.sh/chart: raw-2.0.0
  name: jsonescapingtest
```

### Version 1.0.0-rc0

``` shell
helmfile_1.0.0-rc.0_linux_amd64/helmfile template -f helmfile.yaml.gotmpl
```

The rendered output will be:

``` yaml
apiVersion: v1
data:
  testjson: '{ "test": "test{\\"escapecheck\\"}" } '
kind: ConfigMap
metadata:
  labels:
    app.kubernetes.io/instance: jsonescapingtest
    app.kubernetes.io/managed-by: Helm
    app.kubernetes.io/name: jsonescapingtest-raw
    app.kubernetes.io/version: 1.0.0
    helm.sh/chart: raw-2.0.0
  name: jsonescapingtest
```
