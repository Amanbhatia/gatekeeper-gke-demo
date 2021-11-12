# gatekeeper-gke-demo
Repository to host the gatekeeper and kubernetes demo scripts for cw devcon 2021

## **Runbook**
## Prerequisites
* Kubernetes Cluster with minimum version compatibility
  * Gatekeeper requires Kubernetes resources introduced in v1.16
  * The minimum supported Kubernetes version for Gatekeeper is n-4 of the latest stable release of Kubernetes.
  * Cluster Admin permissions are required for installing Gatekeeper. Below command can be used o create the cluster role binding.
      ```
      kubectl create clusterrolebinding cluster-admin-binding --clusterrole cluster-admin --user <YOUR USER NAME>
      ```
## Implementation
* Deploying gatekeeper using a pre-built image.
      ```
      kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml 
      ```
     
     ![image](https://user-images.githubusercontent.com/3711545/141451067-48f83ba5-5766-42e6-8ac3-ea7c832b57ff.png)
* Applying OPA Policies
  * Before applying OPA Policies, try deploying a dummy pod in the cluster to verify if there is any policy that may restrict the creating of Pod. t should successfully be deployed on the cluster in Running state.
      ```
      kubectl apply -f devcon.yaml
      kubectl get all -n devon (to check if everything is in running state or not)

      ```
