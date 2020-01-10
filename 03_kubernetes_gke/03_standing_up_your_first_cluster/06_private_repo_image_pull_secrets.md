# Private Repo Image Pull Secrets

In Kubernetes, we use a special command to create an `ImagePullSecret` that we can reference in our deployments that use private images.

```
$ kubectl create secret docker-registry regcred \
    --docker-server=<your-registry-server> \
    --docker-username=<your-name> \
    --docker-password=<your-pword> \
    --docker-email=<your-email>
```

The above will automatically create a secret named `regcred`, which you could add to `imagePullSecrets` as part of your k8s resource definitions. The contents of this secret is a paramaterized `~/.docker/config.json` file.

Example deployment with `imagePullSecret` defined:

```
apiVersion: apps/v1 
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: myprivaterepo/nginx:1.7.9
        ports:
        - containerPort: 80
    imagePullSecrets:
    - name: regcred
```

This could be applied via `kubectl`, and will work if the command at the top of this readme is run first.

This secret can also be applied the same way to Pod definitions, StatefulSet definitions, DaemonSet definitions, etc. 


## Google Container Registry

Google Kubernetes Engine uses the service account configured on the VM instances of cluster nodes to push and pull images.

If the Google Kubernetes Engine cluster and the Container Registry storage bucket are in the same Google Cloud project, the Compute Engine default service account is configured with the appropriate permissions to push or pull images. If the cluster is in a different project or if the VMs in the cluster use a different service account, you must configure access to the storage bucket used by the repository.

By default, a Compute Engine VM has the read-only access scope configured for storage buckets. To push private Docker images, your instance must have read-write storage access scope configured as described in Access scopes.

