# AWS, Jenkins, Selenium, Python, Git Integration

Open terminal connection to your AWS Linux2 instance, execute the following commands in command line inteface terminal (CMD/Git Bash)

## AWS EC2 Linux2 instance (AWS Academy Temporary Lab Instance)
```
see commands for a AWS EC2 Ubuntu instance at:
https://github.com/coddyca/seleniumaws/blob/main/README.md
```
### Initial EC2 Installation on AWS

```
sudo yum update
sudo yum upgrade
```

### Install Google Chrome Binary for Linux

```
curl https://intoli.com/install-google-chrome.sh | bash
google-chrome --version && which google-chrome
```

### Install ChromeDriver Binary for Linux

```
cd /tmp/
wget https://chromedriver.storage.googleapis.com/98.0.4758.102/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
sudo mv chromedriver /usr/bin/chromedriver
chromedriver --version
```

### Install Java 11 (will be used by Jenkins)

```
sudo amazon-linux-extras install java-openjdk11 -y
sudo yum update
```

### Install Jenkins

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum upgrade

curl -LO 'https://rpmfind.net/linux/epel/7/x86_64/Packages/d/daemonize-1.7.7-1.el7.x86_64.rpm'

sudo rpm -Uvh ./daemonize-1.7.7-1.el7.x86_64.rpm

sudo yum install jenkins -y


```

### Enable Jenkins and start the service

```
sudo systemctl enable jenkins
sudo systemctl start jenkins
```

### Check Jenkins status (should be green and Active)

```
sudo systemctl status jenkins
cat /var/lib/jenkins/config.xml
```

### Jenkins initial admin password

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

### install Pyhton 3

```
sudo yum update
```
#### install Pyhton 3 prerequisites
```
sudo yum install gcc openssl-devel bzip2-devel libffi-devel
```
#### install Pyhton 3
```
sudo yum install python3
```

### Install Pip
```
sudo yum install python3-pip
```

### Install virtualenv using pip3
```
sudo pip3 install virtualenv
virtualenv --version
```

### Install Git
```
sudo yum install git
git --version
```

---

### Make Jenkins & Git integration (using Terminal or CMD)

#### Generate ssh-key
```
ssh-keygen
```

#### Add Public key to the GitHub > Settings > Deploy Keys, to generate the key:
```
cat /home/ec2-user/.ssh/id_rsa.pub
```
#### Add Privat key in Jenkins Project Config, to generate the key:
```
cat /home/ec2-user/.ssh/id_rsa
```
---

### Jenkins Moodle Selenium Script

#### Jenkins Project, Git Branch name (=>important<= */master by default):
```
*/main
```
#### Jenkins Project, Shell Script:
Setup a proper path, I call my virtualenv dir "venv", and I've got the virtualenv command installed in /usr/local/bin
```
PATH=${PATH}:/usr/local/bin
if [ ! -d "venv" ]; then
        virtualenv venv
fi
. venv/bin/activate
pip3 install faker
pip3 install selenium
pip3 install pytest
cd /var/lib/jenkins/workspace/PythonSeleniumMoodle/
python3 -m unittest discover --pattern=moodle_tests.py
deactivate
```