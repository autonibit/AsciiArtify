# AsciiArtify. Concept

> Comparative analysis of tools for local Kubernetes cluster deployment

## Introduction

Minikube, Kind, and K3d - three popular tools for running Kubernetes locally.

**Minikube**:

- Creates a single-node Kubernetes cluster running inside a VM on your local workstation. The VM can be VirtualBox, VMware, etc.
- Easy way to test Kubernetes features or develop apps against a Kubernetes cluster on your laptop.

**Kind (Kubernetes in Docker)**:

- Creates Kubernetes cluster using Docker containers as nodes.
- The containers communicate together to form a cluster on a single machine. More lightweight than minikube.

**k3d**:

- Also creates Kubernetes cluster using Docker containers like kind, but with a focus on ease of use and additional features for networking, volumes, registries etc.
- Made to be simple to use and configure for local development.

## Features


| Specs               | Minikub                                                                                                                                                              | Kind                                                                                                                                                                  | K3d                                                                                                                                                                                                     |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Supported OS        | Minikube supports various operating systems and architectures, including Linux, macOS, Windows.                                                                      | Kind supports various operating systems and architectures.                                                                                                            | K3d also supports various operating systems and architectures.                                                                                                                                          |
| Automation          | Minikube clusters can be automated and configured via the CLI and YAML files. Supports running custom scripts before or after clusters are set up to install addons. | Main automation is through Docker containers and configuration YAML files passed via the CLI tool. Additional automation requires scripting against the CLI commands. | Provides both CLI flags as well as YAML config files for complete control over clusters. Added automation support through Terraform and Ansible modules to allow declarative infrastructure automation. |
| Additional features | Minikube offers additional features such as Kubernetes cluster monitoring, which could be useful in managing and troubleshooting the cluster.                        | Kind has limitations regarding additional features, such as cluster monitoring, which might affect its utility for complex deployments.                               | K3d provides additional features such as Kubernetes cluster monitoring, which can aid in managing and troubleshooting the cluster.                                                                      |

## Advantages and disadvantages


| Criterials                  | Minikub                                                                                                                                                                                     | Kind                                                                                                                                                                                                                                                                            | K3d                                                                                                                                                                                                                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Ease of use                 | Minikube can be difficult to configure and use, especially for beginners, due to its complexity and the number of features it supports.                                                     | Kind is simple to use, making it more accessible for beginners or those who want a straightforward setup process.                                                                                                                                                               | K3d is also simple to use, offering an intuitive and straightforward setup process, which is beneficial for beginners and experienced users alike.                                                                                                                              |
| Deployment speed            | Minikube supports automation, which can significantly speed up the deployment process, especially for complex configurations and large-scale deployments.                                   | Kind is quick to deploy, making it an ideal choice for developers who need to rapidly set up and tear down Kubernetes clusters.                                                                                                                                                 | K3d is also quick to deploy, which can be beneficial for developers who need to frequently create and delete Kubernetes clusters for testing purposes.                                                                                                                          |
| Stability                   | Minikube is generally stable, but it may have issues with certain configurations. It's necessary to carefully check the compatibility of your specific configuration before using Minikube. | Kind is stable, providing a reliable environment for running Kubernetes clusters. This stability is beneficial for developers who need a consistent and reliable platform for their work.                                                                                       | K3d may be less stable than the other tools, potentially leading to issues in certain scenarios or configurations. It's important to thoroughly test K3d in your specific environment before relying on it for critical tasks.                                                  |
| Documentation and community | Minikube has active community support and extensive documentation, making it easier to troubleshoot issues and learn how to use the tool effectively.                                       | Kind also has active community support and extensive documentation, providing ample resources for learning and troubleshooting.                                                                                                                                                 | K3d may have less community support, which could make it harder to find solutions to issues or learn how to use the tool effectively.                                                                                                                                           |
| Configuration and usage     | Minikube supports various configurations and use cases, but may be complex for beginners. It's suitable for advanced users who need to customize their Kubernetes clusters extensively.     | Kind supports various configurations and use cases, offering more flexibility for different types of projects.                                                                                                                                                                  | K3d supports various configurations and use cases, offering flexibility while maintaining ease of use.                                                                                                                                                                          |
| Scalability                 | Minikube can be used for testing applications on a local machine before deploying them on a large cluster, making it a good choice for scalability testing.                                 | Kind is ideal for working with Kubernetes in Docker but may not support all features provided by Minikube, potentially limiting its scalability.                                                                                                                                | K3d is ideal for quickly deploying and testing applications on Kubernetes, but its scalability may be limited by its potential stability issues.                                                                                                                                |
| Usage with Podman           | Podman can be used as the container runtime within minikube clusters instead of Docker. This allows using Podman for building container images and running them on minikube.                | The toll rely on Docker rather than a stand-alone container runtime like Podman. This makes it harder to substitute Podman for managing the cluster nodes. However, once the cluster is running, Podman could potentially be installed within nodes to build and deploy images. | The toll rely on Docker rather than a stand-alone container runtime like Podman. This makes it harder to substitute Podman for managing the cluster nodes. However, once the cluster is running, Podman could potentially be installed within nodes to build and deploy images. |

## Demo

[![asciicast](https://asciinema.org/a/237vI4ZNyLIrGJqwkQ9ZeWy4q.svg)](https://asciinema.org/a/237vI4ZNyLIrGJqwkQ9ZeWy4q)

1. Install current latest release

* wget:

  ```sh
  wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
  ```
* curl:

  ```sh
  curl -s https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash
  ```

2. Create a cluster named `test-cluster` with just a single server node:

   ```sh
   k3d cluster create test-cluster
   ```
3. Deploy an example hello-world app:

   ```sh
   kubectl create deployment hello-world --image=gcr.io/google-samples/hello-app:1.0
   ```
4. Check pod

   ```sh
   kubectl get pods
   ```
5. Check deploys

   ```sh
   kubectl get deploy
   ```
6. Expose the pod to external network on port 8080:

   ```sh
   kubectl expose deployment hello-world --type=LoadBalancer --port=8080
   ```
7. Check created service

   ```sh
   kubectl get service hello-world
   ```
8. Port-forward

   ```sh
   kubectl port-forward deployments/hello-world 8080&
   ```
9. Check avaliability:

   ```sh
   curl localhost:8080
   ```
10. Delete test-cluster

    ```sh
    k3d cluster detete test-cluster
    ```

## Summary

k3d is designed specifically for running Kubernetes in local development environments with a focus on usability and developer productivity. The built-in support for infrastructure coding and portable architecture make it an ideal choice for most workflows compared to minikube or kind. I would recommend using k3d to Achi as it offers the best balance of ease-of-use and flexibility for running local Kubernetes clusters.
