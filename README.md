Manifests for easy deployment to a GKE Autopilot cluster, using kustomize  
Relies on several GKE only features, not portable to other providers

## Prerequisites
GKE autopilot cluster started and `kubectl` ready  
Domain acquired

## Pre-Deploy
Acquire SSL certificate through gcloud  
`gcloud computer ssl-certificates create $CERTNAME --domains=$DOMAIN --global` 

## Deploy
One-shot deploy, run in base directory  
`kubectl apply -k ./`   
This can also be used to deploy minor changes, e.g. container version change  
A larger change will require one of the many Kubernetes patching features I think

## Post-Deploy
Grab your newly started Gateway's IP   
`kubectl get gateway`   
Add that as A record in your DNS   
Wait a few hours  
At that point gcloud should provision an SSL for your domain and hand it to the Gateway  

## Destroy
This is a fun one  
`kubectl delete -k ./`  
Destroys everything! 

## Unobfuscated PGPASSWORD?
Database is only accessible from within cluster  
Just not a big deal in this case