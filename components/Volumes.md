We need volumes to persist the data in the pod. Like how we have docker volumes in Kubernetes also we have volumes to store the data.

### The 2 Problems volumes has to solve

#### Sharing Data across the replicas of the pod

#### Persisting Data

Let us take the example of deploying mongo db database as deployment and by attaching a node port service. Using mongo compass which is mongo UI ,let us access database.

Lets port forward the service using

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/210a3684-03f7-4582-a25a-640423fe76ce)

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/4da1fb3a-07ad-4edb-9f50-0c37a1c1cf13)

Using the root username and password lets connect to the database.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/8e824de9-fcb0-437c-95f2-46a57a023bde)

### Storing Data at Container Level

If we insert data to this pod through mongo compass , data inside container can persist as long as the container is alive after container restarts all the data stored will be vanished.

So lets delete container by going inside the pod using
![image](https://github.com/KORLA2/Kubernetes/assets/96729391/e0bbbcbf-9614-40f5-af79-2d46fb97e67a)

Using ps aux which lists all the process running inside container lets delete that mongo process using

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/a9e57ca7-9936-4783-8531-4b18bf27d697)

1 is the process id of mongo db process. We can look at the data in /data/db path this is where mongo container stores data .

After killing container, Kubernetes restarts the container and all the data will be lost.

Here no other container can share data except this container and there is no persistence of data.

### Storing Data at Pod Level

Here though container dies as the data stored in pod level container can use this data.

Using emptyDir we can create a path in pod using same name as the pod volume in volume mounts we can connect to the emptyDir pod volume and all the data stored in mountPath.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/5cc84dd5-8794-42ba-9868-e001589a1d4d)

Pod is also ephimeral as container so if the pod restarts then data stored will be vanished and also other replicas will not share the data in this pod .

If the data stored in the node other pods will be able to share though pod dies as the data is in the node no problem. But if the pod is in the other node then those pods will not be able to share the data on this node more over if the node dies all the data will be vanished again.

So to persist data kubernetes has 3 main components

1. Persistent Volume

2. Persistent Volume Claim

3.  Storage Class

### Persistent Volumes

Persistent Volume is the abstract component it has to take the storage from actual storage like AWS EBS , NFS , local disk etc.
   

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/9c9f8ea4-126e-4781-85bb-e0002928a9bb)


It can be created from usual YAML manifests.

3GB storage will now be created here we have used local disk to store data in /storage/data .

There are different access modes

1. ##### ReadWriteMany which means this volume can be used my many nodes if the pods are running on different nodes we can use this.

2. ##### ReadWriteOnce which means volume can be used my only one node.

3. ##### ReadOnlyMany / ReadOnlyOnce (Self Explainatory)
   
4. ##### ReadWriteOncePod which means only one node can use this volume.



### PersistentVolumeClaim

By using persistent volume claims which is another resource we can connect the persistent volumes to the pod.

Using storage and access modes, persistent volume claim selects the correct persistent volume.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/793cc03b-4ec7-4a57-91cc-b4d2248d2577)

##### kubectl get pv // For persistent volumes
##### kubectl get pvc // For Persistent volume claims

Using the name of the persistent volume claims under volumes section in deployment file pods can use the persistent volumes through persistent volume claims.

Now though we delete the pods , nodes even pv, pvc's data will never be vanished.

pv , pvc's cannot be deleted if they are in use to delete them we have to delete the pods using the pvc's and the pvc's using the pv.

### Storage Class

Instead of creating pv's ourselves with different storages we can create dynamically using storage class which reduces our work.

It uses pvc to create pv with provided accessmode and storage.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/08217bec-3ee7-46f1-ab73-c1d624cf47db)

provisioner is whose storage we are using here we are using local disk we can use AWS EBS , NFS etc.

#### volumeBIndingMode can be

1. ##### Immediate which means after creating pvc immediately pv will be created through storage class.

2. ##### WaitForFirstConsumer which means after pvc is bound by pod then pv will be created.

### reclaimPolicy can be

1. ##### Delete which means if the pvc was deleted then corresponding pv will also be deleted.

2. ##### retain which means if pvc deleted pv will not be deleted.

#### kubectl get sc // For storage class

##### By default we have standard storage class that every pvc will use.

If we overwrite our storage class name in pvc then instead of standard storage class, then our storage class will be used by pvc.

In minikube by default every pvc will use standard storage class and use pv created by this storage class and our created pv will be neglected.

To avoid this replace false with true in storageclass.kubernetes.io field.

By default we have standard storage class that every pvc will use.


![image](https://github.com/KORLA2/Kubernetes/assets/96729391/76e0bb18-72a6-46d9-aeb2-d80a203c4e92)

If we create our own storage class then there is no problem.

pvc's are namespaced and pv's and sc's are not namespaced.

Now other replicas can use the data used by on replica and data was persisted irrespective of pod/node health.
