How to explicitly tell kubernetes scheduler to schedule a pod on the node we are interested in?

1. Using Nodename
2. Using Node Selectors
3. Node/Pod  Affinity
4. Tanes

 Using Nodenames we can tell kubernetes scheduler to schedule the pods on the specified node.
 
   <img width="204" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/8ef7b8c6-b594-4f94-97cd-0662c81d3b74">
  Here the pods will be scheduled in node 3.

Using Node Selectors we specify labels to nodes and we tell to deploy the pods to the nodes having corresponding labels.

##### kubectl  label node minikube-m03 minikube-m02 team=analytics

Using this command we can add team=analytics label to nodes  minikube-m02 , m03 In pod definition using node Selector we can deploy the pod to nodes having
team analytics label.

<img width="209" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/2dfbc11f-04ef-49e2-97f8-f08de80a465d">

Using Node,Pod  Affinity we can even schedule better than node names, node selectors. Using these we can tell k8s scheduler deploy to the node whose 
 particualar label value greater/lesser than value.
![image](https://github.com/KORLA2/Kubernetes/assets/96729391/ce24e5a4-3b3b-45a2-bd39-ab325f19ed34)


 There are 2 terms 
####  1. requiredDuringSchedulingIgnoredDuringExecution
     Which means labels are required for node selection if not satisfied pod will be in pending state forever no node has satisfied this criteria.Pods which are already 
     present in the node are ignored though they didn't have necessary labels.

#### 2. prefferedDuringSchedulingIgnoredDuringExecutin
     
 





