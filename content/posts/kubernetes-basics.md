---
title: "Kubernetes Basics: Understanding the Control Plane"
date: 2026-03-15
description: "A quick walkthrough of how the Kubernetes control plane components interact, with an architecture diagram."
tags: ["kubernetes", "cloud-native", "devops"]
categories: ["infrastructure"]
showToc: true
TocOpen: false
draft: false
---

Kubernetes orchestrates containerized workloads across a cluster of machines. Understanding how the control plane fits together is the first step to reasoning about scheduling, failures, and scaling.

## Control Plane Components

| Component | Role |
|---|---|
| `kube-apiserver` | Single entry point for all cluster operations |
| `etcd` | Distributed key-value store — the source of truth |
| `kube-scheduler` | Assigns pods to nodes based on resource constraints |
| `kube-controller-manager` | Runs controllers (ReplicaSet, Node, Job, etc.) |

## Architecture Diagram

{{< mermaid >}}
graph TD
    kubectl -->|REST| apiserver[kube-apiserver]
    apiserver -->|read/write| etcd[(etcd)]
    apiserver --> scheduler[kube-scheduler]
    apiserver --> controller[kube-controller-manager]
    scheduler -->|bind pod| apiserver
    controller -->|reconcile| apiserver

    subgraph Node
        kubelet --> apiserver
        kube-proxy --> apiserver
        kubelet --> container[Container Runtime]
    end
{{< /mermaid >}}

## What Happens When You Run `kubectl apply`

1. `kubectl` serializes your manifest and sends a `POST` to the API server.
2. The API server validates and persists the object to `etcd`.
3. The relevant controller (e.g. `ReplicaSet` controller) notices the new desired state and creates Pod objects.
4. The scheduler watches for unscheduled pods and assigns each one to a node.
5. The `kubelet` on that node pulls the image and starts the container.

## A Minimal Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello
  template:
    metadata:
      labels:
        app: hello
    spec:
      containers:
        - name: hello
          image: nginx:alpine
          ports:
            - containerPort: 80
```

Apply it with:

```bash
kubectl apply -f deployment.yaml
kubectl rollout status deployment/hello
```

## Next Steps

- Explore `kubectl describe` and `kubectl events` for debugging
- Learn about `ResourceQuota` and `LimitRange` for multi-tenant clusters
- Set up Horizontal Pod Autoscaling (HPA) based on CPU/memory metrics
