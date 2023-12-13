# Some useful and frequently used gcloud TPU commands

## Blogs: 
1. Preemptible TPUs: https://cloud.google.com/tpu/docs/preemptible

## Setup a TPU instance
```bash
zone=us-central1-f                                                                                                                                                                    
accelerator_type=v2-8
name=tpu-$zone-$accelerator_type
    
gcloud compute tpus tpu-vm create $name \
    --zone=$zone \
    --accelerator-type=$accelerator_type \
    --version=tpu-ubuntu2204-base #vm-tf-2.15.0-pjrt

# use --preemptible flag to create interruptable TPUs instance (similar to any spot instances)
```

## Connect to Cloud TPU VM
```bash
gcloud compute tpus tpu-vm ssh $TPU_NAME --zone=$ZONE

# For the first time, it generates the ssh keys (public-private), usually ~/.ssh/google_compute_engine*
```

## Remote ssh to compute engine on Google Cloud
```
# 1. Get the external IP of the VM
gcloud compute tpus tpu-vm describe $TPU_NAME --zone=$ZONE
# look for `externalIP`

# 2. create config file
Host XX.XX.XX.XXX (EXTERNAL IP)
HostName XX.XX.XX.XXX (EXTERNAL IP)
   UseKeychain yes
   AddKeysToAgent yes
   IdentityFile ~/.ssh/google_compute_engine
   User your_username

# 3. cmd+shift+P -> Remote-SSH: Connect to Host… and ssh into the XX.XX.XX.XXX
```

## 
