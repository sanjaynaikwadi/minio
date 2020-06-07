# Minio in Standalone mode installation

- Create the StorageClass

vi minio-sc.yaml

```yaml

kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: minio-sc
provisioner: kubernetes.io/portworx-volume
parameters:
  repl: "2"
```

`kubectl apply -f minio-sc.yaml`

- Create the StatefulSet

vi minio-sts.yaml

```yaml
apiVersion: apps/v1 #  for k8s versions before 1.9.0 use apps/v1beta2  and before 1.8.0 use extensions/v1beta1
kind: StatefulSet
metadata:
  # This name uniquely identifies the Deployment
  name: minio-sts
spec:
  selector:
    matchLabels:
      app: minio
  serviceName: minio-service    
  updateStrategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        # Label is used as selector in the service.
        app: minio
    spec:
      # Refer to the PVC created earlier
      #      volumes:
      # - name: storage
      #    persistentVolumeClaim:
          # Name of the PVC created earlier
      #    claimName: minio-pv-claim
      containers:
      - name: minio
        # Pulls the default Minio image from Docker Hub
        image: minio/minio:latest
        args:
        - server
        - /storage
        env:
        # Minio access key and secret key
        - name: MINIO_ACCESS_KEY
          value: "minio"
        - name: MINIO_SECRET_KEY
          value: "minio123"
        ports:
        - containerPort: 9000
          #   hostPort: 8000
        # Mount the volume into the pod
        volumeMounts:
        - name: storage # must match the volume name, above
          mountPath: "/storage"
  volumeClaimTemplates:
  - metadata:
      name: storage
      annotations:
        volume.beta.kubernetes.io/storage-class: minio-sc
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi

```

- Expose the Service via NodePort or LoadBalancer

vi minio-service-expose.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: minio-service
spec:
  type: NodePort
  ports:
    - port: 9000
      nodePort: 30000
  selector:
    app: minio
```

`kubectl apply -f minio-service-expose.yaml`
`

- Access Minio

`kubectl get svc` 

You can verify the service with NodePort is created and use one of the worker node IP to browse the URL e.g:
http://70.0.74.81:30000/minio/


