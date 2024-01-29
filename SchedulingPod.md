How to explicitly tell kubernetes scheduler to schedule a pod on the node we are interested in?

1. Using Nodename
2. Using Node Selectors
3. Node/Pod  Affinity
4. Tanes

 Using Nodenames we can tell kubernetes scheduler to schedule the pods on the specified node.
 
  <img width="204" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/8ef7b8c6-b594-4f94-97cd-0662c81d3b74"/>
  
  Here the pods will be scheduled in node 3.

  Using Node Selectors we specify labels to nodes and we tell to deploy the pods to the nodes having corresponding labels.

##### kubectl  label node minikube-m03 minikube-m02 team=analytics.

  Using this command we can add team=analytics label to nodes  minikube-m02 , m03 In pod definition using node Selector we can deploy the pod to nodes having
  team analytics label.

  <img width="209" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/2dfbc11f-04ef-49e2-97f8-f08de80a465d"/>

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

  Using Pod affinity we can force the scheduler to schedule the pods always on same, different nodes. 
       
 <img width="268" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/be3d3347-22ce-45b1-b4cc-3f34d9f62337"/>
          
   #### Here the pod will be deployed to same nodes in which a pod has a app=todo-ui label deployed.

 Using Pod AntiAffinity we can even force the scheduler to not to deploy on to the nodes in which pods having a todo=ui label.
       
  <img width="260" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/7ec55b3c-a236-42f2-900a-ea39c281cf3a"/>





