### Storage  ######

There are multiple types of storages.
1.emptyDirectory
2.Hostpath
3.static provisioning
4.dynamic provisioning

emptyDirectory:

1. it is ephemeral(temporary)
2. it is an kubernetes ephemeral volume which uses sidecar pattern mount the volume from main container and sidecar container   so that main container will write the files and sidecar container will access the log files and ship to the elastic search or any external system
3.  it is used to ship the pod log files 

Hostpath:
1. it is also ephemeral
2. daemon-set--> it will create each pod in each worker node and it will access all the files
3. hostpath--> it is used to access the storage of the server to pod
4. fluentd ---> it is used to ship the files from server to the external system

static provisioning:
1. it is nothing but attaching harddisk to our computer
2. we need to create one EBS volume in cloud and attach to eks server then all the files will store in EBS, if anytime pod gets deleted then easily mount from there

steps:
1. Create a EBS volume
2. install the EBS driver
3. create a PV(persistent volume)
4. make sure EC2 can access EBS
5. create PVC (persistent volume claim)
6. need to create a volume and attach to the pod


Dynamic provisioning:
1. here the storage class will create EBS volume and PV automatically

EFS:
1.it means elastic network storage and elastic file system
2.EBS will not scale automatically, but EFS will scale automatically (if memory exceeds the limit)

EFS static provisioning:
steps:
1. create EFS file system
2. edit the security group to allow traffic
3. install the EFS csi driver
(kubectl kustomize \
    "github.com/kubernetes-sigs/aws-efs-csi-driver/deploy/kubernetes/overlays/stable/ecr/?ref=release-1.X" > private-ecr-driver.yaml)
4. give access to ec2 to access efs
5. create pv, pvc and mount to pod

EFS dynamic provisioning:
steps:
1. create EFS file system
2. edit the security group to allow traffic
3. install the EFS csi driver
4. give access to ec2 to access efs
5. create pvc and mount to pod