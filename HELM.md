In every environment deploying same manifests with little change is tidious . Using Helm all the manifests uses one ` <b>values.yaml</b> ` file  any changes has to be 
written in this file. 

<img width="312" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/700664d9-5d37-4f76-9628-6ed99374741a">

So instead of deploying every manifest file manually , changing in values.yaml file using single helm command all manifests will be deployed to corresponding 
environments.

Helm is a bundle of kubernetes manifests
Helm is a package manager for kubernetes. 

### Helm Use Cases

1.  Packing all the manifests and deploys it together.
2. Dependencies are easily managed with Helm when ever one pod is dependent on other.
3. In every environment writing and deploying same manifests with some little difference in syntax is tidious.
4. Rollbacks are easy with Helm.

In every stage same manifest files are deployed and these files  share data from values.yaml file what ever we want to change , we can change in this file ,
deploying becomes easy through helm.

## Helm Architecture 

<img width="605" alt="image" src="https://github.com/KORLA2/Kubernetes/assets/96729391/21a15615-347f-487b-8f17-7b0df3a1ec34">

Helm is a command line tool uses values.yaml file to update the content in each manifests file uses artifact hub to pull some charts if mentioned in our chart.yaml file.
uses kubeconfig file (which contains cluster related information) to deploy to the cluster.

Using simple sudo snap install helm installs helm .

##### helm create <Chart Name> command creates necessary chart locally.

### Helm Syntax

1. {{}} braces are used in any file in helm to substitute data from another file.

2. If we want to use our own value when specfied field in values.yaml is not present then we use default keyword
     ex: {{ .Values.data | default 2 }} 2 is used if data is not present in values.yaml file.

3. If we want ot enclose data in string quote keyword is used.
     ex: {{ .Values.data | quote }}

4. nindent key word is used to indent the data upto specified spaces.
   ex: {{ .Values.data | nindent 12 }} indents data to 12 spaces this is required as indentation is required in Yaml.

5. In any braces - symbol is used to remove extra spaces 
   ex: {{-Values.data}}

6. We use range keyword to iterate the array in the values.yaml , we use if/ else conditions to verify fields in values.yaml file and we use end to end the block.

```
deployment.yaml

  {{- if .Values.ingress.tls }}
  tls:
    {{- range .Values.ingress.tls }}
    
    - hosts:
        {{- range .hosts }}
        - {{ . | quote }}
        {{- end }}
      secretName: {{ .secretName }}
    {{- end }}
  {{- end }}

```
```
Values.yaml
 tls: 
    - secretName: chart-example-tls
      hosts:
        - chart-example.local
```
    
### Setting Any Value through Command Line
 `helm install chartname --setdeployment.image:redis`     
### Our Own values.yaml file 
 helm install chartname --values <Location of values.yaml>
 
### Helm commands
 ####  helm install  chartname  deploys all the resources  to kubernetes cluster 
 #### helm template <Location of Manifests> Outputs all the data in all the manifests that helm took from values.yaml
 #### helm upgrade <Release name> Updates the existing chart
 #### helm list  lists all the relases deployed with revisions.
 #### helm history <Release Name> lists all the versions of release of app.
 #### helm rollback <Release Name > <Revision Name> ROll back to that version.
 ### helm package <Location of Manifests> packages the resources. After this command is executed it creates zip file.
 ### helm repo index <Location of above zip file> Creates index.yaml file contains all the information of this chart.

  


