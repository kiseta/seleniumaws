# AWS, Jenkins, Selenium, Python, Git Integration
### Initial EC2 Installation on AWS

sudo yum update
sudo yum upgrade

### Install Google Chrome Binary for Linux

curl https://intoli.com/install-google-chrome.sh | bash
google-chrome --version && which google-chrome

### Install ChromeDriver Binary for Linux

cd /tmp/
wget https://chromedriver.storage.googleapis.com/2.37/chromedriver_linux64.zip
unzip chromedriver_linux64.zip
sudo mv chromedriver /usr/bin/chromedriver
chromedriver --version


### Install Java 11 (will be used by Jenkins)

sudo amazon-linux-extras install java-openjdk11 -y
sudo yum update –y

### Install Jenkins
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
sudo yum upgrade
curl -LO 'https://rpmfind.net/linux/epel/7/x86_64/Packages/d/daemonize-1.7.7-1.el7.x86_64.rpm'
sudo rpm -Uvh ./daemonize-1.7.7-1.el7.x86_64.rpm
sudo yum install jenkins -y
cat /var/lib/jenkins/config.xml

### Enable Jenkins and start the service
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins


### Check Jenkins status (should be green and Active)
```
sudo systemctl status jenkins
```

### Jenkins initial admin password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword