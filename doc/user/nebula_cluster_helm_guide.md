# Install Nebula Cluster with helm

Please install [nebula-operator](install_guide.md) before installing Nebula Cluster.

### Get Repo Info

```shell script
# If you have already added it, please skip.
$ helm repo add nebula-operator https://vesoft-inc.github.io/nebula-operator/charts
$ helm repo update
```

_See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation._

### Install with helm

```shell script
export NEBULA_CLUSTER_NAME=nebula         # the name for nebula cluster
export NEBULA_CLUSTER_NAMESPACE=nebula    # the namespace you want to install the nebula cluster
export STORAGE_CLASS_NAME=gp2             # the storage class for the nebula cluster

$ kubectl create namespace "${NEBULA_CLUSTER_NAMESPACE}" # If you have already created it, please skip.
$ helm install "${NEBULA_CLUSTER_NAME}" nebula-operator/nebula-cluster \
    --namespace="${NEBULA_CLUSTER_NAMESPACE}" \
    --set nameOverride=${NEBULA_CLUSTER_NAME} \
    --set nebula.storageClassName="${STORAGE_CLASS_NAME}"

# Please wait a while for the cluster to be ready.
$ kubectl -n "${NEBULA_CLUSTER_NAMESPACE}" get pod -l "app.kubernetes.io/cluster=${NEBULA_CLUSTER_NAME}"
NAME                READY   STATUS    RESTARTS   AGE
nebula-graphd-0     1/1     Running   0          5m34s
nebula-graphd-1     1/1     Running   0          5m34s
nebula-metad-0      1/1     Running   0          5m34s
nebula-metad-1      1/1     Running   0          5m34s
nebula-metad-2      1/1     Running   0          5m34s
nebula-storaged-0   1/1     Running   0          5m34s
nebula-storaged-1   1/1     Running   0          5m34s
nebula-storaged-2   1/1     Running   0          5m34s
```

### Upgrade with helm

```shell
$ helm upgrade "${NEBULA_CLUSTER_NAME}" nebula-operator/nebula-cluster \
    --namespace="${NEBULA_CLUSTER_NAMESPACE}" \
    --set nameOverride=${NEBULA_CLUSTER_NAME} \
    --set nebula.storageClassName="${STORAGE_CLASS_NAME}" \
    --set nebula.storaged.replicas=5

# Please wait a while for the cluster to be ready.
$ kubectl -n "${NEBULA_CLUSTER_NAMESPACE}" get pod -l "app.kubernetes.io/cluster=${NEBULA_CLUSTER_NAME}"
NAME                READY   STATUS    RESTARTS   AGE
nebula-graphd-0     1/1     Running   0          10m
nebula-graphd-1     1/1     Running   0          10m
nebula-metad-0      1/1     Running   0          10m
nebula-metad-1      1/1     Running   0          10m
nebula-metad-2      1/1     Running   0          10m
nebula-storaged-0   1/1     Running   0          10m
nebula-storaged-1   1/1     Running   0          10m
nebula-storaged-2   1/1     Running   0          10m
nebula-storaged-3   1/1     Running   0          56s
nebula-storaged-4   1/1     Running   0          56s
```

### Uninstall with helm

```shell
$ helm uninstall "${NEBULA_CLUSTER_NAME}" --namespace="${NEBULA_CLUSTER_NAMESPACE}"
```

### Optional: chart parameters

The following table lists is the configurable parameters of the chart and their default values.

