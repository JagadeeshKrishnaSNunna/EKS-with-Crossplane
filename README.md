# Manage Infrastructure with Kubernetes using GitOps.
### Prerequisits
  Set up kubetnetes cluster using any one of the following:
    Minikube
    Microk8s
    Kind

### Tools and Addons
  1. [Crossplane](https://crossplane.io/docs/v1.7/) is a Cloud Native Compute Foundation project. Crossplane is an open source Kubernetes add-on that            transforms your cluster into a universal control plane. Crossplane extends your Kubernetes cluster to support orchestrating any infrastructure or          managed service. Compose Crossplane’s granular resources into higher level abstractions that can be versioned, managed, deployed and consumed using        your favorite tools and existing processes.
     #### Installation:
     ```
     kubectl create namespace crossplane-system
     
     helm repo add crossplane-stable https://charts.crossplane.io/stable
     helm repo update

     helm install crossplane --namespace crossplane-system crossplane-stable/crossplane

     ```
     #### Check Crossplane Status
     ```
     helm list -n crossplane-system

     kubectl get all -n crossplane-system
     
     ```  
     ####Install Crossplane CLI
     ```
     curl -sL https://raw.githubusercontent.com/crossplane/crossplane/master/install.sh | sh
     sudo mv kubectl-crossplane /home/<username>/bin #(promted in output of previous command)
     ```
     (**NOTE:** we use AWS Cloud Service Provider. Crossplne has API to work with [other-providers](https://crossplane.io/docs/v1.7/api-docs/overview.html))
     
  2. [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
