Got tired of looking these up every few months. Pulled them into one list, every value cross-checked against kubernetes.io and etcd.io.

Pods per node: 110

Nodes per cluster: 5,000

Total pods per cluster: 150,000

Total containers per cluster: 300,000

etcd request size: 1.5 MiB

etcd default DB size: 2 GB (8 GB suggested max)

Secret size: 1 MiB

ConfigMap data: 1 MiB

Annotations total per object: 256 KiB (262,144 bytes)

Label/annotation key name: 63 chars max

Label value: 63 chars max

Annotation/label key prefix: 253 chars (DNS subdomain)

Object name (DNS subdomain rule): 253 chars max

Object name (DNS label rule): 63 chars max

NodePort range: 30000 to 32767

Default Service CIDR (kubeadm): 10.96.0.0/12

terminationGracePeriodSeconds: 30s

Eviction hard memory.available: 100Mi

Eviction hard nodefs.available: 10%

Eviction hard nodefs.inodesFree: 5%

Eviction hard imagefs.available: 15%

PodPidsLimit: -1 (unlimited per pod by default)

Kubelet API port: 10250

etcd client port: 2379-2380

kube-apiserver port: 6443

A few things that vary and aren't captured above:

Pods per node on managed services overrides the upstream default. EKS ties it to ENI capacity per instance type (often much lower than 110), GKE Standard goes up to 256, AKS depends on CNI mode.

The 1 MiB ConfigMap/Secret cap is enforced by the apiserver. etcd's own per-request cap is 1.5 MiB, which is why annotations on a large object can push the whole thing over.

DNS subdomain (253) vs DNS label (63) depends on the resource. Pods use subdomain rules, Services use label rules.

OpenShift sets PodPidsLimit to 4096 by default instead of upstream's -1.