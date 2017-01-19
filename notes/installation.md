# Installation and Configuration

* Spin 2 ec2 servers with "jenkins-master" and "jenkins-slave" tags respectively
* Use all default settings except "jenkins-master" has "HTTP" and "SSH" open for its security group, while "jenkins-slave" only has "SSH" open for its security group.

* SSH into "jenkins-master" and do the following:
  * Install docker, nginx, and git
    ```
    # sudo su -
    # yum update -y
    # yum install -y docker nginx git
    ```
  * Install Jenkins (we need to add the Jenkins repository and install Jenkins from there)
    ```
    # wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
    # rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
    # yum install jenkins
    ```
  * As Jenkins typically uses port TCP/8080, we’ll configure Nginx as a proxy. Edit the Nginx config file (/etc/nginx/nginx.conf) and change the server configuration to look like this:
    ```
    server {
      listen       80;
      server_name  _;

      location / {
              proxy_pass http://127.0.0.1:8080;
      }
    }
    ```
    * Start the Docker, Jenkins and Nginx services and make sure they will be running after a reboot:
    ```
    # service docker start
    # service jenkins start
    # service nginx start
    # chkconfig docker on
    # chkconfig jenkins on
    # chkconfig nginx on
    ```
    * Launch the server and you should see this
    [1.png](/screenshots/1.png)
    * Grab the password via `cat /var/log/jenkins/jenkins.log`
    * Install recommended plugin, and create admin user
    * Done!
    [2.png](/screenshots/2.png)
