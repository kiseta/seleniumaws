# AWS, Jenkins, Selenium, Python, Git Integration

Open terminal connection to your AWS Linux2 instance, execute the following commands in command line inteface terminal (CMD/Git Bash)

## AWS EC2 Linux2 instance (AWS Academy Temporary Sandbox Lab Instance)
:bulb: see commands for a **AWS EC2 Ubuntu** instance at:

https://github.com/coddyca/seleniumaws

### Initial EC2 Installation on AWS

```
sudo yum update
```
```
sudo yum upgrade
```

### Install Google Chrome Binary for Linux
#### Note Google Chrome version.

```
curl https://intoli.com/install-google-chrome.sh | bash
```
```
google-chrome --version && which google-chrome
```

### Install chromedriver Binary for Linux
#### find the chromedriver version at https://chromedriver.storage.googleapis.com/index.html
#### replace the version in command below with your version
~~98.0.4758.102~~

```
cd /tmp/
```
```
wget https://chromedriver.storage.googleapis.com/98.0.4758.102/chromedriver_linux64.zip
```
```
unzip chromedriver_linux64.zip
```
```
sudo mv chromedriver /usr/bin/chromedriver
```
```
chromedriver --version
```

### Install Java 11 (will be used by Jenkins)

```
sudo amazon-linux-extras install java-openjdk11 -y
```
```
sudo yum update
```

### Install Jenkins

```
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```
```
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```
```
sudo yum upgrade
```
```
curl -LO 'https://rpmfind.net/linux/epel/7/x86_64/Packages/d/daemonize-1.7.7-1.el7.x86_64.rpm'
```
```
sudo rpm -Uvh ./daemonize-1.7.7-1.el7.x86_64.rpm
```
```
sudo yum install jenkins -y
```

### Enable Jenkins and start the service

```
sudo systemctl enable jenkins
```
```
sudo systemctl start jenkins
```

### Check Jenkins status (should be green and Active)

```
sudo systemctl status jenkins
```
```
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
```
```
virtualenv --version
```

### Install Git
```
sudo yum install git
```
```
git --version
```

---

### Make Jenkins & Git integration (using Terminal or CMD)

#### Generate ssh-keys (public and private)
```
ssh-keygen
```

#### Add Public key to the GitHub > Settings > SSH and GPG keys
To find the public key:
```
cat /home/ec2-user/.ssh/id_rsa.pub
```
#### Add Private key in Jenkins Project Config
To find the private key:
```
cat /home/ec2-user/.ssh/id_rsa
```
---

### Jenkins GitHub Repository Selenium Script Commands
* the sample code below is given for a Jankins project named: **PythonSeleniumMoodle**
* replace **moodle_tests.py** in **python -m unittest discover --pattern=moodle_tests.py** with your tests file name
---
#### Jenkins Project (), Git Branch name
:bulb: (**important** */master by default):
```
*/main
```
#### Jenkins on AWS EC2 Linux Instance >> GitHub Project :octocat:, Shell Script:
Setup a proper path, call virtualenv dir "venv", and virtualenv command installed in /usr/local/bin
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

#### Jenkins on Localhost >> GitHub Project :octocat:, Windows batch commands script:
```
python -m venv ./venv
cd venv/Scripts
call activate.bat
python.exe -m pip install --upgrade pip
pip install faker
pip install selenium
cd ../..
python -m unittest discover --pattern=moodle_tests.py
deactivate
```
#### The same as above with comments:

```
REM comment lines start with REM (Remark) in Windows Batch File
REM create venv in current directory
python -m venv ./venv
REM go to Scripts directory and activate venv
cd venv/Scripts
call activate.bat
REM install additional libraries
python.exe -m pip install --upgrade pip
pip install faker
pip install selenium
REM go back
cd ../..
REM run the script
python -m unittest discover --pattern=moodle_tests.py
REM deactivate venv
deactivate
```

#### Jenkins on Localhost >> Local Python Selenium Project ✔️, Windows batch commands script:
Navigate to the existing Python project (i.e. python_cctb) venv\Scripts directory, activate virtual environment, cd to project directory, run the scripts via moodle_tests.py file, deactivate virtual environment at the end
```
cd C:\Automation\Python\python_cctb\venv\Scripts
call activate.bat
cd C:\Automation\Python\python_cctb\practice\moodle
python -m unittest discover --pattern=moodle_tests.py
deactivate
```

### Build your project! :rocket: :crossed_fingers: :four_leaf_clover: :thumbsup:
