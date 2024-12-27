+++
title = "Docker, Kubernetes and the most popular container runtime"
date = "2024-12-27"
description = "Containerd is the most popular container runtime and is being used in both Docker & Kubernetes"

[taxonomies]
tags = ["docker", "kubernetes", "container"]
+++

Containerd is an industry-standard container runtime with an emphasis on simplicity, robustness, and portability. It is available as a daemon for Linux and Windows, which can manage the complete container lifecycle of its host system: image transfer and storage, container execution and supervision, low-level storage and network attachments, etc.

## What is the most popular container runtime used in Docker?

The most popular container runtime used with Docker is containerd. Docker actually transitioned from using its own runtime (called runc) to containerd as the default runtime engine. Containerd was originally developed by Docker Inc. but was later donated to the Cloud Native Computing Foundation (CNCF) to ensure vendor neutrality and wider adoption.

Containerd serves as a core container runtime that handles container lifecycle operations like pulling images, managing storage, and executing containers. It's also used by other container platforms beyond Docker, including Kubernetes, which demonstrates its widespread adoption in the container ecosystem.

While containerd is the high-level runtime, it actually uses runc (which is also open source) under the hood as the low-level runtime that interfaces directly with the Linux kernel to create and run containers. This combination of containerd and runc provides a robust and efficient container runtime environment.

## What is the most popular container runtime used in Kubernetes?

For Kubernetes, containerd has become the most popular and recommended container runtime. This shift happened particularly after Docker's Container Runtime Interface (CRI) implementation called dockershim was deprecated and removed from Kubernetes in version 1.24 (released in 2022).

The main container runtimes used in Kubernetes are:

1.  containerd - Now the most widely adopted runtime, offering direct CRI support and better performance than the old dockershim approach. Many major Kubernetes distributions and cloud providers use containerd as their default runtime.
2.  CRI-O - Another popular runtime developed specifically for Kubernetes by Red Hat. It's a lightweight alternative that's fully compatible with the CRI specification and is the default runtime in OpenShift.
3.  crun - A newer runtime written in C (instead of Go like most others) that's gaining popularity for its lower memory footprint and faster startup times, though it's not as widely deployed as containerd or CRI-O.

While Docker itself can still be used on Kubernetes nodes (with a CRI adapter like cri-dockerd), most production deployments have moved to containerd or CRI-O for better performance and direct CRI support.

## What does it mean when people say Kubernetes can use Docker as its container runtime?

When people say Kubernetes can use Docker as its container runtime, it's actually a bit of a mischaracterization. Let me explain what's really happening:

In the early days of Kubernetes, the cluster used something called "dockershim" which was a special adapter that allowed Kubernetes to communicate with Docker, which would then use containerd to actually run the containers. This created a somewhat inefficient chain:

Kubernetes -> dockershim -> Docker -> containerd -> containers

This is why when people said "Kubernetes is using Docker as its runtime," it wasn't technically accurate - Docker was more of a middle layer than the actual runtime. The containers were still being run by containerd.

Today, this setup is no longer recommended or supported in newer versions of Kubernetes (it was removed in v1.24). If someone wants to use Docker with modern Kubernetes, they need to use cri-dockerd, which is a separate adapter that maintains compatibility. However, most users now skip Docker entirely and have Kubernetes talk directly to containerd or another CRI-compatible runtime like CRI-O:

Modern approach: Kubernetes -> containerd -> containers

This direct approach is more efficient and has less overhead. It's worth noting that even though Kubernetes might not be "using Docker," you can still use Docker to build your container images - the runtime choice only affects how those containers are executed in the cluster.

