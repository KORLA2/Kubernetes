To Run the Pod resources are necessary (CPU, Memory).
We can request the kubernetes to allocate the pod with specified number of resources.
CPU in percentage  means millcores 100m = 100 millicores. 1000m=1CPU
Memory in bytes like 1KiB 1GiB Kibibytes Gibibytes.. . 1KB = 1000 bytes 1KiB=1024bytes

If the pod uses more resources than requested then other pods on this node can't get enough resources . So we can limit the pod to use number of resources.

<img width="186" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/05300ed4-f05f-4703-9985-b5021da97652">

 For some reasons pod sometimes uses more resources than its limit .If the pod uses more CPU than limit the pod throttles but if pod uses more memory the pod will be killed 
 with OOMkilled (Out of memory) status and it is run again.
 
**If there are 2 pods and each  requested for 3GiB of memory but pod 2 used only 2GiB. When 3rd pod requested for 3GiB although the memory is available on node scheduler 
 doesn't schedule pod on this node as it looks for unallocated memory instead of free memory. When any pod consumes the memory up to limit then there is resource deficiency.
 Then Kubernetes has to kill one pod but on what basis? **.
  
![Uploading image.png…]()

