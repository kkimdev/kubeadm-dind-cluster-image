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

# TODO: Save the Docker images and volumes as files locally and load at the next startup
exit
```

```bash
# TODO: Update ENTRYPOINT using docker commit --change
docker commit -m "[build notes]" -a "creator info" kubeadm-dind-cluster-image kubeadm-dind-cluster-image:latest
docker run -it --rm --privileged kubeadm-dind-cluster-image /bin/bash
```