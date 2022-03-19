# kubevirt-minio-backup-test


## k3d clster

install k3d --> https://k3d.io/v5.3.0/#installation

k3d cluster create --config k3d/config

export KUBECONFIG=$(k3d kubeconfig write minio-cluster)