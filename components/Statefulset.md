In case we need to store the state of the request which means for example wheather to check user was authenticted or not?

If the pod receives the request , verifies credentials and user logged in to use app but if we have many such replicas using load balancer another request may be 
forwarded to some other replica and this replica is unaware of this user. 

If one request depends on the previous one then these apps are known as  stateful. So the status of the user is better to store in database.  Then data base is stateful and 
frontend app is stateless. 


<img width="626" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/8239e298-1356-44fa-8ef3-674d5b171c18">

Problems Statefulset Solves:

#### 1. Seperate PV

To achieve high availability we need replicas . If we deploy state ful apps like data base as a deployment then each replica uses same Persistent Volumes then there is 
inconsistency in Reads and writes.
    
  <img width="182" alt="image" align="center" src="https://github.com/KORLA2/Kubernetes/assets/96729391/a02440d2-685d-4c8e-898c-b4c83f4c978a">


There is other Kubernetes object called Statefulset if we deploy such apps as statefulset then each replica use different PV.

  <img width="181" alt="image"  align="center" src="https://github.com/KORLA2/Kubernetes/assets/96729391/c2ce4e50-02ee-4883-9d4b-dd7cb72a8d95">

 #### 2. Ordered Pods 
 
We have to allow only one data base to write (Master Data Base) and all other replicas to read (Slaves).   Kubernetes start replicas of statefulset orderly which means first master pod and then slave pods will be created one by one  but not like Deployment Randomly. Because there should be sync in between pods. Pods copy data from previous pods. If we delete/ scale down  last pod will be deleted.
   
   <img width="333" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/3915747a-e227-4a76-aa6c-c1d609fe25df">

#### 3. Sticky Identity

Pod copies data from the previous pod, to identify the previous pod it uses its name so that should not change even after restarting.
As each replica gets its own own PV even after restarting pod is attached to same PVC.

<img width="327" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/67646c68-cd9f-4c32-9f2a-69df9d61e661">


### Headless Services

To interact with Pod we need services which  load balances and send request to any pod , but in stateful apps we have to select only one pod and write to that pod and 
rest all pods copies data from it. So for this purpose headless services are used. Headless Services also does load balancing but using these we can select any specific pod.
By Declaring ClusterIp as None we can create headless service.

By mentioning name of the pod in front of service name we can receive response from specific pod.

#### curl mongopod-1.mongosvc 
Assuming mongosvc is a head less service and connected to mongopod-1 

Let us Create 3 replicas of Mongo Db Statefulset and connect with Mongo Compass.
Creating Statefulsets are similar to creating deployments. We have to mention service name.

<img width="338" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/5af6a1eb-ebac-4664-86b2-7bccbd00ea80">


<img width="183" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/d934cc76-c6e7-446b-8665-896f9923a9aa">

Now this headless service is connected to those 3 replicas of mongodb pods.

After connecting to mongosvc through mongo compass as mentioned in volumes chapter.

It is randomly connected to any mongo pod If we write to any pod using mongo compass after refresh service connects to other pod and it is not aware of the data stored 
in other pods as because though it is statefulset there is no replication of data among pods.
To retain the data we have to use persistent volumes.

But unlike deployments we need individual volumes for each pod, statefulsets take care of this by simply using volumeclaimtemplates in statefulset definition.


<img width="254" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/409fc490-02fd-4c4e-842b-4598d73ecf73">

Now using Storage Class or  manually creating persistent volume with necessary configuration persistent volumes are attached.


 
     

