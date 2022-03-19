


kubectl minio init 

kubectl create ns minio-tenant-2

kubectl minio tenant create minio-tenant-2       \
--servers                 3                  \
--volumes                 6                   \
--capacity                10Gi          \
--storage-class           local-path    \
--namespace               minio-tenant-2 \
--output > tenant.yaml 

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout minio-tls.key -out minio-tls.cert -subj "/CN=127.0.0.1.minio.nip.io/O=127.0.0.1.minio.nip.io"
kubectl create secret tls nginx-tls --key  minio-tls.key --cert minio-tls.cert -n minio-tenant-2

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout console-tls.key -out console-tls.cert -subj "/CN=127.0.0.1.console.nip.io/O=127.0.0.1.console.nip.io"
kubectl create secret tls nginx-tls-console --key  console-tls.key --cert console-tls.cert -n minio-tenant-2

via port-forwarding:
kubectl port-forward service/minio-tenant-2-console 9443:9443
kubectl port-forward service/minio 6443:443
https://localhost:6443