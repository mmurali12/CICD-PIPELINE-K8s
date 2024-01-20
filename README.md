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