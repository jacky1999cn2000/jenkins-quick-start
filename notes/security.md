# security

* Login Jenkins with admin user, go to `Jenkins -> Manage Jenkins -> Configure Global Security`, select `Jenkins’ own user database` under `Security Realm`, and select `Matrix-based security` under `Authorization` (the default choice is `Logged-in users can do anything`)

* Add `admin` into `User/group`, and select all the privileges so `admin` can do anything

![7.png](/screenshots/7.png)

* Go to `Jenkins -> Manage Jenkins -> Manage Users` to create new users, and then repeate the steps above to grant the newly create user some privileges
