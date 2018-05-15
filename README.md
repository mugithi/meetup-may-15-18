# PLAN OF THE DAY 

## Address 

[675 Campbell Technology Parkway, Campbell CA, Suite 100](https://goo.gl/maps/RN8dsQekX352)

## Wireless

- SSID: dt-meetup
- Password: *********


## Agenda

**5:30 PM: Social Networking:**

**6:00 – 6:30 PM: Isaack**

- Different container storage options available today (Q1 2018) for the - enterprise storage administrator
- The current state of Kubernetes Project and Storage (Storage SIG)
- Kubernetes Flex Volumes and CSI
- Helm chart - on-prem variable - cloud variable
- Demo of Kubernetes Flex Volumes with an Enterprise Storage Array
- Dynamic provisioning
- Cloning workflow of production MYSQL databases for test & dev use case

**7:00 - 7:30: Josh**
- Managing Kubernetes on AWS the easy way

## Pizza and Beer will be provided

## At the end of the meetup we will be raffling away Google AYI kits


# MEETUP DORY IMPLEMENTATION

### 1. Installation documentation 

[Link to Isaack's guide Installation documentation](https://docs.google.com/document/d/1g8EPzMipes3Z2tHblRY_2PpxQUc8nABWoa6uVSll4s0/edit?usp=sharing)

### 2. Installed components 

- The kube-storage-controller(FlexVolume)

```bash
kubectl get po --namespace=kube-system | grep kube-storage-controller
```

- Tailing the logs

```bash
kubectl log kube-storage-controller-doryd-d4676c4db-jz6jz -n kube-system -f
```

# MEETUP DEMO

### 1. Inspect the Nimble storage kubernetes storage class 

```yaml
---
# Source: meetup/templates/mysql-db-storageclass.yaml
apiVersion: storage.k8s.io/v1beta1
kind: StorageClass
metadata:
  name: wordpress-meetup
  annotations:
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: hpe.com/nimble
parameters:
    thick: "false"
    dedupe: "false"
    perfPolicy: "SQL Server"
    destroyOnRm: "true"
    mountConflictDelay: "30"
    folder: "kubernetes"
    protectionTemplate: "Retain-30Daily"
```


### 2. Create the storage class that references Nimble Storage

```bash
kubectl apply -f part1/wordpress-storage-class.yaml
```
### 3. Inspect the Storage class 

```bash
kubectl get storageclass 
kubectl edit storageclass wordpress-meetup
```

### 3. Inspect the helm chart that is being used to deploy initial chart 

- [path to meetup-helm-chart](https://github.com/mugithi/meetup-may-15-18/tree/master/meetup-helm-chart)
- Review that there is **NO** storageClass defined in the helm chart - needs to be pre-created

### 4. Perform the helm chart dry run to see the output

```basb
helm install --name meetup-demo-01 --dry-run --debug meetup-helm-chart
```










