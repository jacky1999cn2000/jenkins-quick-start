# Parameterized Builds

* With root user, install docker on "jenkins-slave", and add "jenkins" user in docker group (so jenkins user can run docker command)
```
  # yum install -y docker
  # service docker start
  # chkconfig docker on
```

* With root user, add "jenkins" user to docker group,and restart docker
```
  sudo usermod -aG docker jenkins
  service docker restart
```
*Many programs, for instance Postgresql, run as a user which is specifically named after the program. This is done for a variety of reasons including security (finer-grained permissions can be configured for this user) and monitoring (it's easy to see that processes owned by foo user are using the most memory in commands like top). I'm not 100% sure, but there's a good chance the scripting and/or Jenkins itself drop down to this user role to perform their actions.

In your case, if you are having the Jenkins user invoke docker commands, it would probably be easiest to add the Jenkins user to the docker group to allow it to invoke Docker commands without needing sudo:*

`sudo usermod -aG docker jenkins`

*Bewarned: All users of the docker group have, effectively, root-level permissions. Therefore, anyone who can run Jenkins jobs on this box could potentially escalate to root due to their access to the Docker CLI.*

* Create Job
![52.png](/screenshots/52.png)
![53.png](/screenshots/53.png)
![54.png](/screenshots/54.png)

* Run the job and confirm
![55.png](/screenshots/55.png)
![56.png](/screenshots/56.png)
