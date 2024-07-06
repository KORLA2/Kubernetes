To Run the Pod resources are necessary (CPU, Memory).
We can request the kubernetes to allocate the pod with specified number of resources.
CPU in percentage  means millcores 100m = 100 millicores. 1000m=1CPU
Memory in bytes like 1KiB 1GiB Kibibytes Gibibytes.. . 1KiB = 1000 bytes 1KB=1024bytes

If the pod uses more resources than requested then other pods on this node can't get enough resources . So we can limit the pod to use number of resources.

<img width="186" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/05300ed4-f05f-4703-9985-b5021da97652">

 For some reasons pod sometimes uses more resources than its limit .If the pod uses more CPU than limit the pod throttles but if pod uses more memory the pod will be killed 
 with OOMkilled (Out of memory) status and it is run again.
 
If there are 2 pods and each  requested for 3GiB of memory but pod 2 used only 2GiB. When 3rd pod requested for 3GiB although the memory is available on node scheduler 
 doesn't schedule pod on this node as it looks for unallocated memory instead of free memory. When any pod consumes the memory up to limit then there is resource deficiency. Then Kubernetes has to kill one pod but on what basis? .
  
<img width="546" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/e1aeaa51-46f5-4184-a1c4-1edac334cff3">

There are 3 types of classes , Kubernetes assigns any one of these classes to Quality of Service property to the pod which kubernetes automatically assigns and based on the assigned class kubernetes then decides  which pod to kill.

1. Best Effort . If requests and limits are not mentioned. It consumes as much as it needs when few pods are present incase of many pods it might even not consume any resource.
2. Guaranteed. Both mentioned equally.
3. Burstable. Both mentioned unequally.

If Kubernetes has to kill any pod it kills the pod having Best Effort class as we didnt mentioned any request / limits it assumes that we are not much bothered about this pod.
Then Burstable as it might comsume more resources.


Using Limit Range which is another type of Kubernetes object through which we have granular level access to resources. By which we can restrict how much resources a 
pod/container/pvc  can use .If the request/ limits for container/pod .. specified are under the limits of Limit Range definition then only corresponding pod/pvc is created

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/74efdb32-5188-4b11-8817-6b53c6477ba5)

If we define resources less than the request or more than the limit pod won't be scheduled.
If we dont specify any limit or request to the <b>container</b> default , default request will be used. 

Instead of defining at pod level we can define at namespace level.
Using ResourceQuota object we can specify how many deployments/ statefulsets etc.. can be deployed to the namespace where resourcequota is deployed.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/633817b3-69e9-4399-9c54-09647789cf10)

