## Instructions

## 1. Install ebs csi driver

	helm repo add aws-ebs-csi-driver https://kubernetes-sigs.github.io/aws-ebs-csi-driver/
	helm repo update
	
	helm upgrade --install aws-ebs-csi-driver aws-ebs-csi-driver/aws-ebs-csi-driver \
	  --namespace kube-system \
	  --set controller.serviceAccount.create=true \
	  --set controller.serviceAccount.name=ebs-csi-controller-sa
  
## 2. Add Policies to the role for master, worker nodes. 
**AmazonEBSCSIDriverPolicy**

![image](https://github.com/user-attachments/assets/5d34d0b8-dbec-4bc8-a1bc-115f73e63bb0)

## 2. Open All Inbound Ports for master and worker nodes 

## 3. Commands

	  kubectl delete -f dynamic-pvc.yaml 
	  kubectl delete -f jenkins-deployment.yaml 
	  kubectl delete -f jenkins-service.yaml 
	  kubectl delete -f storage-class.yaml 

	  kubectl apply -f storage-class.yaml 
	  kubectl apply -f dynamic-pvc.yaml 
	  kubectl apply -f jenkins-deployment.yaml 
	  kubectl apply -f jenkins-service.yaml 

	  kubectl describe pvc 
