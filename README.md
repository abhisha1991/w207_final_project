# Facial Keypoints Detection

## Contributors

1. Sirisha Bhupathi
2. Abhi Sharma


## Helpful Links

Project: https://www.kaggle.com/c/facial-keypoints-detection 

## Note on Compute Requirements

We will use IBM cloud to provision compute that will host a jupyter notebook that will run our model. We are doing this because adequate compute is needed to be able to run the deep learning models for the above Kaggle competition (which is not available by default on the Kaggle notebooks). Upon running heavy models natively in Kaggle, we get the following prompt - "Your notebook tried to allocate more memory than is available. It has restarted"

## Initial Setup (One time)

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


**Once you are done with the VM and don't plan to use it for a while, please cancel the VM - else it will be billed indefinitely**

## Quick Start

