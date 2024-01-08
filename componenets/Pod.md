

## Node and Pod

Node is just a server on which containers are deployed. Pod is an abstraction over a container, when a Pod is successfully created,
it gets an IP address through which other Pods can communicate. Pods are emphimeral, which means they die easily and gets new IP when they are recreated. 
This makes it difficult to communicate with other pods. Hence, service has come.



 ![image](https://user-images.githubusercontent.com/96729391/226091019-9be32a8d-bc1e-4130-acdf-9074a3128eb3.png)


