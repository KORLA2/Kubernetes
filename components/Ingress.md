### Ingress

 Using NodePort , Load balancer type we can access pod from outside but in Node port type it opens port on Node which is insecure and in load balancer type it only works
 when we use  kubernetes in cloud .
 
 Although we use kubernetes in cloud if we have many services then many load balancers are needed which are costly. Fails to provide secure connection ( https).
 
  More Over Kubernetes fails to provide 

1   Good Load balancing which means service using kube proxy provides basic load balancing 

2.  Whitelist / black list  some ips

3.  Sticky sessions which means redirecting some ips to same pods.

4. Path based Domain based Routing   

People coming from VM to kubernetes has observed many problems that kubernetes could not able to solve are solved by external load balancers 
like Nginx , HA Proxy .. .


So it is impossible for kubernetes to build all the features that 3rd party tools have So Kubernetes allowed those 3rd party tools to contribute to kubernetes.

So we have to install the ingress controller which means which any 3rd party load balancer in the cluster.Then we have to define ingress resource this will be
watched by this ingress controller. Because of Ingress  now we can have enterprise level benefits in kubernetes cluster.



![image](https://github.com/KORLA2/Kubernetes/assets/96729391/77fcc8cc-f08c-403d-b70f-4d2b10d64b90)


 
