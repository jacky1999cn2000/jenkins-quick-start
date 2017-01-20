# Credentials

* As we talked before, when Jenkins was installed, a default user "jenkins" was created; though the user was not loggable by default, it was a normal users (say, it can generate ssh key). So what we want to is, when we run jobs locally or remotely (via slave nodes, will talk about this topic in more details later), we should run them with the "jenkins" user. Therefore, we need to do the following :

  1. go to "jenkins-master", make "jenkins" user loggable
  ![8.gif](/screenshots/8.gif)

  2. verify that we can now logged in as "jenkins"
  ![9.png](/screenshots/9.png)

  3. make "jenkins" a sudo user
  ![10.gif](/screenshots/10.gif)

  4. login as "jenkins" user and create a ssh key (leave passphrase empty)
  ![11.gif](/screenshots/11.gif)

  5. create credentials in Jenkins `Jenkins -> Credentials`
  ![12.png](/screenshots/12.png)
  ![13.png](/screenshots/13.png)

* Let's add the ssh key into our localhost, so the "jenkins-master" can technically ssh into itself if necessary. (as we can see, the "jenkins-master" can't `ssh` into itself now, and `ssh-copy-id` didn't work either)

  ![14.png](/screenshots/14.png)
  ![15.png](/screenshots/15.png)

  In order to make this work, we need to do the following:

  1. with root user, create password for "jenkins" user (when we run `ssh-copy-id` to copy ssh key into localhost, we need to use password authentication, since the ssh key is not there yet - I know it sounds weird since localhost is itself and the key was already there, but just consider localhost as a remote/different machine)
  ![16.png](/screenshots/16.png)

  2. with root user, modify `etc/ssh/sshd_config` file, to set "PasswordAuthentication" to "yes" - in this way, `ssh-copy-id` will ask for password for authentication
    * P.S remember to change it back otherwise we can no longer ssh into this ec2 box from our local computer!!!)
    * the gif shows how we changed it back
  ![17.gif](/screenshots/17.gif)

  3. restart ssh service, login as "jenkins" user, perform `ssh-copy-id` and input password.
  ![18.png](/screenshots/18.png)

  4. now "jenkins" user on "jenkins-master" should be able to ssh into itself. We should do the same for all the IPs bound with "jenkins-master", so no matter how we refer it in our job, "jenkins" user can ssh into itself
  ![19.png](/screenshots/19.png)
  ![20.png](/screenshots/20.png)

  5. remember to change `etc/ssh/sshd_config` back and restart ssh service

* We not only want to use "jenkins" user on our local machine, we also want to "jenkins" user on remote machines (no matter the remote machine is a slave node - more on it later, or is just a remote machine which "jenkins-master" wants to ssh into). In order to achieve this goal, let's do the following:

  1. switch to "jenkins-slave" ec2 (we haven't made this as a slave node yet, so just think it as a remote machine), with root user, create "jenkins" user

  2. with root user, make "jenkins" user a sudo user (as we did above in "jenkins-master")

  3. with root user, change `/etc/ssh/sshd_config` and restart sshd service (as we did before in "jenkins-master")

  ![21.png](/screenshots/21.png)

  4. now switch to "jenkins-master", login as "jenkins" user, and run `ssh-copy-id jenkins@[jenkins-slave-private-IP]` and `ssh-copy-id jenkins@[jenkins-slave-public-IP]`

  ![22.png](/screenshots/22.png)

  5. now "jenkins-master" should be able to ssh into "jenkins-slave" smoothly

  ![23.png](/screenshots/23.png)

  6. don't forget to change `/etc/ssh/sshd_config` back in "jenkins-slave" and restart sshd service 
