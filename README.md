## Installed Jenkins on EC2 Instance
Installed Jenkins, configured Docker as agent, set up cicd, deployed applications to k8s and much more.

## AWS EC2 Instance

- Went to AWS Console
- Instances(running)
- Launched instances

### Installed Jenkins.

Pre-Requisites:
 - Java (JDK)

### Ran the below commands to install Java and Jenkins

Installed Java

```
sudo apt update
sudo apt install openjdk-11-jre
```

Verified Java is Installed

```
java -version
```

Proceeded with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins was not be accessible to the external world due to the inbound traffic restriction by AWS. Opened port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Clicked on <Instance-ID>
- In the bottom tabs -> Clicked on Security
- Security groups
- Added inbound traffic rules as shown in the image (can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

### Logged in to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080    [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]
 
After logging in to Jenkins, 
      - Ran the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`
      - Entered the Administrator password
      
### Clicked on Install suggested plugins

Waited for the Jenkins to Install suggested plugins
Jenkins Installation is Successful. You can now starting using the Jenkins 

## Installed the Docker Pipeline plugin in Jenkins:

   - Logged in to Jenkins.
   - Went to Manage Jenkins > Manage Plugins.
   - In the Available tab, searched for "Docker Pipeline".
   - Selected the plugin and clicked the Install button.
   - Restarted Jenkins after the plugin was installed.
   
Waited for the Jenkins to be restarted.

## Docker Slave Configuration

Ran the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```

### Granted Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su - 
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

Restarted Jenkins

```
http://<ec2-instance-public-ip>:8080/restart
```

The docker agent configuration now successful.




