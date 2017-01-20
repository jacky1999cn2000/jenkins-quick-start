# GitHub

* In order for Jenkins to be able to communicate with GitHub, we need to do the same ssh key exchange as we did between "jenkins-master" and "jenkins-slave". The steps are a little bit different:
  * go to "jenkins-master" terminal, and get the .pub key
  ![41.png](/screenshots/41.png)
  
  * go to your github account, and in "settings", select "SSH keys", create a new SSH Key
  ![42.png](/screenshots/42.png)
