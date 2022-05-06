# Manage Infrastructure with Kubernetes using GitOps.
### Prerequisits
  1. Set up kubetnetes cluster using any one of the following:
      Minikube
      Microk8s
      Kind
  2. Cloud provider credentials(AWS,GCP,AZURE,ALIBHABA,etc)

### Tools and Addons
  1. [Crossplane](https://crossplane.io/docs/v1.7/) is a Cloud Native Compute Foundation project. Crossplane is an open source Kubernetes add-on that            transforms your cluster into a universal control plane. Crossplane extends your Kubernetes cluster to support orchestrating any infrastructure or        managed service. Compose Crossplane’s granular resources into higher level abstractions that can be versioned, managed, deployed and consumed            using your favorite tools and existing processes.
  
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
     (**NOTE:** we use AWS Cloud Service Provider. Crossplne has API to work with [other-providers](https://crossplane.io/docs/v1.7/api-docs/overview.html). [Install](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Configure](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) AWS-CLI-2)
     
     #### SetUP Provider
     Open Terminal in the project directory.
     ```
     AWS_PROFILE=default && echo -e "[default]\naws_access_key_id = $(aws configure get aws_access_key_id --profile $AWS_PROFILE)\naws_secret_access_key = $(aws configure get aws_secret_access_key --profile $AWS_PROFILE)" > creds.conf
     
     kubectl create secret generic aws-creds -n crossplane-system --from-file=creds=./creds.conf
     ```
     Find provider.yaml file in setUp directory.
     ```
     kubectl apply -f provider.yaml
     ```   
     Wait till the package is healthy. You can check it using :
     ```
     watch kubectl get pkg
     ```
     once package is ready create Provider config. Find file ProviderConfig.yaml in setup directory.
     ```
     kubectl apply -f providerConfig.yaml
     ```
     
     #### Other usefull commands
     1. list resources managed
         ``` 
         kubectl get managed
         ```
     2. get all resources 
        ```
        kubectl get crossplane
        ``` 
     
     
   2. [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) is a Kubernetes-native continuous deployment (CD) tool. Unlike external CD tools that only            enable push-based deployments, Argo CD can pull updated code from Git repositories and deploy it directly to Kubernetes resources. It enables              developers to manage both infrastructure configuration and application updates in one system.
    
      #### Installation:
      ```
      kubectl create namespace argocd
      kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
      ```
      wait till all the pods are available.
      ```
      watch kubectl get pods -n argocd
      ```
      ####Port Forwarding
      ```
      kubectl port-forward svc/argocd-server -n argocd 8080:443
      ```
      ####Working with Argo CD from the Command Line
      ```
      bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
      brew install argocd
      ```
      #### login to ArgoCD
      using web-ui at ```localhost:8080```
      **or**
      CLI
      ```
      argocd login localhost:8080
      ```
      username: admin
      password: run command below.
      ```
      kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
      ```
      #### Running Application
      ```
      kubectl apply -f application.yaml
      ```
      ###**NOTE** If you are using Private Repository check steps to follow [here](https://argo-cd.readthedocs.io/en/release-1.8/user-guide/private-repositories/).
    
    
    
    
    
    
    
    
    
