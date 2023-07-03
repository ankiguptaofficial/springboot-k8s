Deploying Spring Boot application on kubernetes

 We will be deploying a Java Spring Boot Application on Kubernetes.

 in this project we can use these step for deploy java base springboot application on k8s

Set up the cloud server
Install the dependencies
Set up Mini-Kube
Clone the Repository
Configure the database required.
Build the project
Create the Docker images
Push the images to Dockerhub
Run the manifest files
Do port forwarding
Check JSON data to be hit on Postman
Add some data to the database using postman
Check the Database if the entries are visible
Check Mini-Kube dashboard

Github repo:- GitHub Repo for source code: https://github.com/ankiguptaofficial/springboot-k8s


In AWS- Create a instance(server) t2.medium

After create ec2 instance we going to ssh :-

Update the packages and package caches. Use commands :

##yum update -y

Install docker. Use commands :
#yum install docker
#systemctl enable docker
#systemctl start docker
#systemctl status docker


Note : Conntrack is needed for minikube.
Conntrack : is a short for Connection Tracking, is a crucial component in Kubernetes networking that is required for reliable communication between containers and services. It is responsible for tracking network connections and their states, allowing the Kubernetes networking stack to function properly.

Install Conntrack
#yum install conntrack -y

The below command is used to download the latest version of Minikube for the Linux platform.
#curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

minikube has been downloaded

Install minikube. Use command :
#sudo install minikube-linux-amd64 /usr/local/bin/minikube

Start minikube. Use command :
minikube is platform which gives k8s, indirectly we r installing k8s here

#/usr/local/bin/minikube start --force --driver=docker


Kubernetes is a platform which is running on docker. Docker is helping Kubernetes to run.
Install kubectl. Use command :

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

Giving permissions to kubectl.
#sudo install -o root -g root -m 0755 kubectl/usr/local/bin/kubectl

Clone the repo. Use command : https://github.com/ankiguptaofficial/springboot-k8s

Now we will start setting up the Database.
DB installation / make the DB up. Use commands :

#/usr/local/bin/kubectl get pods
#/usr/local/bin/kubectl create -f db-deployment.yaml

To create the persistent database and other services.
our database successfully created.

cd /usr/local/bin

Check pods. Use command :
#kubectl get pods

Check database (go inside the container). Use command :
#/usr/local/bin/kubectl exec -it mysql-f759455cd-n5gw6 /bin/bash

Enter the DB
password is : root

#mysql -u root -p

We can see the DB created from the deployment file.
DB name : singamlabs

Install maven now and check version.Use command :
yum install maven -y
mvn -v

Check if any images are available. Use command :
#docker images

Create the docker image now.
docker build -t ankiguptaofficial/java-springboot:latest .

Login to docker hub. Use command :
#docker login

Push the image to docker hub. Use command :
@docker image push rajjo103/java-springboot:latest

Check on DockerHub if image is pushed.

Now execute the app. deploy.yaml file.
#/usr/local/bin/kubectl apply -f app-deployment.yaml

Check pods now.
/usr/local/bin/kubectl get pods

We have 4 pods -> 3 are replicas.

Check svc available
#/usr/local/bin/kubectl get svc

Check minikube ip
#/usr/local/bin/minikube ip

Replicas are being created so that the traffic will get distributed.
/usr/local/bin/kubectl port-forward --address 0.0.0.0 svc/springboot-crud-svc 8080:8080 &

Now if you go and check on postman. You won’t be able to access because you have not opened the port for the server.

Update it.

Now go back to postman again. And add a entry.

Now go to the databse in the server and check the entries done by you in the SQL database.
/usr/local/bin/kubectl exec -it mysql-f759455cd-n5gw6 /bin/bash

You can see all the entries done by you. Now we will add more data to see if its reflecting here.

Now enter the database and check if the newly added entry(misalpav in my case) is reflecting in the database.

Exit from the mySQL now.


Now we will open the Dashboard of K8S.
Open a new terminal of your server and then perform this command.
Inside EC2 our minikube is running so we are setting a proxy for all the local address.
#/usr/local/bin/kubectl proxy --address='0.0.0.0' --accept-hosts='^*$'


The proxy server is started on port 8001.
Now open another terminal and run below command.


#/usr/local/bin/minikube dashboard


http://127.0.0.1:40575/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/
change the IP address with your EC2 server IP address and port 8081.
In my case:
http://18.218.83.178:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/    


You can access the same on browser.

You can get into any pod and check the pod logs.



















