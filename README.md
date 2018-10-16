# kubeadm-dind-cluster-image

```bash
docker run -it --name kubeadm-dind-cluster-image --privileged docker:dind /bin/sh
```

```bash
dockerd-entrypoint.sh &
apk add bash curl --no-cache
curl https://cdn.rawgit.com/kubernetes-sigs/kubeadm-dind-cluster/master/fixed/dind-cluster-v1.11.sh -o /usr/local/bin/dind-cluster
chmod +x /usr/local/bin/dind-cluster
APISERVER_PORT=8080 dind-cluster up
dind-cluster down

docker save $(docker images -q) -o ~/docker-images.tar
# TODO: Save the Docker volumes and load at the next startup
# https://github.com/moby/moby/issues/32263
exit
```

```bash
# TODO: Update ENTRYPOINT using docker commit --change
docker commit -m "[build notes]" -a "creator info" kubeadm-dind-cluster-image kubeadm-dind-cluster-image:latest
docker run -it --rm --privileged kubeadm-dind-cluster-image /bin/bash
```

```bash
dockerd-entrypoint.sh &
# TODO: Load back the image with proper name & tags
# https://stackoverflow.com/questions/35575674/how-to-save-all-docker-images-and-copy-to-another-machine
docker load -i ~/docker-images.tar
```




```bash
# TODO: Maybe try backing up containers directly?
docker export kube-master -o kube-master.gz
docker export kube-node-1 -o kube-node-1.gz
docker export kube-node-2 -o kube-node-2.gz

dind-cluster down

docker import kube-master.gz
docker import kube-node-1.gz
docker import kube-node-2.gz
```