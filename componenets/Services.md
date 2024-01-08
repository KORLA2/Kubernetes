## Service 
Service is going to provide a permanent IP to a pod even when it is recreated, so communication among pods can happen with the previous IP address. The service accepts internal or external requests and forwards the request to the pods that are connected to the particular service. There might be replicas of pods, and the service can forward requests to one of the pods. So services can even act like load balancers.


