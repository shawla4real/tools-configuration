# tools-configuration
git, ansible, jenkins, cli, apache tomcate
Launch an ec2 instance, name it Devops

1. git registration and installation
a. www.github.com  
b. git installation   

yum install git wget -y
git --version


2. ansible installation

cd /tmp
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install epel-release-latest-7.noarch.rpm -y
yum install ansible -y
ansible --version
ansible -m ping localhost

3. Amazon cli installation
install as ec2-user $$

curl -O https://bootstrap.pypa.io/get-pip.py
python get-pip.py --user
export PATH=~/.local/bin:$PATH
export PATH=/home/ec2-user:$PATH
source ~/.bash_profile
pip --version
pip install awscli --upgrade --user
aws --version

4. Apache Tomcat 8 installation.
(A)
sudo su
yum install java -y
cd /opt
wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.0.32/bin/apache-tomcat-8.0.32.tar.gz
tar xzvf apache-tomcat-8.0.32.tar.gz
rm -rf apache-tomcat-8.0.32.tar.gz
mv apache-tomcat-8.0.32 tomcat8
cd tomcat8/bin
./startup.sh    
./shutdown.sh

(B)  i. setting tomcat user 
cd /opt/tomcat8/conf
cp tomcat-users.xml ~

vi tomcat-users.xml

  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <role rolename="manager-script"/>
  <user username="nduka" password="nduka" roles="manager-gui,admin-gui,manager-script"/>

Then start tomcat with the code below
/opt/tomcat8/bin/startup.sh


stop tomcat before proceeding to avoid error. 

ii. configure tomcat as a service

cd /etc/systemd/system
vi tomcat.service

  
[Unit]
Description=Setup Systemd script for tomcat in Tomcat Servlet Engine
After=network.target

[Service]
Type=forking
GuessMainPID=yes
Restart=always
RestartSec=5
ExecStart=/opt/tomcat8/bin/startup.sh start
ExecStop=/opt/tomcat8/bin/shutdown.sh stop

[Install]
WantedBy=multi-user.target
Alias=tomcat.service

chmod 755 tomcat.service
systemctl stop tomcat
systemctl start tomcat


Jenkins installation

cd /opt/tomcat8/webapps

wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war

your ip:8080/jenkins

jekins user name and password , sola... sola
