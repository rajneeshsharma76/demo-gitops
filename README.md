# gitopsdemo How TO Use this Demo 
A) Prerequisites

1>Ensure that you have a running Kubernetes cluster 

2>The cluster-admin role to deploy Flux CD.


B) Install fluxctl:-----
 
 Download the latest release of fluxctl and move it to the /usr/bin directory.

wget https://github.com/fluxcd/flux/releases/download/1.19.0/fluxctl_linux_amd64

mv fluxctl_linux_amd64 /usr/bin/fluxctl

sudo chmod +x /usr/bin/fluxctl

For this Demo, let’s use GitHub as the source code repository. 

Fork the rajneeshsharma76/gitopsdemo repository within your GitHub account.


3)Provide the name of your GitHub user within the GHUSER As an environment variable.
  
  export GHUSER=yourusername
  
  
4)Create Namespace kubectl create ns flux 


5)  Install fux on Kubernetes 

fluxctl install --git-user=${GHUSER} --git-email=${GHUSER}@users.noreply.github.com --git-url=git@github.com:${GHUSER}/gitopsdemo --git-path=app,ns --namespace=flux | kubectl apply -f -


6) Check the deployment Status
   
   kubectl -n flux rollout status deployment/flux
   
   Output should come Like This    deployment "flux" successfully rolled out


7)Get all resources within the flux namespace to see the current state of the object. you can see, there is a flux pod and a memcached pod. There is also a memcached service as the flux pod needs to interact with it. Run below command to check
  
  kubectl get all -n flux


 8)Authorise Flux CD to Connect to Your Git Repository
    
    genrate public SSH key using fluxctl
   
   A> fluxctl identity --k8s-fwd-ns flux 
        
        and copy the output 
         
         
    B> Go to Repository Setting and in Add deploy key section add this key and give read/write Permission 

9) Flux CD synchronises automatically with the configured Git repository every five minutes. However, if you want to synchronise Flux with the Git repo           immediately, you can use fluxctl sync

 fluxctl sync --k8s-fwd-ns flux   This Command will Sync your changes and deploy it to Kubernetes Cluster 
 
 
 10) To Deploy Changes 
  a)make any change in index.hmtl .
  
  b)Build and  the Image .Push it to your Iamge repository .
  
  c)change the new ImageTag in Deployment.yaml.
  
  d)After 5 min Flux will automatically deploy the new image in Clsuter 
 
    
