# CICD-PIPELINE-K8s
## CI CD Pipeline to Deploy to Kubernetes Cluster Using Jenkins
## Flow Diagram 

![CICD](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/54ae4081-c749-4194-859c-f3aecda6f25d)

## Install And Configure the Jenkins

#### (1) Launch the AWS EC2 Instance for jenkins server.
#### (2) Connect to instance using mobaxterm with the help of public key.

## Download and Install Jenkins

#### (1) Update the app packages before installing java and jenkins
            sudo yum update -y
#### (2) Install java
            sudo yum install java-17
                        (or)
            amazon-linux-extras install java-openjdk17 -y
#### (3) Install jenkins
##### (i) Add the Jenkins repo using the following command:
            sudo wget -O /etc/yum.repos.d/jenkins.repo \ 
            https://pkg.jenkins.io/redhat-stable/jenkins.repo
##### (ii) Import a key file from Jenkins-CI to enable installation from the package:
            sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
##### (iii) update the app packages
            sudo yum update -y
##### (iv) Install Jenkins:
            sudo yum install jenkins -y
##### (v) Enable the Jenkins service to start at boot:
            sudo systemctl enable jenkins
##### (vi) Start Jenkins as a service:
            sudo systemctl start jenkins
##### (vii) You can check the status of the Jenkins service using the command:
            sudo systemctl status jenkins
#### (4) Configuring Jenkins
##### (i) Connect to http://<your_server_public_DNS>:8080 from your browser. You will be able to access Jenkins through its management interface:
![unlock_jenkins](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/907209f9-d4f8-4a97-8a9b-fba721f8960a)
##### (ii) As prompted, enter the password found in /var/lib/jenkins/secrets/initialAdminPassword.
###### (a) Use the following command to display this password:
            sudo cat /var/lib/jenkins/secrets/initialAdminPassword
##### (iii) The Jenkins installation script directs you to the Customize Jenkins page. Click Install suggested plugins.
##### (iv) Once the installation is complete, the Create First Admin User will open. Enter your information, and then select Save and Continue.
![create_admin_user](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/0dee6940-d9cd-43a3-8d84-679a39d2f221)


### Download and Install Maven
#### (1) Download the maven tar file using following link
             wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
#### (2) Untar the tar file using following cmd:
            tar -xvf [tarfile]
#### (3) Move the maven file to /opt directory 
            mv [source file path] [destination file path]
            cd /opt
            mv [maven file] maven
#### (4) To Check the Maven version goto bin folder under maven folder [ex: /opt/apache-maven-3.9.6/bin] and enter following cmd:
            ./mvn -v

            Apache Maven 3.9.6 (bc0240f3c744dd6b6ec2920b3cd08dcc295161ae)
            Maven home: /opt/maven
            Java version: 17.0.10, vendor: Amazon.com Inc., runtime: /usr/lib/jvm/java-17-amazon-corretto.x86_64
            Default locale: en_US, platform encoding: UTF-8
            OS name: "linux", version: "5.10.205-195.804.amzn2.x86_64", arch: "amd64", family: "unix"
#### (5) Add maven path to Environment variable
##### (i) goto root directory using CD cmd
##### (ii) type following cmd for knowing hidden files
            ll -a
##### (iii) open the .bash_profile file 
            vim .bash_profile
##### (iv) add the following code
            M2_PATH=/opt/maven
            M2=/opt/maven/bin
            JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto.x86_64
            PATH=$PATH:$HOME/bin:$JAVA_HOME:$M2_PATH:$M2
##### (v) After Save the Above File update the changes
            source .bash_profile
            echo $PATH
            -- /sbin:/bin:/usr/sbin:/usr/bin:/root/.local/bin:/root/bin:/root/bin:/usr/lib/jvm/java-17-amazon-corretto.x86_64:/opt/maven:/opt/maven/bin
