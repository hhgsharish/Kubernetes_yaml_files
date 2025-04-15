
# Step 1: Label your node (Pre-requisite)
Before applying your PersistentVolume, you need to ensure your node has the required label that matches your nodeAffinity selector.

    kubectl label nodes <your-node-name> env=dev
    
Why this step is needed: Your PersistentVolume specifies nodeAffinity that requires nodes with the label env=dev. Without this label, the PV won't be schedulable on any node.
# Step 2: Create the directory on your node
You need to create the directory that will be used for the hostPath volume:

# In Worker Node
# Create the directory with appropriate permissions 
    
    sudo mkdir -p /mnt/data
    sudo chmod 777 /mnt/data

Why this step is needed: The hostPath PV references /mnt/data on the node's filesystem. This directory must exist before the PV can be used.

# Step 3: Create the PersistentVolume
Now let's apply the PersistentVolume definition with comments:

    kubectl apply -f persistent-volume.yaml
    
Why this step is needed: This creates the PersistentVolume resource in Kubernetes, which represents a piece of storage that has been manually provisioned for use.

# Step 4: Create the PersistentVolumeClaim
Next, apply the PersistentVolumeClaim with comments:

    kubectl apply -f persistent-volume-claim.yaml
    
Why this step is needed: The PVC represents a request for storage by a user. It binds to an available PV that matches its criteria (storage class, access mode, and size).

# Step 5: Verify PV and PVC binding
Before proceeding, check that your PVC has successfully bound to the PV:

    kubectl get pv
    kubectl get pvc
    
Why this step is needed: You need to ensure the PVC is in "Bound" status before deploying applications that will use it.

# Step 6: Create the Jenkins Deployment
Now apply the Jenkins deployment with comments:

    kubectl apply -f jenkins-deployment.yaml
    
Why this step is needed: This creates the Jenkins deployment that uses the PVC for persistent storage.

# Step 7: Create a Jenkins Service (Missing Step)
You need to create a Service to access Jenkins from outside the cluster:

    kubectl apply -f jenkins-service.yaml
    
Why this step is needed: A Service provides a stable network endpoint to access the Jenkins pods, regardless of which node they're running on or if they're recreated.

# Step 8: Verify your deployment
Check that everything is running correctly:

    kubectl get pods
    kubectl get svc
    
Why this step is needed: It confirms that your Jenkins pods are running and the service is available.

# Step 9: Access Jenkins
You can now access Jenkins through the NodePort service:
http://<node-ip>:30080
Important Note on Volume Usage with Multiple Replicas:
Your deployment has replicas: 2, but you're using a ReadWriteOnce (RWO) PersistentVolume. This means:

Only one pod can write to the volume at a time
Both pods need to be scheduled on the same node where the PV is available
This could lead to conflicts since Jenkins wasn't designed for multiple instances sharing the same home directory

Consider changing to replicas: 1 for Jenkins, as it's typically run as a singleton service, or implement a proper Jenkins cluster configuration with a separate persistent volume for each instance.
