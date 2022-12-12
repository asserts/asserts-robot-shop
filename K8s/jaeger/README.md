# Jaeger Tracing

For the Jaeger Tracing backend deploy the file in this directory to your cluster.

```sh
$ kubectl apply -f jaeger.yaml
```

All components will be installed in the `monitoring` namespace.

Deploy the Robot Shop application via Helm, setting `jaeger.enabled=true`.

```sh
helm install robot-shop /path/to/chart \
    ... \
    --set jaeger.enabled=true
    ... \
```
