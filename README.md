# Manage Infrastructure with Kubernetes using GitOps.
### Prerequisits
  1. Set up kubetnetes cluster using any one of the following:
      Minikube
      Microk8s
      Kind
  2. Cloud provider credentials(AWS,GCP,AZURE,ALIBHABA,etc)

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
     (**NOTE:** we use AWS Cloud Service Provider. Crossplne has API to work with [other-providers](https://crossplane.io/docs/v1.7/api-docs/overview.html).[Install](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) and [Configure](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html) AWS-CLI-2)
     
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
         ``` kubectl get managed```
     2. get all resources 
        ```kubectl get crossplane``` 
     
     
  2. [ArgoCD](https://argo-cd.readthedocs.io/en/stable/)
