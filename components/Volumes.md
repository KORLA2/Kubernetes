We need volumes to persist the data in the pod. Like how we have docker volumes in Kubernetes also we have volumes to store the data.

### The 2 Problems volumes has to solve

## Sharing Data across the replicas of the pod

## Persisting Data

Let us take the example of deploying mongo db database as deployment and by attaching a node port service. Using mongo compass which is mongo UI ,let us access database.

Lets port forward the service using
