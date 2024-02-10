### Ingress

 Using NodePort , Load balancer type we can access pod from outside but in Node port type it opens port on Node which is insecure and in load balancer type it only works
 when we use  kubernetes in cloud .
 
 Although we use kubernetes in cloud if we have many services that are exposed as load balancers then many load balancers are needed which are costly. Fails to provide secure connection ( https).
 
More Over Kubernetes fails to provide 

1   Good Load balancing which means service using kube proxy services  provides basic load balancing 

2.  Whitelist / black list  some ips

3.  Sticky sessions which means redirecting some ips to same pods.

4. Path based Domain based Routing   

People coming from VM to kubernetes has observed many problems that kubernetes could not able to solve are solved by external load balancers 
like Nginx , HA Proxy  , traefik .. .


So it is impossible for kubernetes to build all the features that 3rd party tools have So Kubernetes allowed those 3rd party tools to contribute to kubernetes.

So we have to install the ingress controller which means which any 3rd party load balancer to  the cluster.Then we have to define ingress resource these ingress rules are monitored by this ingress controller. Because of Ingress  now we can have enterprise level benefits in kubernetes cluster.

<img width="629" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/abd26f8d-a3b1-4d00-a582-20686078d6ce">


Using ingress we can connect to many services so exposing ingress as load balancer requires only one ip through which many services can be accessed.
Using Ingress we can access app through any domain like goutham-app.com

### ingress - class
As ingress controllers are from different 3rd party tools , ingress rules are kubernetes specific which means any ingress controller can run this rules but the 
implementaion varies. So without the ingress class ingress controller cannot run the rules. Ingress rules with the ingress class will be run by ingress controller having same ingress class. This is to restrict multiple ingress controller to run the same ingress class.


### Path Based Routing 

When ever the request comes to controller pod it routes to corresponding service using ingress rules.

<img width="262" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/f3cb5b7c-405f-49ab-85d8-8a54dd301855">

When the path is nginx.o/ then it  routes to pathsvc service if it is nginx.o/lib then routes to libsvc.

### Host Based Routing

<img width="247" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/db79a1dc-13c2-4432-bb7f-8f370145df88">

But to access the domain name in browser we have to manage the local domains , in /etc/hosts file map the domain name to the node ip.

Path type is required in ingress rules manifest file.
Pathtypes can be
1. prefix
2. exact
Prefix is path must be prefix of the requested path.
### /api/bar is prefix of /api/bar/foo or /api/bar/ but not with /api/barfoo
Exact is requested path must match to path in the manifest file.

### Example

In minikube by there is a ingress  addons we can enable using
 #### minikube enable addons ingress

We can list all addons in minikube using  minikube addons list

Then  the necessary ingress controller pod , services .. are deployed to control-plane node in ingress-nginx namespace.
<img width="931" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/164034bc-316f-46bf-92f3-addfa98c296c">

I have created 2 pods and using host volumes I have added some text to /path/index.html file and /libs/index.html file on local-cluster node and mounted to nginx mount path and attached 2 services to 2 pods .


<img width="244" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/d1a6b746-f20f-479f-876b-a960b6cfd784">.

<img width="225" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/b6803061-8b4c-4eb0-957e-4b3b5745f373">

Now I attached ingress to these services.

Using Path based routing as above configuration.
Using curl command I can access the service

<img width="206" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/7a7da1d2-097d-450a-a873-ce0e5963d368">

But there is a problem when we  use curl  nginx.o/lib 

<img width="292" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/71232d77-e15d-4a2e-8ae2-05061b41b7ff">

Though we mentioned when we hit /lib path, request must be routed to libsvc service but the same path is forwarded to the pod but the pod doesnt have the path, so we get this page.

So after the service receives the request the /lib path has to be reomved and forwarded to pod.

<img width="372" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/b9da6f22-a155-4c25-9ee9-464b960b6ab4">

Using rewrite-target  annotation we can achieve this.


<img width="326" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/57d6e138-087a-4fc1-8efb-eecad72915f8">.

So now when I request nginx.o/lib then after the service receives the request /lib is replaced by /.

<img width="302" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/69ecfec0-8318-4e5c-944f-00d6a0292432">



 
