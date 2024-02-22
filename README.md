# DevOps-Project-with-Jenkins-Maven-SonaQube-Docker-and-EKS
This is a simple ci cd project that deploys to kubernetes using jenkins

## Here is the project work flow of how I will build the app

![Screenshot 2024-02-22 135751](https://github.com/mariamo23/Register-app/assets/124802455/3cb5fa71-b5d2-40b1-89d1-c77d8868c139)

# Steps to Follow

## Step 1: Launch an Ubuntu 22.04 t2 micro instance and name is Jenkins-Master

![Screenshot 2024-02-22 150421](https://github.com/mariamo23/Register-app/assets/124802455/4e493911-f2b5-4214-ba5d-5f6e9b56f523)


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

## Step 4 Launch an Ubuntu 22.04 t2 micro instance and name is Jenkins-Agent

![Screenshot 2024-02-22 154531](https://github.com/mariamo23/Register-app/assets/124802455/4d3a0100-3c27-4ab6-b108-078f5f3f9769)

## Step 5: Connect to your console and install Java

```
sudo apt update
sudo vi /etc/hostname  # change hostname
sudo init 6 # reboot the system
```
### Install Java

```
sudo apt install fontconfig openjdk-17-jre -y # install Java 17
java -version  # check Java version
```

## Step 6: Install Docker on Jenkins-Agent

```
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER
sudo init 6
```

## Step 7: Edit ssh config file on both Jenkins Master and Agent

```
sudo vi /etc/ssh/sshd_config
sudo service sshd reload  # reload the sshd service
```
Uncomment and make `PubkeyAuthentication` yes and uncomment `AuthorizedKeysFile`

## Step 8: Genenrate SSH key on the master server

```
ssh-keygen
```

```
cat .ssh/id_rsa.pub
```
copy the key and go to the agent server and run the following:

```
vi .ssh/authorized_keys
```
Paste the copied key here and save the file

## Step 9:

Copy the public IP of Jenkins-Master
