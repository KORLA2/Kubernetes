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

<img width="327" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/67646c68-cd9f-4c32-9f2a-69df9d61e661">

 
     