| Parameter | Description | Default |
|:---------|:-----------|:-------|
| `nameOverride` | Override the name of the chart | `nil` |
| `nebula.version` | Nebula version | `v2.6.1` |
| `nebula.imagePullPolicy` | Nebula image pull policy | `Always` |
| `nebula.storageClassName` | PersistentVolume class, default to use the default StorageClass | `nil` |
| `nebula.schedulerName` | Scheduler for nebula component | `default-scheduler` |
| `nebula.reference` | Reference for nebula component | `{"name": "statefulsets.apps", "version": "v1"}` |
| `nebula.graphd.image` | Graphd container image without tag, and use `nebula.version` as tag | `vesoft/nebula-graphd` |
| `nebula.graphd.replicas` | Graphd replica number | `2` |
| `nebula.graphd.env` | Graphd env | `[]` |
| `nebula.graphd.resources` | Graphd resources | `{"resources":{"requests":{"cpu":"500m","memory":"500Mi"},"limits":{"cpu":"1","memory":"1Gi"}}}`|
| `nebula.graphd.logStorage` | Graphd log volume size | `500Mi` |
| `nebula.graphd.podLabels` | Graphd pod labels | `{}` |
| `nebula.graphd.podAnnotations` | Graphd pod annotations | `{}` |
| `nebula.graphd.nodeSelector` | Graphd nodeSelector | `{}` |
| `nebula.graphd.tolerations` | Graphd pod tolerations | `{}` |
| `nebula.graphd.affinity` | Graphd affinity | `{}` |
| `nebula.graphd.readinessProbe` | Graphd pod readinessProbe | `{}` |
| `nebula.graphd.sidecarContainers` | Graphd pod sidecarContainers | `{}` |
| `nebula.graphd.sidecarVolumes` | Graphd pod sidecarVolumes | `{}` |
| `nebula.metad.image` | Metad container image without tag, and use `nebula.version` as tag | `vesoft/nebula-metad` |
| `nebula.metad.replicas` | Metad replica number | `3` |
| `nebula.metad.env` | Metad env | `[]` |
| `nebula.metad.resources` | Metad resources | `{"resources":{"requests":{"cpu":"500m","memory":"500Mi"},"limits":{"cpu":"1","memory":"1Gi"}}}`|
| `nebula.metad.logStorage` | Metad log volume size | `500Mi` |
| `nebula.metad.dataStorage` | Metad data volume size | `1Gi` |
| `nebula.metad.podLabels` | Metad pod labels | `{}` |
| `nebula.metad.podAnnotations` | Metad pod annotations | `{}` |
| `nebula.metad.nodeSelector` | Metad nodeSelector | `{}` |
| `nebula.metad.tolerations` | Metad pod tolerations | `{}` |
| `nebula.metad.affinity` | Metad affinity | `{}` |
| `nebula.metad.readinessProbe` | Metad pod readinessProbe | `{}` |
| `nebula.metad.sidecarContainers` | Metad pod sidecarContainers | `{}` |
| `nebula.metad.sidecarVolumes` | Metad pod sidecarVolumes | `{}` |
| `nebula.storaged.image` | Storaged container image without tag, and use `nebula.version` as tag | `vesoft/nebula-storaged` |
| `nebula.storaged.replicas` | Storaged replica number | `3` |
| `nebula.storaged.env` | Storaged env | `[]` |
| `nebula.storaged.resources` | Storaged resources | `{"resources":{"requests":{"cpu":"500m","memory":"500Mi"},"limits":{"cpu":"1","memory":"1Gi"}}}`|
| `nebula.storaged.logStorage` | Storaged log volume size | `500Mi` |
| `nebula.storaged.dataStorage` | Storaged data volume size | `1Gi` |
| `nebula.storaged.podLabels` | Storaged pod labels | `{}` |
| `nebula.storaged.podAnnotations` | Storaged pod annotations | `{}` |
| `nebula.storaged.nodeSelector` | Storaged nodeSelector | `{}` |
| `nebula.storaged.tolerations` | Storaged pod tolerations | `{}` |
| `nebula.storaged.affinity` | Storaged affinity | `{}` |
| `nebula.storaged.readinessProbe` | Storaged pod readinessProbe | `{}` |
| `nebula.storaged.sidecarContainers` | Storaged pod sidecarContainers | `{}` |
| `nebula.storaged.sidecarVolumes` | Storaged pod sidecarVolumes | `{}` |
| `imagePullSecrets` | The secret to use for pulling the images | `[]`  |
