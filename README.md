# GitOps

GitOpsのサンプルです。

## ArgoCD

```
ARGOHOST=$([ -e /.dockerenv ] && echo $(ip route | head -1 | cut -d' ' -f 3)|| echo localhost)
argocd login --insecure --username admin --password $(make pwd) $ARGOHOST:9090
```

## 設定

### リリース

```
NAME=nginx
kubectl create namespace $NAME
argocd app create $NAME \
  --repo https://github.com/ksaito1125/gitops-samples.git \
  --path $NAME \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace $NAME
```

### ステージング

```
BRANCH=develop
DEVNAME=$NAME-$BRANCH
kubectl create namespace $DEVNAME
argocd app create $DEVNAME \
  --repo https://github.com/ksaito1125/gitops-samples.git \
  --path $NAME \
  --revision $BRANCH \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace $DEVNAME
```


## MEMO

```
kubectl run nginx --image=nginx --restart=Never --dry-run -o yaml
```

```
kubectl -n nginx-test expose deployment nginx --port 80
```

## 参考

[Argo CD - Declarative GitOps CD for Kubernetes](https://argoproj.github.io/argo-cd/)
[Argo CDによる継続的デリバリーのベストプラクティスとその実装](https://blog.cybozu.io/entry/2019/11/21/100000)
