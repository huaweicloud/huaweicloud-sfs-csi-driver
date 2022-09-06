# Huawei Cloud EVS CSI Plugin

EVS CSI Plugin provides the ability to interface with Huawei Cloud EVS storage resources.

Such as: create volume, attach volume, create snapshot, expand volume.

## Compatibility

For sidecar version compatibility, please refer compatibility matrix for each sidecar here 
-https://kubernetes-csi.github.io/docs/sidecar-containers.html.

## Support version

| CSI version   | EVS CSI Plugin Version | Kubernetes Version Tested | Features                |
|---------------|------------------------|---------------------------|-------------------------|
| v1.5.0        | v0.1.0                 | v1.20 v1.21 v1.22 v1.23   | volume resizer snapshot |

## Deploy

### Prerequisites

- docker, kubeadm, kubelet and kubectl has been installed.

### Steps

- Create the config file

The driver initialization depends on a [cloud config file](../../deploy/cloud-config). 
Make sure it's in `/etc/evs/cloud-config` on your master.

Click to view the detailed description of the file: [cloud-config](../cloud-config.md).

- Create secret resource

```
kubectl create secret -n kube-system generic cloud-config --from-file=/etc/evs/cloud-config
```

This should create a secret name `cloud-config` in `kube-system` namespace.

Once the secret is created, Controller Plugin and Node Plugin can be deployed using respective manifests.

- Create RBAC resources

```
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/rbac-csi-evs-controller.yaml
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/rbac-csi-evs-node.yaml
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/rbac-csi-evs-secret.yaml
```

- Install HuaweiCloud EVS CSI Plugin

```
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/csi-evs-driver.yaml
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/csi-evs-controller.yaml
kubectl apply -f https://raw.githubusercontent.com/huaweicloud/huaweicloud-csi-driver/master/deploy/evs-csi-plugin/kubernetes/csi-evs-node.yaml
```

- Waiting for all the pods in running

```
# kubectl get all -A
NAME                                   READY   STATUS    RESTARTS       AGE
...
csi-evs-plugin-bkkpb                   3/3     Running   0              3m22s
csi-evs-provisioner-54c44b746f-22p46   6/6     Running   0              88s
```

## How to use

The following are examples of specific functions:

**Dynamic Provisioning:** [sample app](evs-normal.md)

**Volume Expansion:** [sample app](evs-resize.md)

**Using Block Volume:** [sample app](evs-block.md)

**Volume Snapshots:** [sample app](evs-snapshot.md)

**Ephemeral Volume:** [sample app](evs-ephemeral.md)

**Topology:** [sample app](evs-topology.md)