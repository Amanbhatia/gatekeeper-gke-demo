# gatekeeper-gke-demo
Repository to host the gatekeeper and kubernetes demo scripts for Clearwater Devcon 2021

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
1. Deploying gatekeeper using a pre-built image.
      ```
      kubectl apply -f https://raw.githubusercontent.com/open-policy-agent/gatekeeper/release-3.5/deploy/gatekeeper.yaml 
      ```
     
     ![image](https://user-images.githubusercontent.com/3711545/141451067-48f83ba5-5766-42e6-8ac3-ea7c832b57ff.png)
2. Deploying pod(with missing nodeselector) without constraint.
   * Before applying OPA Policies, try deploying a dummy pod in the cluster to verify if there is any policy that may restrict the creating of Pod. t should successfully be deployed on the cluster in Running state.
   
      ```
      kubectl apply -f devcon.yaml
      kubectl get all -n devon (to check if everything is in running state or not)

      ```
    * Above should successfully be deployed on the cluster in Running state  
3. Lets deploy OPA policies using Constraint Template.
   * Install constraint template
      ```
      kubectl apply -f nodeSelector-template.yaml
      ```
   * Install the constraint
      ```
      kubectl apply -f nodepool-nodeSelector.yaml
      kubectl get constraints (list all the constraints in the cluster)
      ```
      **NOTE :** The constraint applied in the above steps checks for the label 'node_pool' as a key on the node (compute instance) of the cluster which should match the key of NodeSelector parameter in deployment file.
    
4. Deploying pod(with missing nodeselector) with constraint present in cluster.
   * Now delete the existing deployment (deployed through devon.yaml) and re-deploy it.
       ```
       kubectl delete -f devcon.yaml
       kubectl apply -f devcon.yaml
       ```
   * Above should throw the error as depicted below. This error shows that there is constraint that is restricting deployment to be created successfully.  
      ![image](https://user-images.githubusercontent.com/3711545/141470927-a860633b-b9c0-4411-9269-bc07f6716d9a.png)

5. Deploying pod(with nodeselector) with constraint present in cluster.
   * create the deployment using the corrected manifest
      ```
       kubectl apply -f devcon_corrected.yaml
       kubectl get all -n devcon
      ```
   * The deployment should be successfully created and should be in running state.
