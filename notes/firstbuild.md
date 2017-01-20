# First Build

* How to create a job and build it
  * Create a new item `Jenkins -> New Item`: provide name and choose freestyle type
    ![24.png](/screenshots/24.png)

  * Add a simple `Execute Shell` in the job
    ![25.png](/screenshots/25.png)

  * Add a `index.log` file on "jenkins-slave" under `/home/jenkins` with content of `jenkins slave`
    ![26.png](/screenshots/26.png)

  * Click `Build Now`
    ![27.png](/screenshots/27.png)

* How to create a view
  * Click the plus sign to create a new view
    ![28.png](/screenshots/27.png)

  * Provide name and select which job you want to be included in this view
    ![29.png](/screenshots/29.png)
    ![30.png](/screenshots/30.png)

  * The view is available 
    ![31.png](/screenshots/31.png)
