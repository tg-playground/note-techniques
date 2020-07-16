# Jenkins Note



Jenkins main feature, `Jenkins Pipeline`.

Jenkins Pipeline provides an extensible set of tools for modeling simple-to-complex delivery pipelines "as code". The definition of a Jenkins Pipeline is typically written into a text file (called a `Jenkinsfile`) which in turn is checked into a project’s source control repository.

### Jenkins Setup

#### Install Jenkins

download from official website jenkins.war

Run `java -jar jenkins.war --httpPort=8080`

Browse to `http://localhost:8080`.

#### Configure Jenkins Job to use SSH keys

- Generate SSH key on Jenkins Server

```shell
$ mkdir .ssh
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/var/lib/jenkins/.ssh/id_rsa): /var/lib/jenkins/githubrepos/.ssh/id_rsa
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /var/lib/jenkins/githubrepos/.ssh/id_rsa
Your public key has been saved in /var/lib/jenkins/githubrepos/.ssh/id_rsa.pub
```

- Get public key from id_rsa.pub

```shell
$ cat id_rsa.pub
```

- Configure SSH Key for GitHub Project

Go to repository settings -> Deploy keys -> Add deploy key

Give a name such as ‘Jenkins Build Server’ and add the key. You may select ‘allow write access’ as well. Since we are using Jenkins only to pull repository, we’ll leave this checkbox unchecked.

- Add SSH Key inside Jenkins

Now go to Credentials from left pane inside Jenkins console and then click global:

After this, select ‘Add Credentials’:

Username: GitHub-DemoRepo

Private Key: copy from your server /var/lib/jenkins/githubrepos/.ssh/id_rs

After this, click ok to save the credentials.

- Configure Jenkins Job to use SSH keys

New Item -> xxx -> Add source: Git -> Credentials: select your add global Credentials "GitHub-DemoRepo"

- Verify that SSH key is working

If you go to build output, it should clearly list that SSH key is being used for authentication. 

> git --version # timeout=10
>
> using GIT_SSH to set credentials



References

[Configuring SSH authentication between GitHub and Jenkins](https://mohitgoyal.co/2017/02/27/configuring-ssh-authentication-between-github-and-jenkins/)



### Config GitHub webhook with Jenkins Server

GitHub Repository --> Settings --> Webhooks --> Add webhook --> Payload URL:

http://yourjenkinsmachine.com:8080/github-webhook/

Click Add webhook button



References

https://gist.github.com/misterbrownlee/3708738



### Jenkins Maven Project

#### Create And Build A Simple Project

New Item --> item name: myapp, Freestyle project --> buld Execute shell

```shell
echo "Hello from Jenkins!"
```

Back to the project page and click the “Build now” link.

View Build History panel in the bottom left corner of the page will show you the status of the build

little arrow --> select the “Console Output” menu item to access a detailed build log

#### Build A GitHub Project

Prepare:

Server: Generate a pair key

GitHub: Add SSH public key

Jenkins: Add Global Credentials

Process:

Configure --> Source Code Management --> Git --> Repository URL: your_github_url, Credentials: you_added_github_ssh. 

Configure --> Build Environment -->  Select “Invoke top-level Maven targets” --> Goals: clean package

Click "Save" to save the changes.

back to the project page and click the “Build now” link. Jenkins should clone your GitHub repository and build it with Maven. Once the build is complete, select the “Console Output” menu item to access detailed build information.



#### Create A Continuous Integration Pipeline For A GitHub Project

1. GitHub add webhook

- Navigate to your GitHub repository and click on the “Settings” tab.
- On the Webhook page, enter the following information:
  - Payload URL: *http://SERVER-IP/jenkins/github-webhook/*
  - Content type: select the “application/json” option.
  - Which events would you like to trigger this webhook?: select the “Just the push event”.
  - Click the “Add webhook” button to create the new webhook.

2. Jenkins Config

Configuration --> Build Triggers --> Github hook trigger for GITScm polling

Your pipeline is now configured, and all that remains is to test it. To do this, make a change to the repository - for example, by adding a line to the README file and committing the change.

Clicking the build number will show you the details of the commit that triggered the build

#### Jenkins New Item

1. Jenkins Freestyle Project

**Source Code Management**: Git 

Repository URL: 

Credentials: 

**Build Triggers**

GitHub hook trigger for GITScm polling

**Build**

Goals: clean package

**Save**

2. Multibranch Pipeline

####  **Branch Sources**

Repository URL: 

Credentials: 

**Build Configuration**

Mode: by Jenkinsfile

Script Path: Jenkinsfile

**Scan Multibranch Pipeline Triggers**

- [x] Periodically if not otherwise run

Interval: 1 minute



References

[Create A Continuous Integration Pipeline With Jenkins And GitHub On Oracle Jump Start](https://docs.bitnami.com/oci/how-to/create-ci-pipeline-jenkins-oracle/)

[Continuous integration with Jenkins](https://www.vogella.com/tutorials/Jenkins/article.html)



---



### Run Jenkins in Docker

#### Accessing the Jenkins/Blue Ocean Docker container

#### Setup wizard

#### Stopping and restarting Jenkins

### Fork and clone the sample repository on GitHub

### Create your Pipeline project in Jenkins

### Create your initial Pipeline as a Jenkinsfile

### Add a test stage to your Pipeline

### Add a final deliver stage to your Pipeline