### Install maven plugins and configure jenkiks with maven
#### (1) install the maven integration plugin from the available plugins under plugins
#### (2) Configure the java and maven with the jenkins
##### (i) Goto to Tools and Click the Add Jdk under JDK section 
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/147a26de-445e-43df-9d68-1864410cf04a)
##### (ii) Goto Maven installation Section and Click Add maven then Apply and Save
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/f2c07ac8-9fac-47ba-8cd3-7d680970a240)
#### (3) Configure git as shown in fig
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/9f3dabe9-8abc-4b81-93fd-e01395397fed)
#### (4) Under Build section make sure the Root POM is pom.xml and give goals like "Clean Install" 
#### (5) Build Job

## Ansible Server Installation and Setup

#### (1) Launch the Ec2 Instance
#### (2) connect the Server using mobaxterm
#### (3) change the hostname
            sudo su
            hostnamectl set-hostname ansible-server
            /bin/bash
#### (4) Create a new user and add the user to sudo user-list
            useradd [username]
            passwd  [username]
            new password:
            retype password:
            # To Add user to Sudo user-list
            type: visudo

            # find and add the Below shown lines
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/e182b918-44e8-45c5-b5b2-d49811027e02)
#### (5) Enable the passwordAuthentication 
##### (i) open the /etc/ssh directory
            cd /etc/ssh
##### (ii) open sshd_config file and Find the Below lines and change to "Yes"
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/1fcac5e4-678a-4c77-888b-8ed2e628c169)

##### (iii) If needed Reload the sshd Service
            systemctl reload sshd.service
#### (6) login to new user that we created before
            sudo su - ansible
#### (7) install Ansible
            sudo amazon-linux-extras install epel
            sudo yum install ansible -y
            sudo ansible --version
#### (8) integrate ansible with jenkins
##### (i) Goto jenkins Dashboard
##### (ii) goto manage jenikns >> plugins >> available plugins >> Download ansible plugin,Publish over SSH
##### (iii) goto to manage jenkins >> system >> Publish over SSH >> add SSH Server
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/c7dba0f2-a637-4005-9338-c3e2748c7938) | ![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/d3074123-23fd-4c3a-b96b-76733b2f7525)


#### (9) create a folder for dockerfile
##### (i) create a folder
            sudo mkdir Docker
##### (ii) Change ownership of the file
            sudo chown -R ansible:ansible Docker
#### (10) Goto Jenkins Console
##### (i) Goto manage jenkins >> System >> Post Build Actions
##### (ii) Select the Send Build Artifact over SSH
##### (iii) Enter the Details As Shown Below fig
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/2065ee60-a674-4c4f-b5e4-15e9dc479855)
##### Appaly And Save ,Build the job to Verify

### Install and Configure Docker on Ansible server

#### (1)  To install the docker
            sudo yum install docker -y
#### (2) Add the user to docker group
            sudo usermod -aG docker ansible
#### (3) Start the Docker Service
            sudo service docker start
