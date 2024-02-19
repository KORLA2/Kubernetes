To access any resource first we must prove that we are a valid user this is authentication and we also  must prove that we are valid user to perform actions on the
resource this is  authorization.

<img width="328" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/24b21bf5-fb45-41ba-a87b-5e84fc30d123">

Till now we deployed/ accessed all the objects using minikube root user , kubernetes also has this type of feature  accesing the resources based on the permissions 
the user has.

 ### Role Based Access Control
 In Kubernetes we can set the permissions to  user/group or some other application to perform the activities on the cluster.


By default kubernetes doesn't support users user management was supported by external tools like AWS IAM , keycloack , but authorization was managed by kubernetes itself.
Every request to access the cluster 1st reaches to API server it decides wheather or not allow the request. 
![image](https://github.com/KORLA2/Kubernetes/assets/96729391/977a7980-a464-457a-8918-2624a7c78f00)


All the users realted information present in ~/.kube/config file in the minikube. Any user is treated as authenticated if the user certificate is verified by the 
certificate authority.

<img width="367" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/92486444-1899-4db6-a8fb-7c31f83e01d7">.


##### Every kubectl command reaches to API server through the kube config file it has all the details where is the master node , user , clusters , namespaces etc.

As mentioned before, Kubernetes has no concept of users, it trusts certificates that is signed by its CA.
This allows a lot of flexibility as Kubernetes lets you bring your own auth mechanisms, such as OpenID Connect or OAuth.

This allows managed Kubernetes offerings to use their cloud logins to authenticate.

So on Azure, I can use my Microsoft account, GKE my Google account and AWS EKS my AWS account.

This means on kubernetes we can't cerate a user called goutham.

1. On minikube we have to create a client-certificate and mention that it has a common name goutham 
2. And we have to sign the client certificate with kubernetes ca and cakey
3. Then we have to create a role to add permissions , create role binding to assign the role to certificate using same common name. 

So when we use any command, kubernetes verifies the client-certificate and the role it has to perform actions on the cluster.

#### On Minikube we have to create the client certificate and attach common name and do all these but in managed kubernetes authenticating is different.

Using Openssl we can create certificates.

Let's create a certificate for Goutham:


```
#start with a private key
openssl genrsa -out goutham.key 2048
```

Now we have a key, we need a certificate signing request (CSR). </br>
We also need to specify the groups that Bob belongs to. </br>
Let's pretend goutham is part of the `Shopping` team and will be developing 
applications for the `Shopping` 

```
openssl req -new -key goutham.key -out goutham.csr -subj "/CN=Goutham/O=Shopping"
```

Use the CA to generate our certificate by signing our CSR. </br>
We may set an expiry on our certificate as well

```
openssl x509 -req -in bob.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out bob.crt -days 10

```

We can even create our own kube/config file and explicitly tell the kubectl commands to look at this file while connecting masternode.

We'll be trying to avoid messing with our current kubernetes config. </br>

So lets tell `kubectl` to look at a new config that does not yet exists 

```
export KUBECONFIG=~/.kube/new-config
```

Create a cluster entry which points to the cluster and contains the details of the CA certificate:


```
kubectl config set-cluster dev-cluster --server=https://127.0.0.1:52807 \
--certificate-authority=ca.crt

#see changes 
vim  ~/.kube/new-config
```


kubectl config set-credentials goutham --client-certificate=goutham.crt  --client-key=goutham.key 

kubectl config set-context dev --cluster=dev-cluster --namespace=shopping --user=Goutham  <b>Must be same as the CN in openssl command </b> 

kubectl config use-context dev

kubectl get pods
Error from server (Forbidden): pods is forbidden: User "Goutham" cannot list resource "pods" in API group "" in the namespace "shopping"




