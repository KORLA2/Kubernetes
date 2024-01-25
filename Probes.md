There might be many problems for the pod failure though the pod fails  and the main process in the container fails kubelet automatically restarts pod  but if there are bugs,
any database failures , out of memory issues kubelet wont start the pod and the application will not show the desired result.

Probes are used when we want kubelet to restart the pod when we dont expect desired result.. 

There are 3 types of probes.
1. Liveness probe
2. Readinesss probe
3. Start up probe

### LivenessProbe

  Ensures container is always  running and pods are always running. We make network calls / execute some commands inside the container,  
  if fails then kubelet restarts the pod.
  
  <img width="600" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/310cdd58-caf9-409e-be9c-2c2347432fd1">

We can run some commads / make network calls / verify some port is open at the container level. If probe results in failure kubelet restarts the pod.

How frequently we have to run the pod ?  

  <img width="618" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/cbd7870e-ecc2-4e7e-a5dc-0bc561e6a0b0">

 **initialdelaySeconds -** After how much time after the container is ready kubernetes has to run the probe.

 **periodSeconds-** How  frequently kubernetes has to run the probe.

 **time out** Each probe is marked as  failure after these many seconds.
 
 **failure threshold** How many times kubernetes has to restart the probe after the failure . 
 
 <img width="250" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/75fce9a8-f4db-469d-82c7-3b747d146201">


### Readiness Probe 

It determines when container is ready to accept traffic from service. If this fails pod is not restarted and then pod Ip was removed from service end points.

### StartUp probe 

Liveness and Readiness Probe will only be executed when startup probe is successful.

<img width="637" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/35b8a032-6069-4484-80eb-985fa806a87b">


