# GKE

Google Provides a managed Kubernetes cluster service 'Kubernetes Engine', that you can utilize to deploy your workloads.

In this section, we will walk through setting up an GKE cluster, which you can use to experiment in the coming sections and demonstrations.

We will use the GCP GKE Console for this section. The equivalent terminal commands will also be displayed for completeness.


## Pre-Reqs

1. You need to have access to an existing GCP account.
2. GCP Project has been created for this workshop.
3. GKE API has been opened up by an Administrator on the project.

## Steps in Console

1. Log into the GCP Dashboard

	![image14](gke_images/image-14.png)
	
2. Navigate to GKE Console

    ![image-3](gke_images/image-3.png)
    
3. Click on **Create New Cluster**

    ![image-4](gke_images/image-4.png)
    
4. Fill in the appropriate information in **Create a Kubernetes cluster Form**
    
    Open and edit the node pool configuration.  We are going to use ubuntu for our nodes.  This will allow us to upgrade docker to 
    greater than 17.05 CE and allow us to conduct multi-stage builds later in the workshop.
    
    ![image-19](gke_images/image-19.png)
    
    Click on **Save**
    
    ![image-8](gke_images/image-8.png)
    
    Continue to fill in the Create Cluster Form and click on **Create**
    
5. Navigate to **Clusters** in GKE Console

    ![image-7](gke_images/image-7.png)
    
    Here you can see our running Kubernetes 3 node cluster which is now ready to deploy workloads.

## Steps in Gcloud Terminal (Optional)

The gcloud terminal also provides a convenient avenue to deploy/configure GKE clusters.  In this _`optional`_  step we will
spin up a similar cluster using he command line.  This apprach is prefrered and easy to automate.

1. Log into the GCP Dashboard

    ![image14](gke_images/image-14.png)
	
2. in the upper right corner of console click on the following icon    
 
    ![image-2](gke_images/image-2.png)
    
    This will open up the gcloud terminal window.
    
3. **Type or Copy** the following in the terminal

    `gcloud container clusters create gke-workshop-gcloud --zone us-west2-a --labels=createdBy=jdawson --machine-type=n1-standard-2`

    The cluster will spin up in after a short delay and display it's running state.
    
    ![image-12](gke_images/image-12.png)

   
## Validate

1. Navigate to Clusters section of GKE Console

    ![image-3](gke_images/image-3.png)

    In the image below you can view both the GKE clusters you have created. One by way of console and one via gcloud terminal.
    
    ![image-13](gke_images/image-13.png)
    
    
2. You can also verify your deployment by looking at the Kubernetes system pods. Open up the gcloud console again by selecting the
   following icon
   
    ![image-2](gke_images/image-2.png)
    
3. **Type of Copy**  the following in the terminal

    `kubectl get pods --all-namespaces`

    You should see output similar to the below image
    
    ![image-15](gke_images/image-15.png)
    
    Since we haven't deployed any user workloads at this point, the `--all-namespaces` flag includes the system namespace
    where kubernetes stores all of it's objects.
    
## Delete Optional Cluster

1. Navigate to Clusters section of GKE Console

   ![image-3](gke_images/image-3.png)
    

2. `Select` Checkbox next to **gke-workshop-gcloud** and `Click` trashcan icon.

   ![image-13](gke_images/image-13.png)
   
    
## Enable NAT for Private Clusters

```
gcloud compute routers create nat-router \
    --network default \
    --region us-central1

gcloud compute routers nats create nat-config \
    --router-region us-central1 \
    --router nat-router \
    --nat-all-subnet-ip-ranges \
    --auto-allocate-nat-external-ips
```

It may take up to 3 minutes for the NAT configuration to propagate, so wait at least a minute before trying to access the Internet again.


## Continuing

The next sections wll explain kubectl, and give you a chance to flex some of these actions against your GKE cluster.
