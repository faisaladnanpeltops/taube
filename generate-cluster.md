# Networking
## Virtual Private Cloud Network
```
gcloud compute networks create taube --subnet-mode custom

gcloud compute networks subnets create kubernetes \
  --network taube \
  --range 10.240.0.0/24
```
## Firewall Rules
```
gcloud compute firewall-rules create taube-allow-internal \
  --allow tcp,udp,icmp \
  --network taube \
  --source-ranges 10.240.0.0/24,10.200.0.0/16
```

```
gcloud compute firewall-rules create taube-allow-external \
  --allow tcp:22,tcp:6443,icmp \
  --network taube \
  --source-ranges 0.0.0.0/0
```
## Kubernetes Public IP Address
```
gcloud compute addresses create taube \
  --region $(gcloud config get-value compute/region)
gcloud compute addresses list --filter="name=('taube')"
```

# Instances
## Controller
```
for i in 0 1 2; do
  gcloud compute instances create controller-${i} \
    --async \
    --boot-disk-size 200GB \
    --can-ip-forward \
    --image-family ubuntu-2004-lts \
    --image-project ubuntu-os-cloud \
    --machine-type e2-standard-2 \
    --private-network-ip 10.240.0.1${i} \
    --scopes compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
    --subnet kubernetes \
    --tags taube,controller
done
```
## Workers
```
for i in 0 1 2; do
  gcloud compute instances create worker-${i} \
    --async \
    --boot-disk-size 200GB \
    --can-ip-forward \
    --image-family ubuntu-2004-lts \
    --image-project ubuntu-os-cloud \
    --machine-type e2-standard-2 \
    --metadata pod-cidr=10.200.${i}.0/24 \
    --private-network-ip 10.240.0.2${i} \
    --scopes compute-rw,storage-ro,service-management,service-control,logging-write,monitoring \
    --subnet kubernetes \
    --tags taube,worker
done
```
## Verify
```
gcloud compute instances list --filter="tags.items=taube"
```
