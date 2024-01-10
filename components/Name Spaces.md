NameSpaces are isolated environments to the teams.

Kubernetes Namespaces are important 

1. To Group the similar resources.

2. We need name spaces if many teams are deploying the same named deployment in the same cluster then previous one will be over wrote with next.

3. If in case of 2 versions of application that share common resources instead of deploying the next one in to other cluster we can deploy the next one in other name space in same cluster.

 kubectl get ns lists all namespaces in cluster.

Instead of using name of the namespace after each kubectl command to know resources in that namespace.

 using kubens we can switch the name spaces.

For Example:

### kubens mynamespace 

Now we can list all the resources in the mynamespace .

We can install it using sudo snap install kubectx. 

We can also switch the name space by 
###  kubectl config set-context --current --namespace==dev

