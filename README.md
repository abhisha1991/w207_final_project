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
4. (Optional - only if you are using your own setup) Create SSH Key for SSH log in into the VM
5. (Optional - only if you are using your own setup) Get your SSH Key ID. 
```
ibmcloud sl security sshkey-list
```
6. We will use the following SSH key ID assuming you are working with abhisha's account - 1822632
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

**Ensure the above steps have been done and we have a valid VM at minimum.**

If you're using a Windows machine, you'll need SSH to access the VM. Get SSH on Windows powershell following the steps [here](https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/)

Assuming VM already exists with right docker image as per the above steps

**SSH into the machine as per above steps to start the docker container from scratch**
1. SSH as per steps above
2. Check if any containers are running via "docker ps -a"
3. Optionally remove any container that may be running via "docker rm <container_name>". For example, docker rm w207-project
4. Start new container with "docker run -d --name w207-project -p 8888:8888 -v /root/w207_final_project:/project w207"
5. Confirm notebook is up and running with "docker ps -a"
6. Optionally, enter into the docker container via "sudo docker exec -it w207-project /bin/bash"

**OR If the container with the Jupyter notebook is already running**

1. Access the notebook auth token via the following command
```
docker logs w207-project
```
2. The above command gives an output like so. It also displays any errors / update messages from Jupyter notebooks
```
root@w207:~/w207_final_project# docker logs w207-project
[I 22:15:37.259 NotebookApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
[I 22:15:37.860 NotebookApp] Serving notebooks from local directory: /project
[I 22:15:37.860 NotebookApp] Jupyter Notebook 6.1.5 is running at:
[I 22:15:37.860 NotebookApp] http://02958160049d:8888/?token=c07bff81363fd69fac6563e38414da39a075495ecaa8cf7d
[I 22:15:37.860 NotebookApp]  or http://127.0.0.1:8888/?token=c07bff81363fd69fac6563e38414da39a075495ecaa8cf7d
[I 22:15:37.860 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 22:15:37.871 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://02958160049d:8888/?token=c07bff81363fd69fac6563e38414da39a075495ecaa8cf7d
     or http://127.0.0.1:8888/?token=c07bff81363fd69fac6563e38414da39a075495ecaa8cf7d
```
3. Extract the token from the above output (the token changes every time we start a new docker container)
4. Construct a URL like https://PUBLICIP:8888/?token=TOKENID - 
```
For the above example -  https://PUBLICIP:8888/?token=c07bff81363fd69fac6563e38414da39a075495ecaa8cf7d
```
5. The above endpoint points to the Jupyter session on the VM, we can edit/start/stop the notebook from the browser itself using any machine as long as that endpoint is valid (Jupyter server is running)
6. You can get the above VM's public IP address via "ifconfig"
