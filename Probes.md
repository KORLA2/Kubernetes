There might be many problems for the pod failure though the pod fails  and the main process in the container fails kubelet automatically restarts pod  but if there are bugs,
any database failures , out of memory issues kubelet wont start the pod and the application will not show the desired result.

Probes are used when we want kubelet to restart the pod when we dont expect desired result.. 

There are 3 types of probes.
1. Liveness probe
2. Readinesss probe
3. Start up probe

**LivenessProbe**

  Ensures container is always  running. We make network calls / execute some commands inside the container if fails then kubelet restarts the pod.
  
