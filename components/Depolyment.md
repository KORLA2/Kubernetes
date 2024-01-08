## Deployment 
Deployment is a template to create pods instead of creating a pod with command we write how many pods we want in a configuration file in YAML and we run that file this is called deployment.

We create deployment which creates pods and container runs inside a pod and application runs inside a container.

Deployment
![image](https://user-images.githubusercontent.com/96729391/226091159-1cae5ca3-3048-4c97-ad7f-b94ae7909a1a.png)



While rolling out to some version of app in Kubernetes if we mention some meaningful text under annotations in deployment file helps us in identifying what we have modified in each version, else we might not be able to know what that version does.

Using kubectl rollout history deployment <Deployment Name>
lists all the versions of the app and what we have done in that version.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/0771c9d2-9c33-45cc-b294-92478f2066a4)


Using
 kubectl rollout undo deployment <Deployment Name > --to-revision=4
Rolls out to 4th version of the app.

![image](https://github.com/KORLA2/Kubernetes/assets/96729391/fc471a85-68b2-4064-90bc-e005240d022f)


![image](https://github.com/KORLA2/Kubernetes/assets/96729391/defbfa2f-9780-4345-baad-f09fa8c278b3)