#### (4) Create a Docker file for pull the tomcat image from docker hub and copy the jar/war file to tomcat/webapps
            vim Dockerfile

            FROM tomcat:latest
            RUN cp -R /usr/local/tomcat/webapps.dist/* /usr/local/tomcat/webapps
            COPY ./*.jar /usr/local/tomcat/webapps

### Create A Ansible PlayBook to Create Docker image and copy image to docker hub
#### (1) open the hosts file under /etc/ansible/ and add the group name and private ip address of target servers as shown below fig then save
            sudo vim /etc/ansible/hosts
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/9d980577-1d71-43bd-8057-a655bae07753)
#### (2) copy the keys to the target server using private ip as shown below fig
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/7f7ee63a-4462-46d3-8c6a-cb4d00f54eb0)
#### (3) To Create the Ansible play book use below commands and add the code as shown below fig and save
            vim regapp.yml
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/fa2fa9fc-2823-4f96-af7b-ea2842bd5cd7)
#### (4) Ansible’s check mode allows you to execute a playbook without applying any alterations to your systems. You can use check mode to test playbooks before implementing them in a production environment.
####     To run a playbook in check mode, you can pass the -C or --check flag to the ansible-playbook command:
            ansible-playbook --check regapp.yaml
#### if any errors happen please do change as per error massage
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/f53b935f-66d9-4820-a968-f825d3412123)
#### (5) Run the playbook and check the images in Docker
            ansible-playbook regapp.yaml
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/001eb1ab-ce82-46a0-a068-6c31e00f3b1b)
            docker images
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/8cfa6be4-9176-40a5-b72c-ad1556567835)

#### To push the image to Docker Hub
##### (1) First login to the Docker hub as shown below
            docker login
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/867e646a-6cb6-4a28-a70a-1982b0a10519)
##### (2) Modify the ansible playbook and add below code to tag and push the image to Docker hub and save the file.
            vim regapp.yaml
            
            ---
            - hosts: ansibleserver

              tasks:
              - name: create a docker image
                command: docker build -t regapp:latest .
                args:
                  chdir: /opt/Docker
                  
              - name: Create a tag to push the image on to Docker hun
                command: docker tag regapp:latest murali2712/regapp:latest
            
              - name: To Push docker image to docker hub
                command: docker push murali2712/regapp:latest

##### (3) Check the Playbook and Run it
            ansible-playbook regapp.yaml --check

            ansible-playbook regapp.yaml
### Update Jenkins Job to use the ansible playbook
#### Add the below Command to post-build actions
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/76429ce3-987e-4467-bf0c-a8e394781466)

### Set Up Bootstrap for eksctl

#### (1) Launch the New Server
#### (2) Connect to server through mobaxterm
#### (3) install the kubectl
##### (i) Visit the Amazon EKS website and copy,paste the below command
            curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.28.5/2024-01-04/bin/linux/amd64/kubectl
##### (ii) Goto Root Dir and Check the Installed file i.e; kubectl
##### (iii) Apply execute permissions to the binary, and Check using "ll" cmd
            chmod +x ./kubectl
##### (iv) move the kubectl file to bin because all executable files should be in bin folder.
            mv kubectl /bin
##### (v) if You want to know the version of kubectl 
            kubectl version --output=yaml
#### (4) Install the eksctl
##### (i) Install the eksctl using below command and move the file to bin folder
            curl -sL https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz | tar -xz -C /tmp && sudo mv /tmp/eksctl /bin
##### (ii) Create a Role for the eks
###### (a) First Goto IAM in console
###### (b) Click Create Role
###### (c) Select Aws Service
###### (d) Select EC2 and click next
###### (e) Give the Permissions listed below
            AdministratorAccess (in prod env we shoud avoid this access,here i give for testing perpose)
            AmazonEC2FullAccess
            AWSCloudFormationFullAccess
            IAMFullAccess

            Give the Role name and Create it
###### (f) modify the iam role for the server
            Goto to instance >> Actions >> Security >> modify iam role >> Select the Role that we create now >> update the role

### Setup the Kubernetes using eksctl
#### (1) create a cluster as shown below
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/cf017f65-f402-4007-8a06-74283804f6cb)

            eksctl create cluster --name murali-cluster \
            > --region ap-south-1 \
            > --node-type t2.small
#### (2) verify the cluster
            kubectl get nodes
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/7f76cae4-c6d2-4865-8424-ab89ccfd521e)
#### (3) Refer the Kubernetes Documentation and create the Deployment manifest file
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/1cb4ce28-797e-4609-bf7f-34bf333cb1ba)
#### (4) Create a Service manifest file
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/1feae5ba-a016-4c8c-a5d9-a6c16dfc8559)

### Integrate Bootstrap server with ansible

#### (1) Enable the Password Authentication 
            vim /etc/ssh/sshd_config

            change PasswordAuthentication yes
#### (2) update the password for root users
            passwd root
            new password:
            retype-password:
#### (3) Goto the ansible server and add the one more host group under hosts file as shown below
            [note: please add private ip of the server]
            vim /etc/ansible/hosts
![image](https://github.com/mmurali12/CICD-PIPELINE-K8s/assets/102593989/f95357ce-af99-4851-874f-0bd729dfcc0d)
####

            
















