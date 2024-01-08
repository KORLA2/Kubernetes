## Deployment and Statefulset
Deployment is a template to create pods instead of creating a pod with command we write how many pods we want in a configuration file in YAML and we run that file this is called deployment.

We create deployment which creates pods and container runs inside a pod and application runs inside a container.

Statefulset is also a template to create pods but replicas created by statefulset is considered as different for example if we want to create database pods we have to choose statefulset as it ensures uniqueness among replicas but deployment doesn't.

Deployment
![image](https://user-images.githubusercontent.com/96729391/226091159-1cae5ca3-3048-4c97-ad7f-b94ae7909a1a.png)



