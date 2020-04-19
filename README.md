### Jenkins pipeline

Basic setup of a Jenkins pipeline on an AWS EC2 instance.

---
#### Installation of jenkins
**Steps**

1. Update the instance and install the java jdk

```
    sudo apt update
    sudo apt upgrade
    sudo apt install default-jdk 
```
2. Add the jenkins package repo key
```
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
```

3. Add the jenkins package repo address to the server's sources list
```
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
```

4 . Update the packages list and install jenkins and check if it's running
```
    sudo apt update
    sudo apt install jenkins
    sudo systemctl status jenkins
```
If you get an error about the signature not being verified because of a missing public key, run this command and run the above steps again.

```
# Replace THE_MISSING_KEY_HERE with the key (NO_PUBKEY) in the error message
sudo apt-key adv --keyserver hkp://pool.sks-keyservers.net:80 --recv-keys THE_MISSING_KEY_HERE
```
5. Head over to your browser and access Jenkins instance IP or the Public DNS on port 8080 `<instance-public-ip or instance-public-dns>:8080`
![jenkins](https://user-images.githubusercontent.com/5388763/79688196-4c2bee80-8255-11ea-9eab-e20ad00d6f66.JPG)

6. To get the password to log into the Jenkins dashboard, ssh into the instance and run the command `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`. Copy the string of characters that is outputed and paste in on the jenkins login dashboard.
This will login to a page with an option to install plugins, select the `Install suggested plugins` option.

7. Once the plugins are done installing, you should be looking at a landing page. On the left there is a menu bar. Select `Manage Jenkins>Manage Plugins`. Select the `Available` tab

8. In the search bar, search for blue ocean and pipeline-aws, select their checkboxes and click `Install without restart` at the bottom of the list. Once they are done installing, restart Jenkins via the instance by running `sudo systemctl restart Jenkins`. You will have to log back into the dashboard and should now be able to see a `Open Blue Ocean` button on the menu to the left. When you click on it, you get a page that looks like the one below.
![add a pipeline](https://user-images.githubusercontent.com/5388763/79688201-5a7a0a80-8255-11ea-9bd3-7ee76568cc71.JPG)

9. Click on the `Create a new pipeline` button then select `Github` because the repository is on Github. On the `Connect to Github` step, we need to create a token. Follow the link below to do so. Give the token an appropriate name and save it because you will not be able to see it again once you leave that page.
```
https://github.com/settings/tokens/new?scopes=repo,read:user,user:email,write:repo_hook 
``` 

9. Once you paste the `token` in the available field and select `Connect`, you should be able to connect to your Github account. If you are unable to connect to you Github account, you may need to downgrade the version of the `Github API` plugin to version 1.106. See [issue](https://issues.jenkins-ci.org/browse/JENKINS-61822) here.

10. When you succcessfully connect to your Github account, you should be able to select the repository you are going to setup a pipeline for. This repository should have `Jenkinsfile` configured as needed. 
![example pipeline](https://user-images.githubusercontent.com/5388763/79688531-8dbd9900-8257-11ea-8bfe-753764d5dd30.JPG)