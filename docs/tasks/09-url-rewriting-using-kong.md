# URL rewriting using Kong

« [Understand what kubectl does behind](08-understand-what-kubectl-does.md) | [Main Page](../../README.md) | Put all things together »

---

## Apply Kong request-transformer plugin

To redirect URL endpoint from /apis/cache.example.com/v1alpha1 to /apis/cache2.example.com/v1alpha1, let's use the samples/kong-url-rewriting.yaml file.

See how it looks:

```shell
cat samples/kong-url-rewriting.yaml
```

It includes an ingress defined specific to /apis/cache.example.com, and a Kong plugin called request-transformer, which is used to define the rewrite rule.

Let's apply it:

```shell
kubectl apply -f samples/kong-url-rewriting.yaml
```
<!--
sleep 3
-->

## Test and verify

To verify it, let's get the HTTPs node port for Kong proxy:

```shell
PROXY_HTTPS_NODEPORT=$(kubectl get svc example-kong-kong-proxy -o jsonpath="{.spec.ports[1].nodePort}")
echo $PROXY_HTTPS_NODEPORT
```

Then call /apis/cache.example.com/v1alpha1 and /apis/cache2.example.com/v1alpha1:

```shell
curl -s -i -k https://127.0.0.1:$PROXY_HTTPS_NODEPORT/apis/cache.example.com/v1alpha1
curl -s -i -k https://127.0.0.1:$PROXY_HTTPS_NODEPORT/apis/cache2.example.com/v1alpha1
```

They should return the same response.