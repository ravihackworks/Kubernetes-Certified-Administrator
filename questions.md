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

## Question : 6

Use context: **kubectl config use-context k8s-cluster8-context**
There is a node that is  not ready state. You have to identify the cause of failure, then fix the problem.
Also verify node is now in ready state.

**Solution :**  
1. kubectl config use-context **k8s-cluster8-context**  
2. ssh k8s-node01
3. sudo -i
4. Check all k8s components are up and running on this node.
5. Problem is kubelet is not running. (Check systemctl status kubelet)
6. systemctl enable kubelet
7. systemctl daemon-reload
8.  systemctl restart kubelet
9.  Now check status for kubelet again (systemctl status kubelet)
10.  exit from node
11.  Now check kubectl get nodes

**Note:**
1. There was no issue in kubelet configuration. 
2. Only issue was kubelet not running.

## Question : 7
In this question, you have to identify the high cpu pod name which are having label **name=cpu-utilizer** and then save into file /opt/KUTR00401/KUTROO401.txt.
**Solution :**  
kubectl top pods -l name=cpu-utilizer --sort-by=cpu --no-headers | cut -f1 -d" "" | head -n1 > /opt/KUTR00401/KUTROO401.txt

## Question : 8
List the noSchedule taints for cluster nodes and count the nodes.
**Solution :**  
kubectl get nodes -o jsonpath="{range.items[*]} {.metadata.name} {.spec.taints[].effect} {\"\n\"}" | grep -v NoSchedule | wc -l

## Question : 9
Create a new ClusteRole name deployment-clusterrole , which only allows to create the following resources types - deployment,statefulSet,Daemonset
Create a new ServiceAccount named cicd-tocken in the existing namespace app-team
bind the new clisterrole deplyment-clusterrole to the new service account cicd-tocken limited to the namespace app-team
**Solution :**
kubectl create clusterrole deployment-clusterrole --verb=create --resource=Deployment,StatefulSet,DaemonSet
kubectl create sa cicd-tocken --namespace=app-team
kubectl create clusterrolebinding deployment-bind --clusterrole=deployment-clusterrole --serviceaccount=app-team:cicd-tocken
**Note:**
1. Kubernetes documentation - https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
2. Kubernetes imperative command - https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrole-em-

