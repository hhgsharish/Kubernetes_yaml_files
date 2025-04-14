  kubectl create namespace my-jenkins

  kubectl apply -f jenkins-deployment.yaml


  kubectl apply -f jenkins-service.yaml

Accessible in  <Public IP> : 30008

![image](https://github.com/user-attachments/assets/b71d1d34-20b8-4c8a-886e-016e12c2db0d)

  kubectl get pods -A
  
  kubectl logs jenkins-deployment-67f8b96b5c-b4jbh -n my-jenkins
