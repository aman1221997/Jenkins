# Jenkins

## Prerequisites

An AWS account. If you don’t have one, then register at https://portal.aws.amazon.com/billing/signup#/start/email

## Launch an AWS EC2 instance

Launch an LAmazon Linux EC2 instance and enable port 8080 in Firewall(Security groups)

  To setting up Security groups:
  
  i) Select Add Rule, and then select SSH from the Type list.
  
  ii) Select Add Rule, and then select HTTP from the Type list.
  
  iii) Select Add Rule, and then select Custom TCP Rule from the Type list.
  
  iv) Under Port Range, enter 8080.

## Connect EC2 instance using terminal

Open Terminal and run command

```
PS C:\Users\aman1> ssh -i .\Downloads\jenkins-keypair.pem ec2-user@ec2-43-204-214-249.ap-south-1.compute.amazonaws.com
[ec2-user@ip-172-31-5-63 ~]$ sudo su -
[root@ip-172-31-5-63 ~]# yum update -y
Last metadata expiration check: 0:10:37 ago on Sat Nov 25 00:46:51 2023.
Dependencies resolved.
Nothing to do.
Complete!
[root@ip-172-31-5-63 ~]#
```

## Download and install Jenkins

Complete the above steps and then download and install Jenkins

```
[root@ip-172-31-5-63 ~]# yum install java-11-amazon-corretto-headless -y
Complete!
[root@ip-172-31-5-63 ~]# wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat-stable/jenkins.repo
[root@ip-172-31-5-63 ~]# rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
[root@ip-172-31-5-63 ~]# yum upgrade
[root@ip-172-31-5-63 ~]# yum install jenkins -y
Installed:
  jenkins-2.426.1-1.1.noarch

Complete!
[root@ip-172-31-5-63 ~]# systemctl enable jenkins
Created symlink /etc/systemd/system/multi-user.target.wants/jenkins.service → /usr/lib/systemd/system/jenkins.service.
[root@ip-172-31-5-63 ~]# systemctl start jenkins
[root@ip-172-31-5-63 ~]# systemctl status jenkins
● jenkins.service - Jenkins Continuous Integration Server
     Loaded: loaded (/usr/lib/systemd/system/jenkins.service; enabled; preset: disabled)
     Active: active (running) since Sat 2023-11-25 01:04:17 UTC; 12s ago
 ```

## Configure Jenkins

1.) Jenkins is now installed and running on your EC2 instance. Now configure Jenkins

  Connect to http://<your_server_public_DNS>:8080 from your browser
  
  ![Screenshot_unlock_jenkins](https://github.com/aman1221997/Jenkins/assets/60047833/cb40f855-55d0-475a-8988-08f031d31342)

2.) Use the below command to display this password:

```
[root@ip-172-31-5-63 ~]# cat /var/lib/jenkins/secrets/initialAdminPassword
a7bcba64248d4b7f806f8dda3126bbb9
```
3.) The Jenkins installation script directs you to the *Customize Jenkins page*. Click *Install suggested plugins*.

4.) Once the installation is complete, the *Create First Admin Use*r will open. Enter your information, and then select *Save and Continue*.

  ![screenshot_create_first_admin_user](https://github.com/aman1221997/Jenkins/assets/60047833/d4747674-b436-4bb9-9985-88cb738ddba4)

5.) On the left-hand side, select *Manage Jenkins*, and then select *Manage Plugins*.

6.) Select the *Available* tab, and then enter *Amazon EC2 plugin* at the top right.

7.) Select the checkbox next to *Amazon EC2 plugin*, and then select *Install without restart*.


## Troubleshooting

### Job for jenkins.service failed because the control process exited with error code. See "systemctl status jenkins.service" and "journalctl -xe" for details

```
[root@ip-172-31-4-58 ec2-user]# service jenkins start
Redirecting to /bin/systemctl start jenkins.service
Job for jenkins.service failed because the control process exited with error code. See "systemctl status jenkins.service" and "journalctl -xe" for details.
[root@ip-172-31-4-58 ec2-user]#
```
First check Java version.
```
[root@ip-172-31-4-58 ec2-user]# java -version
openjdk version "1.8.0_382"
```
If old java version is present, upgrade it.

Uninstall all Java version installed

    yum remove java*   

Now install Upgraded version of Java by using below command

    yum install java-11-amazon-corretto-headless -y
    
or

    dnf install java-17-amazon-corretto -y


### Error: GPG check FAILED

### Failed to restart jenkins.service: Unit jenkins.service not found

### Failed to enable unit: Unit file jenkins.service does not exist

To resolve above 3 errors, import latest key by running command

    rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key





