# Kubernetes Pods and Deployments

This project contains two simple examples that run the NGINX web server in Kubernetes.

## What is a Pod?

A **Pod** is the smallest unit in Kubernetes. It is a wrapper around one or more containers. Containers inside a Pod share a network and can communicate easily.

Think of a Pod as one small running unit:

```text
Pod
└── NGINX container
```

The [nginx-pod.yaml](nginx-pod.yaml) file creates one Pod named `nginx-pod`. It:

- Uses the `nginx:latest` image
- Runs one NGINX container
- Makes port `80` available inside the container

### Run the Pod

```bash
kubectl apply -f nginx-pod.yaml
```

### Check the Pod

```bash
kubectl get pods
kubectl describe pod nginx-pod
```

## What is a Deployment?

A **Deployment** manages Pods for you. It creates and maintains a desired number of Pod replicas and replaces Pods that fail or are deleted.

The [nginx-deployment.yaml](nginx-deployment.yaml) file creates three copies, or replicas, of the NGINX Pod.

```text
Deployment
├── nginx Pod 1
├── nginx Pod 2
└── nginx Pod 3
```

It tells Kubernetes:

- Create `3` replicas
- Find Pods with the label `app: nginx`
- Create each Pod from the template in the file
- Use the `nginx:latest` image and port `80`

### Run the Deployment

```bash
kubectl apply -f nginx-deployment.yaml
```

### Check the Deployment

```bash
kubectl get deployments
kubectl get pods
kubectl describe deployment nginx-deployment
```

## Pod vs Deployment

| Pod                                            | Deployment                                    |
| ---------------------------------------------- | --------------------------------------------- |
| Runs containers directly                       | Manages Pods automatically                    |
| Usually not self-healing when deleted manually | Recreates missing or failed Pods              |
| Simple for learning or one-time use            | Better for applications that need reliability |
| Can run one or more containers                 | Can maintain multiple replicas                |

## Try Scaling

Change `replicas` in `nginx-deployment.yaml`, then apply the file again:

```yaml
replicas: 5
```

```bash
kubectl apply -f nginx-deployment.yaml
kubectl get pods
```

## Clean Up

Delete the examples when finished:

```bash
kubectl delete -f nginx-pod.yaml
kubectl delete -f nginx-deployment.yaml
```
