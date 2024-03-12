# DailyGrind

## Project Overview

This project is a microservices-based application that uses Docker and Kubernetes for containerization and orchestration. The application consists of the following services:

- **Zookeeper**: A service used for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

- **Kafka**: A distributed streaming platform used for building real-time data pipelines and streaming apps. In this project, it's used as a message broker between services.

- **Frontend**: The user interface of the application, built with React.

- **API Gateway**: A service that acts as a single entry point into the system, allowing multiple APIs or microservices to act cohesively and provide a uniform experience to the user.

- **Post Service**: A service responsible for handling all the operations related to posts in the application.

## Converting Docker Compose to Kubernetes

1. Navigate to the directory containing your `docker-compose.yml` file.

2. Run the `kompose convert` command. This will convert your Docker Compose file to files that you can use with `kubectl`.

```bash
kompose convert
```

You will see output similar to the following:
```bash
INFO Kubernetes file "zookeeper-service.yaml" created 
INFO Kubernetes file "kafka-service.yaml" created 
INFO Kubernetes file "frontend-service.yaml" created 
INFO Kubernetes file "api-gateway-service.yaml" created 
INFO Kubernetes file "post-service-service.yaml" created 
INFO Kubernetes file "zookeeper-deployment.yaml" created 
INFO Kubernetes file "kafka-deployment.yaml" created 
INFO Kubernetes file "frontend-deployment.yaml" created 
INFO Kubernetes file "api-gateway-deployment.yaml" created 
INFO Kubernetes file "post-service-deployment.yaml" created
```

## Deploying the Application to Kubernetes

3. Start a Kubernetes cluster with:
```Bash
minikube start
```

You will see output similar to the following:
```Bash
😄  minikube v1.32.0 on Microsoft Windows 10 Home 10.0.19045.4046 Build 19045.4046
✨  Using the docker driver based on existing profile
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
🔄  Restarting existing docker container for "minikube" ...
🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
🔎  Verifying Kubernetes components...
    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0
    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🌟  Enabled addons: storage-provisioner, default-storageclass, dashboard
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

4. Apply the generated Kubernetes configuration files using `kubectl apply -f ./`.

5. To access the frontend and API gateway on your local machine, you need to port forward the services:

```bash
kubectl port-forward service/frontend 3000:3000
kubectl port-forward service/api-gateway 8080:8080
```
This will allow you to access the frontend at `http://localhost:3000`, because the app is only accessable from within the cluster.

6. Open your web browser and navigate to `http://localhost:3000` to access the frontend.

## Kubernetes dashboard

To start the dashboard to view your Kubernetes deployment:

```Bash
minikube dashboard
```

You will see output similar to the following:

```Bash
🤔  Verifying dashboard health ...
🚀  Launching proxy ...
🤔  Verifying proxy health ...
🎉  Opening http://127.0.0.1:51621/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

## Cleaning Up
After you are finished testing out the application deployment, simply run the following command in your shell to delete the resources used.
`kubectl delete -f ./`

You can stop the minikube cluster with:
```Bash
minikube stop
```
