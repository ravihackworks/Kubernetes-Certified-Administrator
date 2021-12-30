## Disclaimer
  These are the questions based on the experience. There is no guarantee that these questions would come in you exam as well :)
  But It is strongly recommended that you must do hands-on for these questions before sitting in your CKA exam.

## Question : 1

  Use context: **kubectl config use-context k8s-etcd-context**
  Create snapshot of the etcd running at https://127.0.0.1:2379 save snapshot into  /srv/data/etcd/etcd-snapshot.db

  Also, restore the backup from /opt/etcd-snapshot.db.

**Solution :**

1. kubectl config use-context k8s-etcd-context
2. ETCDCTL_API=3 etcdctl --endpoints 127.0.0.1:2379 --cacert=/opt/ca.crt  --cert=/opt/etcd-client.crt --key=/opt/etcd-client.key snapshot save /srv/data/etcd/etcd-snapshot.db
3. ETCDCTL_API=3 etcdctl --endpoints 127.0.0.1:2379 --cacert=/opt/ca.crt  --cert=/opt/etcd-client.crt --key=/opt/etcd-client.key snapshot restore snapshotdb
4. Link for K8S documentation - https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/#backing-up-an-etcd-cluster

**Note:**
1. Don't forget to change the context. 
2. Path for certificates (like cacert, cert, and key) are already given in question.
3. Do not restore the snapshot that you have taken. In question path for snapshot db already given, so you need to restore from /opt/etcd-snapshot.db
4. It may also possible, they might asked you to take backup on specific node. So you need to do simply ssh <node_name> and then sudo -i. Then follow above steps mentioned in solution. 

## Question : 2

  Use context: **kubectl config use-context k8s-cluster3-context**
  Set the node name ek8s-node-1 as unavailable and reschedule the workload. 
  
**Solution :**

1. kubectl config use-context k8s-cluster3-context
2. kubectl drain node01 --ignore-daemonsets --delete-local-data

**Note:**
1. Kubernetes documentation -  https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
2. Kubenetes command arguments - https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#drain
  
## Question : 3

Use context: **kubectl config use-context k8s-cluster2-context**
Create  multi container  pod  kucc4 with below information : 
1. Container Name : nginx and image  name: nginx
2. Container Name: redis and image name: redis

**Solution :**

1. kubectl config use-context k8s-cluster2-context
2. Create below YAML - 
   apiVersion: v1
   kind: Pod
   metadata:
     name: kucc4 
   spec:
     containers:
     - name: nginx
       image: nginx
     - name: redis
       image: redis

**Note:**
1. Kubernetes documentation - https://kubernetes.io/docs/concepts/workloads/pods/#using-pods
2. You can generate pod template by kubectl run kucc4 --image=nginx --dry-run -o yaml > 3.yaml and then edit yaml as per need.


## Question : 4

  Use context: **kubectl config use-context k8s-cluster7-context**
  Schedule pod for node (disk = ssd)
    Name: nginx  
    image: nginx
    
**Solution :**   

1. kubectl config use-context k8s-cluster7-context
2. Create below pod specification
   apiVersion: v1
   kind: Pod
   metadata:
     name: nginx
   spec:
     containers:
     - name: nginx
       image: nginx
       imagePullPolicy: IfNotPresent
     nodeSelector:
       disk: ssd

**Note:**
  1. Kubernetes documentation - https://kubernetes.io/docs/concepts/configuration/assign-pod-node/#nodeselector 
  2. Label already mentioned on node, you don't required to add.
  3. kubectl apply -f <pod_spec.yaml>
  4. kubectl get pods -o wide. Now you may confirm that your pod must be in running condition on that node where label is same.
  5. To check label on node, kubectl get nodes <node_name> --show-labels

## Question : 5

  Use context: **kubectl config use-context k8s-cluster8-context**
  Scale the deploymet webserver to 5 pods
  
**Solution :**   
1. kubectl config use-context k8s-cluster8-context  
2. Check deployment is deployed or not by command - kubectl get deployments 
3. kubectl  scale --replicas=5 deployment webserver

**Note:**
  
  1. Kubernetes documentation - https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale
  2. Deployment webserver already deployed in your default namespace. Only you have to scale. 
