# DevOps-Project-with-Jenkins-Maven-SonaQube-Docker-and-EKS
This is a simple CICD project that deploys to kubernetes using jenkins

## Here is the project work flow of how I will build the app

![Screenshot 2024-02-22 135751](https://github.com/mariamo23/Register-app/assets/124802455/3cb5fa71-b5d2-40b1-89d1-c77d8868c139)

# Steps to Follow

## Step 1: Launch an Ubuntu 22.04 t2 large instance 

![Screenshot 2024-02-26 150639](https://github.com/mariamo23/Register-app/assets/124802455/d9201b73-377b-4b60-a4c9-c71c224d25f0)


## Step 2: Go to your Security Group and open Inbound Port 8080, since Jenkins works on Port 8080

![Screenshot 2024-02-22 153607](https://github.com/mariamo23/Register-app/assets/124802455/518d1338-fa04-4cd0-b78a-5ed5c29473c4)


## Step 3: Connect to your console and create a shell script to install Java and Jenkins

```
sudo vi /etc/hostname  # change hostname
sudo init 6 # reboot the system
```

Refer--https://www.jenkins.io/doc/book/installing/linux/

### Install Java and Jenkins

```
vi Jenkins.sh
```

```
#!/bin/bash
sudo apt update 
sudo apt install fontconfig openjdk-17-jre -y # install Java 17
java -version  # check Java version
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update 
sudo apt-get install jenkins -y
sudo systemctl enable jenkins  # enable the Jenkins service to start at boot
sudo systemctl start jenkins # start the Jenkins service
sudo systemctl status jenkins # check the status of the Jenkins service
```

```
sudo chmod 777 Jenkins.sh # this will give Jenkins.sh executable permission
./Jenkins.sh    # this will installl java and jenkins
```

## Step 4:

Copy the public IP address and open on the browser using port 8080.

![Screenshot 2024-02-26 151357](https://github.com/mariamo23/Register-app/assets/124802455/572231bf-053e-48d5-be43-f685f60132ce)

Grab the password using below command

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![Screenshot 2024-02-26 151440](https://github.com/mariamo23/Register-app/assets/124802455/8c8c2459-15eb-4f11-aa44-4eaa61c11f55)

Install suggested plugins

![Screenshot 2024-02-26 151601](https://github.com/mariamo23/Register-app/assets/124802455/60e0f24e-997f-4fe2-abdf-a4b6ba9d7ade)

Create a user and then save and continue

## Step 5: Install Docker on the server

```
vi Docker.sh
```

```
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER   #my case is ubuntu
newgrp docker
sudo chmod 777 /var/run/docker.sock
```
```
sudo chmod 777 Docker.sh # this will give Docker.sh executable permission
./Docker.sh    
```

## Step 6: Install plugins like Maven, JDK

Go to Manage Jenkins, plugins, available plugins. Select Maven integration, Pipleline maven integration and Eclipse Temurin Installer Plugin to install JDK.

![Screenshot 2024-02-26 162615](https://github.com/mariamo23/Register-app/assets/124802455/7c153e41-fb45-4c57-995e-6c654ebeb14f)
![Screenshot 2024-02-26 162926](https://github.com/mariamo23/Register-app/assets/124802455/98739ed1-5692-4e22-af1c-f421140fdf5a)

Go to Manage Jenkins, Tools, maven installation and name it and select install authomatically. Also, go to JDK installation and give it a name and select install authomatically. Click add installer and select install from adoptium.net. Select JDK-17.05+8. Apply and Save

![Screenshot 2024-02-26 163643](https://github.com/mariamo23/Register-app/assets/124802455/9f3bfd63-e756-4bf5-bfd8-bf502af5a7e6)

![image](https://github.com/mariamo23/Register-app/assets/124802455/d9d16313-b898-478e-be5c-c18568937870)



## Step 7: Github integration with Jenkins

Add github credentials to Jenkins. Go to Manage Jenkins --> Credentials --> under stores scoped to jenkins, select global and add credentials.

![image](https://github.com/mariamo23/Register-app/assets/124802455/3a2be149-ff0a-4198-8b30-01ac851eca7f)

![image](https://github.com/mariamo23/Register-app/assets/124802455/808adcd7-c262-4302-9225-ae07a67e9844)


Select Username with password

Username: Github username

Password: Token generated from github ( Settings --> Develpers settings --> Personal access token --> Token(classic) --> Generate new token )

ID and description: github

Then click create





