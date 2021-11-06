# promscale-demo
Use tobs to set up and try out a simple observability stack.

Used the process described here for install: https://github.com/timescale/tobs

check it's version:
```shell
❯ tobs version
Tobs CLI Version: 0.7.0, latest tobs helm chart version: 0.7.0 
```
Create a kind cluster with cluster.yaml
```
❯ kind create cluster --config=cluster.yaml
Creating cluster "kind" ...
 ✓ Ensuring node image (kindest/node:v1.21.1) 🖼
 ✓ Preparing nodes 📦 📦 📦 📦  
 ✓ Writing configuration 📜 
 ✓ Starting control-plane 🕹️ 
 ✓ Installing CNI 🔌 
 ✓ Installing StorageClass 💾 
 ✓ Joining worker nodes 🚜 
Set kubectl context to "kind-kind"
You can now use your cluster with:

kubectl cluster-info --context kind-kind

Have a question, bug, or feature request? Let us know! https://kind.sigs.k8s.io/#community 🙂
```
Set the config values properly to turn on tracing and deploy into the monitoring namespace by editing the values.yaml file.
```shell
tobs helm show-values > values.yaml
```
Create the monitoring namespace
```shell
❯ k create namespace monitoring
namespace/monitoring created
```
Install the observability stack
```shell
❯ tobs install --tracing

WARNING: Using a generated self-signed certificate for TLS access to TimescaleDB.
         This should only be used for development and demonstration purposes.
         To use a signed certificate, use the "--tls-timescaledb-cert" and "--tls-timescaledb-key"
         flags when issuing the tobs install command.

Creating TimescaleDB tobs-certificate secret
Creating TimescaleDB tobs-credentials secret
skipping to create TimescaleDB s3 backup secret as backup option is disabled.
Couldn't find the cert-manager, Installing the cert-manager...
confirm the action by typing y or yes and press enter: y
Waiting on pod cert-manager-7d59dd4888-7ncg8...
Pod cert-manager-7d59dd4888-7ncg8 has started
Waiting on pod cert-manager-cainjector-85899d45d9-92tl4...
Pod cert-manager-cainjector-85899d45d9-92tl4 has started
Waiting on pod cert-manager-webhook-84fcdcd5d-2qrjn...
Pod cert-manager-webhook-84fcdcd5d-2qrjn has started
Successfully created cert-manager
Installing The Observability Stack
2021/11/06 15:08:08 release installed successfully: tobs/tobs-0.7.0
Waiting for helm install to complete...
Waiting for pods to initialize...
Waiting on pod tobs-kube-prometheus-operator-f88d85bc7-p6f45...
Pod tobs-kube-prometheus-operator-f88d85bc7-p6f45 has started
Waiting on pod tobs-prometheus-node-exporter-24g7n...
Pod tobs-prometheus-node-exporter-24g7n has started
Waiting on pod tobs-prometheus-node-exporter-8d25n...
Pod tobs-prometheus-node-exporter-8d25n has started
Waiting on pod tobs-prometheus-node-exporter-gnk5s...
Pod tobs-prometheus-node-exporter-gnk5s has started
Waiting on pod tobs-prometheus-node-exporter-rqqpp...
Pod tobs-prometheus-node-exporter-rqqpp has started
Waiting on pod tobs-timescaledb-0...
Pod tobs-timescaledb-0 has started
Waiting on pod tobs-grafana-69446b86c8-zdb8m...
Pod tobs-grafana-69446b86c8-zdb8m has started
Waiting on pod tobs-kube-prometheus-operator-f88d85bc7-p6f45...
Pod tobs-kube-prometheus-operator-f88d85bc7-p6f45 has started
Waiting on pod tobs-kube-state-metrics-f48d7588b-bmktf...
Pod tobs-kube-state-metrics-f48d7588b-bmktf has started
Waiting on pod tobs-promscale-8bf9587d7-gfk8q...
Pod tobs-promscale-8bf9587d7-gfk8q has started
Waiting on pod tobs-grafana-db-9dwfp...
Pod tobs-grafana-db-9dwfp has started
Waiting on pod opentelemetry-operator-controller-manager-7bff64d9fd-w55dz...
Pod opentelemetry-operator-controller-manager-7bff64d9fd-w55dz has started
###############################################################################
👋🏽 Welcome to tobs, The Observability Stack for Kubernetes

✨ Auto-configured and deployed:
🔥 Kube-Prometheus
🐯 TimescaleDB
🤝 Promscale
🧐 PromLens
📈 Grafana
🚀 OpenTelemetry
🎯 Jaeger

###############################################################################
🔥 PROMETHEUS NOTES:
###############################################################################

Prometheus can be accessed via port 9090 on the following DNS name from within your cluster:
tobs-kube-prometheus-prometheus.default.svc.cluster.local

Get the Prometheus server URL by running these commands in the same shell:
  tobs prometheus port-forward


The Prometheus alertmanager can be accessed via port 9093 on the following DNS name from within your cluster:
tobs-kube-prometheus-alertmanager.default.svc.cluster.local


Get the Alertmanager URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace default -l "app=alertmanager,alertmanager=tobs-kube-prometheus-alertmanager" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace default port-forward $POD_NAME 9093
WARNING! Persistence is disabled on AlertManager.
         You will lose your data when the AlertManager pod is terminated.

###############################################################################
🐯 TIMESCALEDB NOTES:
###############################################################################

TimescaleDB can be accessed via port 5432 on the following DNS name from within your cluster:
tobs.default.svc.cluster.local

To get your password for superuser run:
    tobs timescaledb get-password -U <user>

To connect to your database, chose one of these options:

1. Run a postgres pod and connect using the psql cli:
    tobs timescaledb connect -U <user>

2. Directly execute a psql session on the master node
   tobs timescaledb connect -m


###############################################################################
🧐 PROMLENS NOTES:
###############################################################################
   PromLens is a PromQL query builder, analyzer, and visualizer.

   You can access PromLens via a local browser by executing:
    tobs promlens port-forward

   Then you can point your browser to http://127.0.0.1:8081/.
###############################################################################
🚀  OPENTELEMETRY NOTES:
###############################################################################

    The OpenTelemetry collector is deployed to collect traces.

    OpenTelemetry collector can be accessed with the following DNS name from within your cluster:
    tobs-opentelemetry-collector.default.svc.cluster.local
###############################################################################
🎯 JAEGER NOTES:
###############################################################################

    Jaeger visualizes traces.

    You can access Jaeger via a local browser by executing:
    tobs jaeger port-forward

    Then you can point your browser to http://127.0.0.1:16686/.
###############################################################################
📈 GRAFANA NOTES:
###############################################################################

1. The Grafana server can be accessed via port 80 on
   the following DNS name from within your cluster:
   tobs-grafana.default.svc.cluster.local

   You can access grafana locally by executing:
    tobs grafana port-forward

   Then you can point your browser to http://127.0.0.1:8080/.

2. The 'admin' user password can be retrieved by:
    tobs grafana get-password

3. You can reset the admin user password with grafana-cli from inside the pod.
    tobs grafana change-password <password-you-want-to-set>

🚀 Happy observing!
The Observability Stack has been installed successfully
```
Have a look at what was created
```shell
❯ kgaa
NAMESPACE                       NAME                                                             READY   STATUS      RESTARTS   AGE
cert-manager                    pod/cert-manager-7d59dd4888-7ncg8                                1/1     Running     0          8m24s
cert-manager                    pod/cert-manager-cainjector-85899d45d9-92tl4                     1/1     Running     0          8m24s
cert-manager                    pod/cert-manager-webhook-84fcdcd5d-2qrjn                         1/1     Running     0          8m23s
default                         pod/alertmanager-tobs-kube-prometheus-alertmanager-0             2/2     Running     0          7m36s
default                         pod/prometheus-tobs-kube-prometheus-prometheus-0                 2/2     Running     0          7m36s
default                         pod/tobs-grafana-69446b86c8-zdb8m                                2/2     Running     3          7m56s
default                         pod/tobs-grafana-db-9dwfp                                        0/1     Completed   4          7m56s
default                         pod/tobs-jaeger-promscale-query-5cdc5c58df-mtq7j                 1/1     Running     1          7m56s
default                         pod/tobs-kube-prometheus-operator-f88d85bc7-p6f45                1/1     Running     0          7m56s
default                         pod/tobs-kube-state-metrics-f48d7588b-bmktf                      1/1     Running     0          7m56s
default                         pod/tobs-opentelemetry-collector-6b847f479d-hccz2                1/1     Running     0          6m14s
default                         pod/tobs-prometheus-node-exporter-24g7n                          1/1     Running     0          7m56s
default                         pod/tobs-prometheus-node-exporter-8d25n                          1/1     Running     0          7m56s
default                         pod/tobs-prometheus-node-exporter-gnk5s                          1/1     Running     0          7m56s
default                         pod/tobs-prometheus-node-exporter-rqqpp                          1/1     Running     0          7m56s
default                         pod/tobs-promlens-7f778cc958-zdtkw                               1/1     Running     0          7m56s
default                         pod/tobs-promscale-8bf9587d7-gfk8q                               1/1     Running     4          7m56s
default                         pod/tobs-timescaledb-0                                           1/1     Running     0          7m56s
kube-system                     pod/coredns-558bd4d5db-k7h5k                                     1/1     Running     0          9m23s
kube-system                     pod/coredns-558bd4d5db-n9nk4                                     1/1     Running     0          9m23s
kube-system                     pod/etcd-kind-control-plane                                      1/1     Running     0          9m24s
kube-system                     pod/kindnet-7qpwv                                                1/1     Running     0          9m23s
kube-system                     pod/kindnet-d7sc7                                                1/1     Running     0          9m8s
kube-system                     pod/kindnet-mqxpj                                                1/1     Running     0          9m8s
kube-system                     pod/kindnet-wq59b                                                1/1     Running     0          9m8s
kube-system                     pod/kube-apiserver-kind-control-plane                            1/1     Running     0          9m24s
kube-system                     pod/kube-controller-manager-kind-control-plane                   1/1     Running     0          9m37s
kube-system                     pod/kube-proxy-cm9g5                                             1/1     Running     0          9m8s
kube-system                     pod/kube-proxy-kmfv9                                             1/1     Running     0          9m8s
kube-system                     pod/kube-proxy-tm9tx                                             1/1     Running     0          9m23s
kube-system                     pod/kube-proxy-wdm9l                                             1/1     Running     0          9m8s
kube-system                     pod/kube-scheduler-kind-control-plane                            1/1     Running     0          9m24s
local-path-storage              pod/local-path-provisioner-547f784dff-znmvc                      1/1     Running     0          9m23s
opentelemetry-operator-system   pod/opentelemetry-operator-controller-manager-7bff64d9fd-w55dz   2/2     Running     0          7m56s

NAMESPACE                       NAME                                                                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                  AGE
cert-manager                    service/cert-manager                                                ClusterIP   10.96.93.231    <none>        9402/TCP                                 8m24s
cert-manager                    service/cert-manager-webhook                                        ClusterIP   10.96.208.164   <none>        443/TCP                                  8m24s
default                         service/alertmanager-operated                                       ClusterIP   None            <none>        9093/TCP,9094/TCP,9094/UDP               7m37s
default                         service/kubernetes                                                  ClusterIP   10.96.0.1       <none>        443/TCP                                  9m37s
default                         service/prometheus-operated                                         ClusterIP   None            <none>        9090/TCP                                 7m37s
default                         service/tobs                                                        ClusterIP   10.96.177.92    <none>        5432/TCP                                 7m56s
default                         service/tobs-config                                                 ClusterIP   None            <none>        8008/TCP                                 7m57s
default                         service/tobs-grafana                                                ClusterIP   10.96.235.109   <none>        80/TCP                                   7m57s
default                         service/tobs-jaeger-promscale                                       ClusterIP   10.96.166.139   <none>        16686/TCP                                7m57s
default                         service/tobs-kube-prometheus-alertmanager                           ClusterIP   10.96.49.207    <none>        9093/TCP                                 7m57s
default                         service/tobs-kube-prometheus-operator                               ClusterIP   10.96.181.18    <none>        443/TCP                                  7m56s
default                         service/tobs-kube-prometheus-prometheus                             ClusterIP   10.96.216.108   <none>        9090/TCP                                 7m57s
default                         service/tobs-kube-state-metrics                                     ClusterIP   10.96.232.149   <none>        8080/TCP                                 7m56s
default                         service/tobs-opentelemetry-collector                                ClusterIP   10.96.193.61    <none>        14250/TCP,14268/TCP,4317/TCP,55681/TCP   6m14s
default                         service/tobs-opentelemetry-collector-headless                       ClusterIP   None            <none>        14250/TCP,14268/TCP,4317/TCP,55681/TCP   6m14s
default                         service/tobs-opentelemetry-collector-monitoring                     ClusterIP   10.96.248.234   <none>        8888/TCP                                 6m14s
default                         service/tobs-prometheus-node-exporter                               ClusterIP   10.96.78.184    <none>        9100/TCP                                 7m57s
default                         service/tobs-promlens                                               ClusterIP   10.96.88.27     <none>        80/TCP                                   7m57s
default                         service/tobs-promscale-connector                                    ClusterIP   10.96.186.84    <none>        9201/TCP,9202/TCP                        7m57s
default                         service/tobs-replica                                                ClusterIP   10.96.130.209   <none>        5432/TCP                                 7m57s
kube-system                     service/kube-dns                                                    ClusterIP   10.96.0.10      <none>        53/UDP,53/TCP,9153/TCP                   9m35s
kube-system                     service/tobs-kube-prometheus-coredns                                ClusterIP   None            <none>        9153/TCP                                 7m57s
kube-system                     service/tobs-kube-prometheus-kube-controller-manager                ClusterIP   None            <none>        10252/TCP                                7m57s
kube-system                     service/tobs-kube-prometheus-kube-etcd                              ClusterIP   None            <none>        2379/TCP                                 7m57s
kube-system                     service/tobs-kube-prometheus-kube-proxy                             ClusterIP   None            <none>        10249/TCP                                7m57s
kube-system                     service/tobs-kube-prometheus-kube-scheduler                         ClusterIP   None            <none>        10251/TCP                                7m57s
kube-system                     service/tobs-kube-prometheus-kubelet                                ClusterIP   None            <none>        10250/TCP,10255/TCP,4194/TCP             7m37s
opentelemetry-operator-system   service/opentelemetry-operator-controller-manager-metrics-service   ClusterIP   10.96.178.26    <none>        8443/TCP                                 7m57s
opentelemetry-operator-system   service/opentelemetry-operator-webhook-service                      ClusterIP   10.96.251.114   <none>        443/TCP                                  7m56s

NAMESPACE     NAME                                           DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
default       daemonset.apps/tobs-prometheus-node-exporter   4         4         4       4            4           <none>                   7m56s
kube-system   daemonset.apps/kindnet                         4         4         4       4            4           <none>                   9m34s
kube-system   daemonset.apps/kube-proxy                      4         4         4       4            4           kubernetes.io/os=linux   9m35s

NAMESPACE                       NAME                                                        READY   UP-TO-DATE   AVAILABLE   AGE
cert-manager                    deployment.apps/cert-manager                                1/1     1            1           8m24s
cert-manager                    deployment.apps/cert-manager-cainjector                     1/1     1            1           8m24s
cert-manager                    deployment.apps/cert-manager-webhook                        1/1     1            1           8m24s
default                         deployment.apps/tobs-grafana                                1/1     1            1           7m56s
default                         deployment.apps/tobs-jaeger-promscale-query                 1/1     1            1           7m56s
default                         deployment.apps/tobs-kube-prometheus-operator               1/1     1            1           7m56s
default                         deployment.apps/tobs-kube-state-metrics                     1/1     1            1           7m56s
default                         deployment.apps/tobs-opentelemetry-collector                1/1     1            1           6m14s
default                         deployment.apps/tobs-promlens                               1/1     1            1           7m56s
default                         deployment.apps/tobs-promscale                              1/1     1            1           7m56s
kube-system                     deployment.apps/coredns                                     2/2     2            2           9m35s
local-path-storage              deployment.apps/local-path-provisioner                      1/1     1            1           9m34s
opentelemetry-operator-system   deployment.apps/opentelemetry-operator-controller-manager   1/1     1            1           7m56s

NAMESPACE                       NAME                                                                   DESIRED   CURRENT   READY   AGE
cert-manager                    replicaset.apps/cert-manager-7d59dd4888                                1         1         1       8m24s
cert-manager                    replicaset.apps/cert-manager-cainjector-85899d45d9                     1         1         1       8m24s
cert-manager                    replicaset.apps/cert-manager-webhook-84fcdcd5d                         1         1         1       8m24s
default                         replicaset.apps/tobs-grafana-69446b86c8                                1         1         1       7m56s
default                         replicaset.apps/tobs-jaeger-promscale-query-5cdc5c58df                 1         1         1       7m56s
default                         replicaset.apps/tobs-kube-prometheus-operator-f88d85bc7                1         1         1       7m56s
default                         replicaset.apps/tobs-kube-state-metrics-f48d7588b                      1         1         1       7m56s
default                         replicaset.apps/tobs-opentelemetry-collector-6b847f479d                1         1         1       6m14s
default                         replicaset.apps/tobs-promlens-7f778cc958                               1         1         1       7m56s
default                         replicaset.apps/tobs-promscale-8bf9587d7                               1         1         1       7m56s
kube-system                     replicaset.apps/coredns-558bd4d5db                                     2         2         2       9m23s
local-path-storage              replicaset.apps/local-path-provisioner-547f784dff                      1         1         1       9m23s
opentelemetry-operator-system   replicaset.apps/opentelemetry-operator-controller-manager-7bff64d9fd   1         1         1       7m56s

NAMESPACE   NAME                                                              READY   AGE
default     statefulset.apps/alertmanager-tobs-kube-prometheus-alertmanager   1/1     7m37s
default     statefulset.apps/prometheus-tobs-kube-prometheus-prometheus       1/1     7m37s
default     statefulset.apps/tobs-timescaledb                                 1/1     7m56s

NAMESPACE   NAME                        COMPLETIONS   DURATION   AGE
default     job.batch/tobs-grafana-db   1/1           2m27s      7m56s
```
