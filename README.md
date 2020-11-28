# Facial Keypoints Detection

## Contributors

1. Sirisha Bhupathi
2. Abhi Sharma


## Helpful Links

Project: https://www.kaggle.com/c/facial-keypoints-detection 

## Note on Compute Requirements

We will use IBM cloud to provision compute that will host a jupyter notebook that will run our model. We are doing this because adequate compute is needed to be able to run the deep learning models for the above Kaggle competition (which is not available by default on the Kaggle notebooks). Upon running heavy models natively in Kaggle, we get the following prompt - "Your notebook tried to allocate more memory than is available. It has restarted"

## Initial Setup (One Time)

1. Contact abhisha@berkeley.edu for permissions to IBM account (or follow the steps below with YOUR IBM cloud account)
2. Get the IBM Cloud CLI tools
```
curl -fsSL https://clis.ng.bluemix.net/install/linux | sh
```
3. Log in with your IBM account credentials
```
ibmcloud login
```
4. Create SSH Key for SSH log in into the VM
5. Get your SSH Key ID. 
```
ibmcloud sl security sshkey-list
```
6. We will use the following SSH key ID assuming you are working with abhisha@ischool.berkeley.edu - 1822632
7. Create the VM in IBM cloud and wait till it is fully provisioned
```
ibmcloud sl vs create --datacenter=dal13 --hostname=w207 --domain=ucb.com  --cpu=4 --memory=32768 --disk=100 --os=UBUNTU_18_64 --billing=hourly --network=1000 --key=1822632 --san
```
8. Get the public IP address of the VM provisioned (either via CLI or via the [portal](https://cloud.ibm.com/))
```
ibmcloud sl vs list
```
9. SSH into the VM (with the sshkey file). The below assumes you are using the key associated with the above key ID (1822632)
```
ssh -i sshkey root@<PUBLIC IP ADDRESS>
```
10. Once you are in the VM, we need to install a few things
```
sudo apt-get update
sudo apt install docker
snap install docker
sudo apt install git
git clone https://github.com/abhisha1991/w207_final_project
cd w207_final_project
cd docker
docker build -t w207 -f Dockerfile .
```

11. Run the following as a sanity check to confirm everything has installed correctly

```
docker images # Should show the image called w207
docker ps -a # Should not have any active docker containers running
```

**Once you are done with the VM and don't plan to use it for a while, please cancel the VM - else it will be billed indefinitely**

## Quick Start

Ensure the above steps have been done and we have a valid VM at minimum.

If you're using a Windows machine, you'll need SSH to access the VM. Get SSH on Windows powershell following the steps [here](https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/)

(Assuming VM already exists with right docker image as per the above steps)

1. SSH into the machine as per above steps to start the docker container from scratch
  1.1. SSH as per steps above
  1.2. Check if any containers are running via "docker ps -a"
  1.3. Optionally remove any container that may be running via "docker rm <container_name>"
  1.4. Start new container with "docker run -d --name w207-project -p 8888:8888 -v /root/w207_final_project:/project w207"

2. OR if the container with the Jupyter notebook is already running, access the notebook via the public IP endpoint of the VM
