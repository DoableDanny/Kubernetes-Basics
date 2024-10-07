### Repository for the K8s in 1 hour video (https://www.youtube.com/watch?v=s_o8dwzRlu4)

This is an example Kubernetes project that creates a simple example Node app with MongoDB, and configures the environment with .env vars for the Node app, and configures the Mongo db with an initial user.

The project will look like this:

![example Kubernetes project structure](config.png)

#### K8s manifest files

- mongo-config.yaml
- mongo-secret.yaml
- mongo.yaml
- webapp.yaml

#### K8s commands

##### start Minikube and check status

    minikube start (ensure you've opened up Docker Desktop first)
    minikube status

##### get minikube node's ip address

    minikube ip

##### get basic info about k8s components

    kubectl get node
    kubectl get pod (see pods)
    kubectl get svc
    kubectl get all (see all components in the cluster -- but it doesn't show configMap and Secret, so:)
    kubectl get configmap
    kubectl get secret

#### Config Kubernetes

Apply command manages applications through files containing K8s resources. -f stands for file. Whatever is defined in the file is created, e.g. ConfigMap, Service, Deployment etc.:

kubectl apply -f mongo-congig.yaml
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f webapp.yaml

Notice that we created mongo config and secret first, because the mongo db depends on it, and we create the mongo db before the webapp, because the webapp depends on it.

##### get extended info about components

    kubectl get pod -o wide
    kubectl get node -o wide (can also get the cluster ip with this as well as `minkube ip`)

##### get detailed info about a specific component

    kubectl describe svc {svc-name}
    kubectl describe pod {pod-name}

##### get application logs

    kubectl logs {pod-name}
    e.g. kubectl logs webapp-deployment-7f479b7c79-v828v => `app listening on port 3000!`

    You can also stream the logs with -f option:
    kubectl logs webapp-deployment-7f479b7c79-v828v -f

#### Access app in browser

1. Find what public ip the cluster is available on with `kubectl get node -o wide` => INTERNAL-IP or `minikube up`.
2. Find the NodePort number: look inside your webapp's service config for nodePort, or `kubectl get svc`
3. Access MinikubeIP:NodePort in the browser, e.g. http://192.168.49.2:30100/

Or, if that doesn't work:

##### stop your Minikube cluster

    minikube stop

<br />

> :warning: **Known issue - Minikube IP not accessible**

If you can't access the NodePort service webapp with `MinikubeIP:NodePort`, execute the following command:

    minikube service webapp-service

<br />

#### Links

- mongodb image on Docker Hub: https://hub.docker.com/_/mongo
- webapp image on Docker Hub: https://hub.docker.com/repository/docker/nanajanashia/k8s-demo-app
- k8s official documentation: https://kubernetes.io/docs/home/
- webapp code repo: https://gitlab.com/nanuchi/developing-with-docker/-/tree/feature/k8s-in-hour
