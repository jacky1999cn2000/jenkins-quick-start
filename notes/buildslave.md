# Build Slave

To make it more clear, in "Credentials" chapter, we added the default "jenkins" user on "jenkins-master" as Jenkins' credential default account - which means all the jobs we run, Jenkins will use this user to ssh into itself or other machines (we first used `ssh key-gen` to create ssh key for this user, then used `ssh-copy-id` to copy the .pub key to both itself and the remote "jenkins-slave" server).

However, till now, the job was built (or run) on "jenkins-master" server (yes, inside the job, we ssh into "jenkins-slave" server and cat a file out, but the job itself was run on "jenkins-master").

Now, we want to add a slave node to Jenkins, so "jenkins-master" can focus on administrative tasks while offloading all the heavy-lifting of actually running the job to slave node(s).

In order to achieve this, we first need to create key exchanges for "jenkins" user on "jenkins-slave" so it can ssh into itself (remember in our job, the first thing is to ssh into "jenkins-slave"; if the job was supposed to be running on "jenkins-slave" after we added it as slave node, then "jenkins-slave" should be able to ssh into itself via its own IP) or other remote servers to do some operations - just like what we did for the default "jenkins" user on "jenkins-master".

Please be clear, before we add "jenkins-slave" as a slave node, the "jenkins-slave" server was just a remote server, so when we build the job, the job itself was still running on "jenkins-master".

Now, after we add "jenkins-slave" as a slave node, when we run/build jobs, "jenkins-master" wil not run the job on itself, but will offload the job (and all the information required to run the job) to "jenkins-slave" (well, being able to ssh into "jenkins-slave" was a prerequisite for sure).

When this happen, the "jenkins" user on "jenkins-slave" (since we used "jenkins" user on "jenkins-master" to ssh into "jenkins" user on "jenkins-slave") should have its own key (and done all the necessary key exchanges), so it can ssh into either itself or other remote machines.

The steps are identical to what we did on "jenkins-master":

  * modify `etc/ssh/sshd_config` file, restart ssh service, login as "jenkins" user, and generate key

  ![32.png](/screenshots/32.png)

  * perform `ssh-copy-id` to copy .pub key to itself (via localhost, public & private IPs; if "jenkins-slave" needs to ssh into other remote machines, then `ssh-copy-id` to the remote machine - just as what we did for "jenkins-master" so it can ssh "jenkins-slave")

  ![33.png](/screenshots/33.png)

  * make sure sshd_config was changed back and restart ssh service

  ![34.png](/screenshots/34.png)

Now we have configured "jenkins-slave" server, and let's add it as a slave node:

  * First, we need to make "jenkins-master" not care about running jobs. Go to `Jenkins -> Manage Jenkins -> Manage Nodes`, and click the config icon on master.

  ![35.png](/screenshots/35.png)

  * Change the "Usage" select to "Only build jobs ..."

  ![36.png](/screenshots/36.png)

  * Now back to "Manage Nodes" menu, and select "New Node", provide name and select type

  ![37.png](/screenshots/37.png)

  * Fill in all the information (I used private IP in the Host, so make sure "jenkins-master" can visit "jenkins-slave" via this IP)

  ![38.png](/screenshots/38.png)

  * Wait for the slave node to be synced

  ![39.png](/screenshots/38.png)

  * Run the job again, and see the logs to make sure the job was actually run on slave node

  ![40.png](/screenshots/38.png)
