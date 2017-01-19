# Configuration

* Jenkins installation created a user called "jenkins", and this user is not loggable by default - even if you tried login as "jenkins", you will be kicked out. However, the Jenkins process was owned by this "jenkins" user even though by default it cannot be logged (we'll make it loggable later).
![3.png](/screenshots/3.png)

* Jenkins has the concepts of "master" node and "slave" nodes, while the "master" node is supposed to only take care of the administrative tasks, and should offload the real build jobs into one or more "slave" nodes. When we do that, we need to make this default "jenkins" user loggable, add "jenkins" user in all the "slave" nodes, setup ssh keys exchanges, and then use this user to do all the jobs. (will detail this in later notes).

* Jenkins folder `/var/lib/jenkins`
![4.png](/screenshots/4.png)
  * All the job details were stored in `jobs` folder - including xml file to describe the job and workspace for the job
  * userContent can be used as a place to serve wiki, e.g. create a jobXXX.html in the folder, restart jenkins service `service jenkins restart`, and go to `http://[IP ADDRESS]/userContent/jobXXX.html`
  ![5.png](/screenshots/5.png)
  ![6.png](/screenshots/6.png)
