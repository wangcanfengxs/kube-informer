# kube-informer

# build/test
```
CGO_ENABLED=0 GOOS=linux go build -o bin/kube-informer -ldflags '-s -w' cmd/*.go
bin/kube-informer -h
bin/kube-informer --watch=apiVersion=v1,kind=Pod -- env

bin/kube-informer --watch=apiVersion=v1,kind=Pod --pass-args -- echo
bin/kube-informer --watch=apiVersion=v1,kind=Pod --selector='example=true' --pass-stdin -- jq .
bin/kube-informer --watch=apiVersion=v1,kind=Pod --selector='example=true' --field-selector='status.phase=Running' --pass-stdin -- jq .

bin/kube-informer --watch=apiVersion=v1,kind=ConfigMap --watch=apiVersion=v1,kind=Secret -- env
bin/kube-informer --watch=apiVersion=v1,kind=ConfigMap:apiVersion=v1,kind=Secret -- env

bin/kube-informer --watch=apiVersion=v1,kind=Pod --leader-elect=endpoints/kube-informer -- env

docker run -it --rm -v /root:/root -v $PWD/bin/kube-informer:/usr/bin/kube-informer debian:8 \
kube-informer --watch apiVersion=v1,kind=ConfigMap --leader-elect=configmaps/kube-informer -- \
bash -c 'sleep 1.5s & sleep 1s && echo $INFORMER_EVENT $INFORMER_OBJECT_NAMESPACE.$INFORMER_OBJECT_NAME'

```

# docker image
```
docker run -it --rm -v /root:/root xiaopal/kube-informer --watch apiVersion=v1,kind=Pod -- bash -c 'echo $INFORMER_EVENT $INFORMER_OBJECT_NAMESPACE.$INFORMER_OBJECT_NAME'
```