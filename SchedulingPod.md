Kubernetes Scheduler filters all the nodes that can run the pod and score the nodes based on resources and many factors and schedules on  highest scored node.

How to explicitly tell kubernetes scheduler to schedule a pod on the node we are interested in?

1. Using Nodename
2. Using Node Selectors
3. Node/Pod Affinity
4. Taints and Tolerations
 ## Node names
 Using Nodenames we can tell kubernetes scheduler to schedule the pods on the specified node.
 
  <img width="204" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/8ef7b8c6-b594-4f94-97cd-0662c81d3b74"/>
  
  Here the pods will be scheduled in node 3.
  ## Node Selectors
  To achieve high availability node names are not suited.
  Using Node Selectors we specify labels to nodes and we tell to deploy the pods to the nodes having corresponding labels.
  Now pods are across different nodes and high availability is achieved and deployed on the nodes we want.
    kubectl  label node minikube-m03 minikube-m02 team=analytics.

  Using this command we can add team=analytics label to nodes  minikube-m02 , m03. In pod definition using node Selector we can deploy the pod to nodes having
  team analytics label. Now pods are deployed to minikube-m02 , minikube-m03 nodes.

  <img width="209" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/2dfbc11f-04ef-49e2-97f8-f08de80a465d"/>
  
## Node/Pod Affinity
Using Node,Pod  Affinity we can even schedule better than node names, node selectors. Using these we can tell k8s scheduler deploy to the node whose 
particualar label value greater/lesser than value and much more we can always deploy some pods on the same node or always different nodes etc.
 
 ![image](https://github.com/KORLA2/Kubernetes/assets/96729391/ce24e5a4-3b3b-45a2-bd39-ab325f19ed34)


   There are 2 terms. 
 
#### 1. requiredDuringSchedulingIgnoredDuringExecution
 Which means labels are required for node selection if not satisfied pod will be in pending state forever.Pods which are already present in the node are ignored 
 though they didn't have necessary labels.

#### 2. prefferedDuringSchedulingIgnoredDuringExecution
 Which means labels are preferred though none of the node has the lables best rank node will be preferred.Here we have given more preference to the node which has 
label rank and the value is greater than 4, if this is not possible then on to node which has team=analytics label . If this is also not possible then pod is 
 scheduled on some other node. This term is just preffered.
 
 <img width="346" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/b8f6eddf-0a4e-47f1-a0c6-06e4161d6630"/>

Node affinity is selecting nodes and pod affinity is selecting pod.

  Using Pod affinity we can force the scheduler to schedule the pods always on same, different nodes. 
       
 <img width="268" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/be3d3347-22ce-45b1-b4cc-3f34d9f62337"/>
          
   #### Here the pod will be deployed to same nodes in which a pod has a app=todo-ui label deployed.

 Using Pod AntiAffinity we can even force the scheduler to not to deploy on to the nodes in which pods having a todo=ui label.
       
  <img width="260" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/7ec55b3c-a236-42f2-900a-ea39c281cf3a"/>

## Taints and Tolerations.

In case we want dont want pods to be deployed on to node in which it can't tolerate which means we dont want some pods to be deployed on some nodes then we simple add some taints (simple labels) to nodes. There are 3 type of  taints

  ### 1. No Schedule
  ### 2. No Execute
  ### 3. Prefer No Schedule.

#### No Schedule is explicitly we tell the scheduler that don't schedule pods on the nodes if they cant tolerate. Already existing pods on the node will not be affected by this.  

#### No Execute is more stricter than no schedule , alredy existing pods will be terminated and will be scheduled to other nodes if they can't tolerate.

#### Prefer no schedule is scheduler will not prefer the node if pods cant tolerate , but if no satisfied node was found scheduler schedules on the node though pod can't tolerate. 

     kubectl taint minikube-m02 env=production:NoExecute
Meaning we tainted node minikube-m02 with env:production label if the pods existing on this node cannot tolerate they will be terminated and scheduled on other nodes and no new pod will be scheduled on this node if they can't tolerate.

If we taint the node with above command and if we wantedly tell scheduler to schedule pod on the minikube-m02 node using node selector or node affinity, then pod will be in pending state.

If the pod can tolerate the taint using this,

 <img width="199" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/22e75c8a-b209-45ba-89a9-8958bde84d6f">

then pod can be scheduled.
Tolerations are at pod level.

Without adding tolerations to taints pods will not be scheduled on the node having taint.

### Untaint the label on the node
    kubectl taint minikube-m02 env:prod:NoExecute-
     
  



