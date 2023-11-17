## Notes
When running k3d to build your cluster and using persistent volumes
you need to configure the following

1. create cluster via k3d and pass in the -v flag to mount local volume into docker container for nodes
2. create a PV using local-path storage class and define your path use local instead of hostpath
   '''yaml
        local:
         path: "/data"
   '''
   also the value in the path field must be the path that is mapped into your docker container for node. 
   When use local instead of hostpath you must also include nodeAffinity

When using the k3d dynamic provisioner the default local for the created pv is 
/var/lib/rancher/ks3/storage inside the docker node. so you when creating the cluster you need to map your volume as "$HOME/some/directory:/var/lib/rancher/k3s/storage@all". The dynamic provisioner will create a folder that with the same name as the pvc that is created in the cluster. 
From the local host any files that are put into the pvc folder will be see in the pod you that is using the pvc.

https://k3d.io/v5.4.6/usage/k3s/ 

https://github.com/k3d-io/k3d/discussions/787