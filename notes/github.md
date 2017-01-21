# GitHub (all the plugins were already installed at the end of Jenkins installation)

* In order for Jenkins to be able to communicate with GitHub, we need to do the same ssh key exchange as we did between "jenkins-master" and "jenkins-slave". The steps are a little bit different:
  * go to "jenkins-slave" terminal (because all the jobs were run in "jenkins-slave", so the `git` command in the shell script was run in "jenkins-slave", therefore, we need "jenkins-slave" to be able to access GitHub), and get the .pub key
  ![41.png](/screenshots/41.png)

  * go to your github account, and in "settings", select "SSH keys", create a new SSH Key
  ![42.png](/screenshots/42.png)

* Now we could create a job to pull a repo from Github, build it, and push it to "jenkins-slave" (and we can visit it via "jenkins-slave" Apache server)

  * I forked this "https://github.com/mourngrym1969/static" into my own Github

  * Install Apache (along with other lamp stack softwares) and Git on "jenkins-slave"
  ```
    sudo yum update -y
    sudo yum install -y httpd24 git php56 mysql55-server php56-mysqlnd
    sudo service httpd start
    sudo chkconfig httpd on
  ```

  * Modify inbound rule for "jenkins-slave" security group so HTTP port is open
  ![43.png](/screenshots/43.png)

  * Use "jenkins-slave" public IP and make sure Apache test page is available
  ![44.png](/screenshots/44.png)

  * Create job
    ![45.png](/screenshots/45.png)
    ![46.png](/screenshots/46.png)

    ```
    ssh jenkins@172.31.25.166
    cd /home/jenkins/workspace
    rm -rf html
    git clone git@github.com:jacky1999cn2000/static.git html
    sudo cp -rf html/* /var/www/html/    
    sudo chown root:root /var/www/html -R
    sudo chmod 755 /var/www/html
    sudo chmod 644 /var/www/html/*
    ```
  * Run the job, and confirm the new content was pushed to Apache
  ![47.png](/screenshots/47.png)
  ![48.png](/screenshots/48.png)
