## Create an AWS Account for Free

This video provides a detailed and concise tutorial on how to create a free AWS account quickly. It includes step-by-step instructions, covering essential aspects such as account setup, verification, and initial configuration. 

[Watch the video on YouTube](https://www.youtube.com/watch?v=JFwAS_8BZvM&ab_channel=CSCORNERSunitaRai)

### Steps Covered in the Video:
1. **Account Registration**: How to sign up for AWS and input the required details.
2. **Verification Process**: Completing phone and email verification.
3. **Billing Information**: Adding payment details while ensuring you're on the free tier.
4. **Initial Setup**: Navigating the AWS Management Console and understanding the dashboard.



## Step 1: Spin up EC2 Instance ##
* Go to AWS Console to spin up an EC2 instance of Ubuntu. Select t3.tiny (free tier), but increase the storage volume to 30gb (free tier also)
* Download the secret access keys (.pem file)
* Take note of the public IP


# Environment Set up #
Keep in mind that computers have different operative systems and commands. The guide below might not apply to your machine. Follow the steps and use google, documentation and chatgpt to achieve the goals mentionned below if the commands provided do not work.


## Step 2: Connect to Ubuntu EC2 server ##
* This is done with the powershell terminal in windows, or the terminal on a mac. The goal here is to connect on our machine to the EC2 instance that is in the cloud. Our computer will remotely access the cloud machine and we will be able to interact with it through an ssh connection.

* Your goal here is to connect to the EC2 instance remotely with the ssh log in information that you will find in the aws section related the the newly created EC2 instance.

* Execute the following command from .ssh directory to change the permission of .pem file to protect it
  ``` sh
  chmod 400 downloaded_pem_file
  ```
* Execute the following command to connect to the server
  ``` sh
  ssh -i pem_file ubuntu@public_ip
  ```
* For ease of connecting the server add the connection details in config file in .ssh directory.
  ``` sh
  nano ~/.ssh/config
  ```
* If config file is missing you can create a new one
  ``` sh
  touch config
  ```
  Add the following and save it.
  ```
  Host short-name-of-your-choice
       Hostname public-ip-of-ec2
       User ubuntu
       IdentityFile path-to-.pem-file
       StrictHostKeyChecking no
  ```
  Now run ```ssh short-name-of-your-choice``` to quickly connect to the ubuntu server. 

## Step 3: Configure Ubuntu server ##
### Install Anaconda ###

* Now that you have remote access to the EC2 instance, you will setup a few things inside of it.

* Once connected to your EC2 instance via powershell or other, visit the Anaconda website to get the download link for Linux (x86) version and execute the following to download the file in the server.
  ``` sh
  wget link_copied_from_anaconda_website
  ```
  for example: 
  
  ```
  wget https://repo.anaconda.com/archive/Anaconda3-2024.02-1-Linux-x86_64.sh
  ```
* Run the following command to install the downloaded file
  ``` sh
  bash downloaded_installation_file
  ```

  for example:
  ```
  bash Anaconda3-2024.02-1-Linux-x86_64.sh
  ```
  If required, logout and re-login to the server.


### Install Docker ###
* Next we install Docker inside the EC2 instance. However if you run into "Package ‘docker.io’ has no installation candidates" error just update the system first and then try installing again.
  ``` sh
  sudo apt update
  sudo apt install docker.io
  ```
* To run without sudo, add your user to the docker user group.
  ``` sh
  sudo groupadd docker
  sudo usermod -aG docker $USER
  ```
### Install docker-compose ###
* Create a separate directory to install docker compose.
  ``` sh
  mkdir soft
  cd soft
  ```
* To install Docker Compose get the latest release version for your OS (https://github.com/docker/compose -> Releases -> Assets) and make it executable.
  ``` sh
  wget link-from-docker-compose-github -o docker-compose
  chmod -x docker-compose
  ```
* To ensure docker-compose is called from soft directory from wherever we call, we need to edit .bashrc file.
  ``` sh
  nano ~/.bashrc
  export PATH="${HOME}/soft:${PATH}"
  source ~/.bashrc
  ```
  If required, logout and re-login to the server.

### Run Docker ###
* To verify that docker setup the following should run.
``` sh
  docker run hello-world
```

If you see a message saying Hello from Docker! you have completed the steps successfully!


###  BONUS : VS Code Setup

* VSCode allows you to connect to your EC2 cloud machine with an ssh connection.

* This is great because it allows you to see the cloud machine files and interact with it through your terminal as if you were coding in a local repository.

* Your goal here is to setup an ssh connection to your cloud machine inside vscode, successfully connect to it and use the remote file explorer to see what is inside the cloud machine.

* Some instructions are here to guide you but you might need to read documentation or ask chatgpt for help.

* Install "Remote - SSH" extension in VS Code
* Click on "Open a Remote Window" icon on bottom-left corner
* From dropdown select "Connect to Host" and then select Linux. That opens a new VSCode window.
* In terminal clone MLOps-Zoomcamp project
  ``` sh
  git clone https://github.com/DSML-bootcamp/w8-nbs.git
  ```
* Create a notebooks folder
  ``` sh
  mkdir notebooks
  cd notebooks
  ```
* The notebooks are hosted on the server. However to access them locally you might need to do the port forwarding that can be easily done in VSCode. Open Ports section next to terminal in VSCode and enter 8888 as port for source and enter. This will add the port to allow traffic.
* Next open jupyter notebook
  ``` sh
  jupyter notebook
  ```


# Important
## Do not forget to stop the EC2 instance if not in use.
