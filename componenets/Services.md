## Service and Ingress
Service is going to provide a permanent IP to a pod even when it is recreated, so communication among pods can happen with the previous IP address. The service accepts internal or external requests and forwards the request to the pods that are connected to the particular service. There might be replicas of pods, and the service can forward requests to one of the pods. So services can even act like load balancers.

Ingress is used to accept the request from outside the cluster and forward the request to service and service then forwards it to pods. There is a difference between external service and Ingress, external service accepts requests with IP address of the application but Ingress accepts with the domain name.

![image](https://user-images.githubusercontent.com/96729391/226091113-b424bec5-efe7-4deb-a4b2-fac2cebd7e15.png)


Statefulset 
![image](https://user-images.githubusercontent.com/96729391/226091166-c87e7818-75d6-4abb-8690-78ffb960f4ce.png)

