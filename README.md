# Kubernetes_NodePort_MiniProject

In this project, we will learn about the concept of a NodePort as a service in the Kubernetes. NodePorts monitor external requests and directs them to, say, an application within the node. We will spin up multiple clusters and deploy the application on a number of them.

Firstly, let's create a cluster of nodes of size 3 (default):

```gcloud container clusters create test --zone europe-west2-c```

You can check your vm instances in Google Cloud, 3 new vms should have been created. Next, let's deploy a hello-world application on two of our vms:

```kubectl run hello-world --replicas=2 --labels="run=load-balancer-example" --image=gcr.io/google-samples/node-hello:1.0  --port=8080```

To see your deployments, run:

```kubectl get deployments hello-world```

Next, we must expose our deployments, to do this setup a NodePort service:

```kubectl expose deployment hello-world --type=NodePort --name=example-service```

A NodePort is a service that operates on ports 30000 - 32767. If you do not specify a specific port, Kubernetes will assign a random port in this range to your service. To see the value of your NodePort run the kubectl describe command:

```kubectl describe services example-service```

Next, remember that we deployed the application on two of our servers, but we created 3? To find out which servers we installed the applications on, run:

```kubectl get pods --selector="run=load-balancer-example" --output=wide```

Take the ip of one of your cluster vms, and set the tcp port for your NodePort on this vm. We know our NodePort from running the ```kubectl describe``` command previously. Now we can curl the message:

```curl http://<your IP>:<NodePort>```

Output:

```Hello Kubernetes!```
