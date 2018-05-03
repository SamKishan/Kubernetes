This repository contains the necessary files to deploy a Python web app (with redis database) on Kubernetes. It makes use of a single-node local cluster that can be spun up using minikube.
The command to do that is
```
minikube start
```
Make sure your docker client points to the docker engine running on the minikube VM using the command eval 
```
$(minikube docker-env) 
```
Now the samkishan/web2 docker image needs to be build. So, go to the ./Kube/Docker directory and exeute
``` 
docker build -d samkishan/web2 .
```
This should create the image and after completion you can check if the image has been created or not by using the command 
```
docker images
```

This newly created image has to be pushed to the local repository. This can be done using the command 
```
docker push samkishan/web2
``` 

"cd" into the Kubernetes directory and then create the Kubernetes resources in the following order (using the following commands):
```
kubectl create -f db-pod.yml
kubectl create -f db-svc.yml
kubectl create -f web-pod.yml
kubectl create -f web-svc.yml
```

The functions of the yml files:
db-pod.yml: Used to spin up the redis Pod.
db-svc.yml: Allow the db-pod.yml be exposed in order to have the web server pod be able to communicate with it.
web-pod.yml: Used to spin up the webserver pod.
web-svc.yml: Allow the webserver pod and redis pod communicate within the private network. Also expose the web server pod to the Internet.

To check if the web application runs or not, you need to do two things.
1) Get the IP address of minikube vm by executing the command 
```
minikube ip
```
2) Get the node port using the "kubectl get svc" command. This will tell you which port on the Node forwards http requests to the web server pod. It's usually around 32000.

With that information, go to your browser and enter the following URL "http://<minikube_vm_ip>:<node_port>

You should see the following sort of output "Hello Container World! I have been seen x times. "  

If you need more information, you can reach me at "saki8093@colorado.edu"


Improvements for the future:
	1) Addition of a replication controller.
	2) Migration to Google cloud since GCE offers a lot of support for Kubernetes deployments.


-Sam Kishan
