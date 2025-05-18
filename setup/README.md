# Host Setup

## OS Install

Install Raspberry Pi OS Lite 64-bit.

In imager:
- Create user + pass
- Enable SSH
- Add public key:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC5IZfqS7X0JBCqu7mtImO/qqQpmw8kY725CAQbvKARbQKVFTo6pf2HXknWFcK6JyryilvxysWvJtBxDD6kJxz2QKWrs2HQFiy/fJ4cdMJybvnvwwFMCtSqpAsBwSAPXdOVF/wxiBfvYvQ1xOHKLdrUd7qQlS2zvtOCVHNdkgHW+lVb5sP7KCyVOu7tcA6xxepY0n02+pde7uJtZzhXIUUOzyc8efOA511a/5qpOfluvPKfiH8cGlHLMT20X93ocHHS5k/lgTKpFmkC6XrVQ6gEHuL52PC0PV/LCyhh9e+x48yQhsqsLE6dSngnjns20TwRZfoJEA4IiRQIOZt6IFkvN0jTjMDc+uSlFFiMVlGP8KMCZ+mqivy4PaNsGWvikx5AGXSMrTMPHGNNzzEgZ3CqOhOZNahb/i1aDEaicuq6JQo0vBYRaxeEuufPx/QXOy4kCBcjwghyYxVI2pk07XVtkaQpDilzf695xeTzC9yrz7koV8pLqBzZ7ZrRlav2Hbc= samuel@samuel-xps
```

## Packages

Install longhorn deps:
```
sudo apt-get install -y nfs-common open-iscsi util-linux
```

## Networking

- Assign static IP in router
- Add to DNS server

## Storage

Create `/storage01` directory.

## Kernel Params

Append `cgroup_memory=1 cgroup_enable=memory` to `/boot/firmware/cmdline.txt`.

# k3s


## Master

```
# install k3s
curl -sfL https://get.k3s.io | sh -s - --write-kubeconfig-mode 644 --disable servicelb --bind-address 192.168.1.150 --disable-cloud-controller --disable local-storage

# get auth token
sudo cat /var/lib/rancher/k3s/server/node-token
```

## Worker

```
curl -sfL https://get.k3s.io | K3S_URL="https://<MASTER_IP>:6443" K3S_TOKEN="<TOKEN>" sh -
```

# ArgoCD

```
kubectl apply -k setup/argocd
```

Add repo credentials *as template*.
