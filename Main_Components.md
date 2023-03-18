# Kubernetes Components

Some of the important components in the Kubernetes World that I learned are

1 . Node and Pod

2. Service and Ingress

3. ConfigMap and Secrets

4. Deployment and Statefulset

## Node and Pod

Node is just a server on which containers are deployed. Pod is an abstraction over a container, when a Pod is successfully created,
it gets an IP address through which other Pods can communicate. Pods are emphimeral, which means they die easily and gets new IP when they are recreated. 
This makes it difficult to communicate with other pods. Hence, service has come.



 ![image](https://user-images.githubusercontent.com/96729391/226091019-9be32a8d-bc1e-4130-acdf-9074a3128eb3.png)

## Service and Ingress
Service is going to provide a permanent IP to a pod even when it is recreated, so communication among pods can happen with the previous IP address. The service accepts internal or external requests and forwards the request to the pods that are connected to the particular service. There might be replicas of pods, and the service can forward requests to one of the pods. So services can even act like load balancers.

Ingress is used to accept the request from outside the cluster and forward the request to service and service then forwards it to pods. There is a difference between external service and Ingress, external service accepts requests with IP address of the application but Ingress accepts with the domain name.

![image](https://user-images.githubusercontent.com/96729391/226091113-b424bec5-efe7-4deb-a4b2-fac2cebd7e15.png)

## Deployment and Statefulset
Deployment is a template to create pods instead of creating a pod with command we write how many pods we want in a configuration file in YAML and we run that file this is called deployment.

We create deployment which creates pods and container runs inside a pod and application runs inside a container.

Statefulset is also a template to create pods but replicas created by statefulset is considered as different for example if we want to create database pods we have to choose statefulset as it ensures uniqueness among replicas but deployment doesn't.

Deployment
![image](https://user-images.githubusercontent.com/96729391/226091159-1cae5ca3-3048-4c97-ad7f-b94ae7909a1a.png)



Statefulset 
![image](https://user-images.githubusercontent.com/96729391/226091166-c87e7818-75d6-4abb-8690-78ffb960f4ce.png)


ConfigMap and Secrets
Both are configuration files in YAML and keys in these files are used as environment variables while creating deployment files. Secrets contains usernames and passwords in base64 encoded ConfigMap contains some urls , names etc which we don't want to put in deployment files.

![image](https://user-images.githubusercontent.com/96729391/226091176-33de05b3-53e7-46a8-b5df-9b3837d21a3f.png)


