# Kubernetes_NodePort_MiniProject

In this project, we will learn about the concept of a NodePort as a service in the Kubernetes. NodePorts monitor external requests and directs them to, say, an application within the node. We will spin up multiple clusters and deploy the application on a number of them.

*NOTE:* A node is a vm.

Firstly, let's create a cluster of nodes of size 1 (if ```--num-nodes``` unspecified then 3 nodes will be created):

```gcloud container clusters create test --zone europe-west2-c --num-nodes 1```

You can check your vm instances in Google Cloud, one new vm (node) should have been created. Next, let's create two pods each containing our hello-world application (deploying the hello-world application on our node):

```kubectl run hello-world --replicas=2 --labels="run=load-balancer-example" --image=gcr.io/google-samples/node-hello:1.0  --port=8080```

To see your deployments, run:

```kubectl get deployments hello-world```

Next, we must expose our deployments, to do this setup a NodePort service:

```kubectl expose deployment hello-world --type=NodePort --name=example-service```

A NodePort is a service that operates on ports 30000 - 32767. If you do not specify a specific port, Kubernetes will assign a random port in this range to your service. To see the value of your NodePort run the kubectl describe command:

```kubectl describe services example-service```

Next, let us see the pods that we created, run:

```kubectl get pods --selector="run=load-balancer-example" --output=wide```

We can see two pods attached to the one node that we created for our cluster.

Next, allow tcp access for your NodePort on this node. To do this, we must configure the firewall rules on the node through Google Cloud to accept your NodePort. We know our NodePort from running the ```kubectl describe``` command previously.

Now we can curl the message:

```curl http://<IP of your vm>:<NodePort>```

Output:

```Hello Kubernetes!```

We have now established a connection to the containerized application within the node. The service acts as a load balancer, automatically managing traffic.
