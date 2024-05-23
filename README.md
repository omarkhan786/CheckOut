# CheckOut

**Run server.py**
Open the CheckOut directory in an IDE

Run python3 server.py in a Terminal. This will return the following:

'Starting httpd server on port 8080'

Browse to http://localhost:8080 and you will see the Hello, World!

**Build the docker image**

docker build -t hello-world-server .

**Run the Docker container**

docker run -p 8080:8080 hello-world-server

**Access the web server**

After running the container, browse to http://localhost:8080 and you will see the Hello, World!

**Create an EKS Cluster using eksctl**

Run the following:

(INSTALL EKSCTL) curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin

(CREATE A CLUSTER) eksctl create cluster --name hello-world-cluster --region us-west-2 --nodes 2

**Create and push a Docker image to ECR**

(CREATE A REPO IN ECR) aws ecr create-repository --repository-name hello-world-repo --region us-west-2

(BUILD THE DOCKER IMAGE) docker build -t hello-world .

(CREATE A TAG) aws_account_id=$(aws sts get-caller-identity --query Account --output text)
docker tag hello-world:latest $aws_account_id.dkr.ecr.us-west-2.amazonaws.com/hello-world-repo:latest

(RUN THE AUTHENTICATION) aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin $aws_account_id.dkr.ecr.us-west-2.amazonaws.com

(PUSH THE IMAGE TO ECR) docker push $aws_account_id.dkr.ecr.us-west-2.amazonaws.com/hello-world-repo:latest

**Apply the deployment**

kubectl apply -f deployment.yaml

**Apply the service**

kubectl apply -f service.yaml

**Check it is running**

kubectl get deployments
kubectl get services

**Obtain the external ip**

kubectl get service hello-world-service

**Run the webapp**

Browse to the app by using the external ip 


