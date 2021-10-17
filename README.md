# lazy-pulling-estargz

Image size comparison:

```
felipecruz/node13-fn latest 43ff022f9166 About an hour ago 961MB

estargz.kontain.me/felipecruz/node13-estargz-fn latest bbbee27d75de 2 hours ago 974MB
```

```
kind create cluster --name stargz-demo --image ghcr.io/stargz-containers/estargz-kind-node:0.7.0
arkade install openfaas

# Forward the gateway to your machine

kubectl rollout status -n openfaas deploy/gateway
kubectl port-forward -n openfaas svc/gateway 8080:8080 &

# If basic auth is enabled, you can now log into your gateway:

PASSWORD=$(kubectl get secret -n openfaas basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode; echo)
echo -n $PASSWORD | faas-cli login --username admin --password-stdin

```

```

faas-cli template pull https://github.com/felipecruz91/templates

faas-cli new node13-estargz-fn --lang node13-estargz
sed -i '' 's/node13-fn:latest/felipecruz\/node13-fn:latest/g' node13-fn.yml
faas-cli up -f node13-fn.yml

# Convert pushed image into eStargz on the registry-side, using estargz.kontain.me.

docker pull estargz.kontain.me/felipecruz/node13-estargz-fn:latest

sed -i '' 's/node13-estargz-fn:latest/estargz.kontain.me\/felipecruz\/node13-estargz-fn:latest/g' node13-estargz-fn.yml
faas-cli deploy -f node13-estargz-fn.yml
kubectl scale --replicas=0 deployment node13-estargz-fn -n openfaas-fn
time curl http://127.0.0.1:8080/function/node13-estargz-fn

```

```

faas-cli new node13-fn --lang node13
sed -i '' 's/node13-fn:latest/felipecruz\/node13-fn:latest/g' node13-fn.yml
faas-cli up -f node13-fn.yml
kubectl scale --replicas=0 deployment node13-fn -n openfaas-fn
time curl http://127.0.0.1:8080/function/node13-fn

```

```

kind delete cluster --name stargz-demo

```

```

```
