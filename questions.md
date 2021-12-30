**Question : 1**

  Use context: **kubectl config use-context k8s-etcd-context**
  Create snapshot of the etcd running at https://127.0.0.1:2379 save snapshot into  /srv/data/etcd/etcd-snapshot.db

  Also, restore the backup from /opt/etcd-snapshot.db.

**Solution : **

1. kubectl config use-context k8s-etcd-context
2. ETCDCTL_API=3 etcdctl --endpoints 127.0.0.1:2379 --cacert=/opt/ca.crt  --cert=/opt/etcd-client.crt --key=/opt/etcd-client.key snapshot save /srv/data/etcd/etcd-snapshot.db
3. ETCDCTL_API=3 etcdctl --endpoints 127.0.0.1:2379 --cacert=/opt/ca.crt  --cert=/opt/etcd-client.crt --key=/opt/etcd-client.key snapshot restore snapshotdb
4. Link for K8S documentation - https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster

**Note:**
1. Don't forget to change the context. 
2. Path for certificates (like cacert, cert, and key) are already given in question.
3. Do not restore the snapshot that you have taken. In question path for snapshot db already given, so you need to restore from /opt/etcd-snapshot.db
