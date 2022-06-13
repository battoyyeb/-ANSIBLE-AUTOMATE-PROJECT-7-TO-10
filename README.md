# ANSIBLE-AUTOMATE-PROJECT-7-TO-10

You have been implementing some interesting projects up until now, and that is awesome.

We have performed a lot of manual operations to seet up virtual servers, install and configure required software, deploy your web application from project 7 to 9.

This Project will make you appreciate DevOps tools even more by making most of the routine tasks automated with Ansible Configuration Management, at the same time you will become confident at writing code using declarative language such as YAML. Ansible Client as a Jump Server (Bastion Host)

A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. 
If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface.

**Task**

Install and configure Ansible client to act as a Jump Server/Bastion Host
Create a simple Ansible playbook to automate servers configuration

## Step - 1 INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

2. In your GitHub account create a new repository and name it ( ansible-config-mgt ).

3. Install Ansible


```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible

```

Check your Ansible version by running

```
ansible --version

```
![Screen Shot 2022-06-08 at 7 39 18 PM](https://user-images.githubusercontent.com/74343792/172735072-fc843fd1-69b1-4b2b-9f1c-1f4a447f45cb.png)

4. Configure Jenkins build job to save your repository content every time you change it – this will solidify your Jenkins configuration skills acquired in Project 9.

-- Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.
-- Configure Webhook in GitHub and set webhook to trigger ansible build.
-- Configure a Post-build job to save all (**) files, like you did it in Project 9.
Step 1 – Install Jenkins server


1. Create an AWS EC2 server based on Ubuntu Server 22.04 LTS and name it "Jenkins", on the EC2 instance allow port 8080. ssh into the terminal.

2. Install JDK (since Jenkins is a Java-based application)

```
sudo apt update
sudo apt install default-jdk-headless
```


3. Install Jenkins

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt update
sudo apt-get install jenkins
```

![Screen Shot 2022-06-05 at 4 05 50 PM](https://user-images.githubusercontent.com/74343792/172068547-9e25e1fc-3c72-49d2-867a-674a816227a0.png)

  
Make sure Jenkins is up and running

```
sudo systemctl status jenkins
```

![Screen Shot 2022-06-05 at 4 07 44 PM](https://user-images.githubusercontent.com/74343792/172068593-d73d417a-c4ff-454b-b74f-52745aaa18a5.png)

Go to your browser and type public-IP-Jenkins:8080
  
![Screen Shot 2022-06-05 at 4 09 26 PM](https://user-images.githubusercontent.com/74343792/172068733-b0cf44e9-f265-444a-9736-4c255ba7b48e.png)

To get the Administrator password use

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
4. By default Jenkins server uses TCP port 8080 – open it by creating a new Inbound Rule in your EC2 Security Group
  
Perform initial Jenkins setup.

![Screen Shot 2022-06-05 at 4 13 33 PM](https://user-images.githubusercontent.com/74343792/172068814-394e8aef-fbe7-4e09-a92b-e3f5a2a1d9b7.png)

![Screen Shot 2022-06-05 at 4 15 07 PM](https://user-images.githubusercontent.com/74343792/172068876-b196c27a-4773-45bc-98b4-f0c57109f302.png)
  
![Screen Shot 2022-06-05 at 4 16 15 PM](https://user-images.githubusercontent.com/74343792/172068913-d5d7cffd-3edf-4c9a-bf41-88019b6c0626.png)

  
You will be prompted to provide a default admin password
  
  
![Screen Shot 2022-06-05 at 4 18 13 PM](https://user-images.githubusercontent.com/74343792/172068972-6ccfaef6-3096-45e1-9763-5c138b3581aa.png) 
  
![Screen Shot 2022-06-05 at 4 22 37 PM](https://user-images.githubusercontent.com/74343792/172069082-029abb03-7d71-435a-9e6b-5a35b79ee17d.png)

![Screen Shot 2022-06-05 at 4 23 39 PM](https://user-images.githubusercontent.com/74343792/172069133-8b3c4bd5-a1aa-4e22-8055-3fe51186ca3a.png) 
  
![Screen Shot 2022-06-05 at 4 24 07 PM](https://user-images.githubusercontent.com/74343792/172069142-f5f3f30b-20f9-495b-bc8b-e3a6790d7805.png) 

In this part, you will learn how to configure a simple Jenkins job/project (these two terms can be used interchangeably). 
This job will will be triggered by GitHub webhooks and will execute a ‘build’ task to retrieve codes from GitHub and store it locally on Jenkins server.

![Screen Shot 2022-06-08 at 8 00 55 PM](https://user-images.githubusercontent.com/74343792/172737281-8b41fd44-b9da-4932-9e69-691987de7c4d.png)

1. Enable webhooks in your GitHub repository settings in the ansible-config-mg repository.

![Screen Shot 2022-06-08 at 8 06 14 PM](https://user-images.githubusercontent.com/74343792/172737466-cfc58ec6-3e73-4262-806f-5c919edf0901.png)

![Screen Shot 2022-06-08 at 8 08 33 PM](https://user-images.githubusercontent.com/74343792/172737677-7ea0dcd1-2b25-497f-8b21-27a9e20cae2f.png)

2.Go to Jenkins web console, click "New Item" and create a "Freestyle project" ans name it "Ansible"


Go to source code management and select git. Grab your url by going to to tooling folder, click code and copy the url.
To connect your GitHub repository, you will need to provide its URL, you can copy from the repository itself. In configuration of your Jenkins freestyle project choose Git repository, provide there the link to your ansible-conf-mg GitHub repository and credentials (user/password) so Jenkins could access files in the repository.

![Screen Shot 2022-06-08 at 8 25 39 PM](https://user-images.githubusercontent.com/74343792/172739152-04a57251-f47f-4c9f-a081-de5e1b176630.png)

![Screen Shot 2022-06-08 at 8 15 33 PM](https://user-images.githubusercontent.com/74343792/172738334-36ebd80c-09df-4137-bc76-3a43128abbc5.png)

Configure triggering the job from GitHub webhook:

![Screen Shot 2022-06-05 at 5 18 31 PM](https://user-images.githubusercontent.com/74343792/172071027-c54d70d6-6dae-4e21-bf4e-63491bf8d5f8.png)

Configure "Post-build Actions" to archive all the files – files resulted from a build are called "artifacts".

![Screen Shot 2022-06-05 at 5 19 35 PM](https://user-images.githubusercontent.com/74343792/172071063-7f77cc78-98f7-4f43-b288-86f78acffe7b.png)

Click save. Test your jenkins by clicking "Build now"

![Screen Shot 2022-06-08 at 8 28 15 PM](https://user-images.githubusercontent.com/74343792/172739425-471da9ac-b822-4637-8f90-6b875065c9c5.png)

Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) 
in following folder

```
sudo ls /var/lib/jenkins/jobs/Ansible/builds/<build_number>/archive/

```


Note: Trigger Jenkins project execution only for /main (master) branch.

![Screen Shot 2022-06-08 at 8 40 18 PM](https://user-images.githubusercontent.com/74343792/172740396-92284cc2-1279-4579-827c-f39bc9da1709.png)

![Screen Shot 2022-06-08 at 8 37 08 PM](https://user-images.githubusercontent.com/74343792/172740143-a7e6c390-a0b6-4c1a-bf7b-a1f3ddf27f0e.png)

## Step 2 – Prepare your development environment using Visual Studio Code
First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – youneed an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – Visual Studio Code (VSC), you can get it here.

After you have successfully installed VSC. Install a "remote development".

Also, configure it to connect to your newly created GitHub repository. On the homepage of your VS code, click the clone repository, slect the new repository you created, and select the folder in which it was stored.

![Screen Shot 2022-06-08 at 8 52 10 PM](https://user-images.githubusercontent.com/74343792/172741458-59ab8381-00ed-43c2-aa73-a1817cd2f1be.png)

![Screen Shot 2022-06-08 at 9 24 06 PM](https://user-images.githubusercontent.com/74343792/172744486-2232489d-f2e3-4227-a876-2481f651d3a3.png)

## BEGIN ANSIBLE DEVELOPMENT


1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.
Tip: Give your branches descriptive and comprehensive names, for example, if you use Jira or Trello as a project management tool – include ticket number (e.g. PRJ-145) in the name of your branch and add a topic and a brief description what this branch is about – a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm)


2. Checkout the newly created feature branch to your local machine and start building your code and directory structure

3. Create a directory and name it playbooks – it will be used to store all your playbook files.

4. Create a directory and name it inventory – it will be used to keep your hosts organised.

5. Within the playbooks folder, create your first playbook, and name it common.yml

6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

Check your branch on the VS code terminal

```
git branch
```
![Screen Shot 2022-06-08 at 9 29 20 PM](https://user-images.githubusercontent.com/74343792/172744955-9ad6e547-ad79-433b-8d2d-35b00ac54932.png)

Create a new branch using

```
git checkout -b prj-11
```

Create a directory "playbooks"

```
mkdir playbooks
```
Create a folder inventory

![Screen Shot 2022-06-08 at 9 36 17 PM](https://user-images.githubusercontent.com/74343792/172745718-3d61bd4b-dfa0-4b28-9477-b38bbc82fa7f.png)

Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively.

```
cd inventory
```
```
touch dev.yml staging.yml uat.yml prod.yml
```
![Screen Shot 2022-06-08 at 9 42 01 PM](https://user-images.githubusercontent.com/74343792/172746300-22ca19bc-62d8-47bb-97cf-7a877c37c0cc.png)

![Screen Shot 2022-06-08 at 9 42 17 PM](https://user-images.githubusercontent.com/74343792/172746313-0123dc4f-b081-4a41-9144-0248e6af38b7.png)


## Step 4 – Set up an Ansible Inventory

An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup.

Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you need to copy your private (.pem) key to your server. Do not forget to change permissions to your private key chmod 400 key.pem, otherwise EC2 will not accept the key. Now you need to import your key into ssh-agent:


```
eval `ssh-agent -s`
ssh-add <path-to-private-key>
ssh -A ubuntu@public-ip-address of your instance
```
![Screen Shot 2022-06-08 at 9 59 35 PM](https://user-images.githubusercontent.com/74343792/172748115-21e15f5e-da6b-4365-ad12-7d030ffa2bcf.png)

Also notice, that your Load Balancer user is ubuntu and user for RHEL-based servers is ec2-user.

![Screen Shot 2022-06-08 at 10 09 48 PM](https://user-images.githubusercontent.com/74343792/172749239-78b0f355-a4bc-40e2-9d5c-705d0e1c9a55.png)

Check if your Ansible-Jenkins EC2 instance can access the EC2 instances you created

```
ssh ubuntu@private-key
ssh ec2-user@private-key
```
![Screen Shot 2022-06-08 at 10 15 47 PM](https://user-images.githubusercontent.com/74343792/172749897-ac3f573b-b9fc-4dbc-9720-13e9d21db4f5.png)

![Screen Shot 2022-06-08 at 10 18 11 PM](https://user-images.githubusercontent.com/74343792/172750176-759e495c-9347-45a3-b369-c647ce48424f.png)

Update your inventory/dev.yml file with this snippet of code:


```
[nfs]
<NFS-Server-Private-IP-Address> ansible_ssh_user='ec2-user'

[webservers]
<Web-Server1-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web-Server2-Private-IP-Address> ansible_ssh_user='ec2-user'

[db]
<Database-Private-IP-Address> ansible_ssh_user='ec2-user' 

[lb]
<Load-Balancer-Private-IP-Address> ansible_ssh_user='ubuntu'
```
![Screen Shot 2022-06-08 at 10 24 17 PM](https://user-images.githubusercontent.com/74343792/172750851-ec191060-971c-44f7-83b8-2d9d3f1aeace.png)

## Step 5 CREATE A COMMON PLAYBOOK

It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

Update your playbooks/common.yml file with following code:

```
---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is at the latest version
      apt:
        name: wireshark
        state: latest
        
```
![Screen Shot 2022-06-08 at 10 34 17 PM](https://user-images.githubusercontent.com/74343792/172751989-63a6a376-ba1f-4be3-b1c3-c8d26f3e5648.png)

Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

Feel free to update this playbook with following tasks:


-- Create a directory and a file inside it

-- Change timezone on all servers

-- Run some shell script


## Step 6 – Update GIT with the latest code

Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. 
In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes – it is also called "Four eyes principle".

Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

Commit your code into GitHub:


1. use git commands to add, commit and push your branch to GitHub. (Use . to add all files that are yet to be committed)

```
git status

git add .

git commit -m "made some chnages"

```
 Click save. 

```
git add ../playbooks/

git commit -m "added playbooks folder"

```
Now push to the branch we are working on

```
git push origin prj-11

```

![Screen Shot 2022-06-08 at 10 50 59 PM](https://user-images.githubusercontent.com/74343792/172753920-380231c7-4d2c-43cc-a83f-c998688aba6f.png)

![Screen Shot 2022-06-08 at 10 52 43 PM](https://user-images.githubusercontent.com/74343792/172754077-3aabd1dd-c3ff-485c-8aa2-29e4b0b19aac.png)

Click on "compare &pull request". Click create pull request. If everything is okay, click merge merge pull request. Confirm merge.

![Screen Shot 2022-06-08 at 10 56 40 PM](https://user-images.githubusercontent.com/74343792/172754573-a2a0836a-57b7-41a4-a382-8a7cc98d9cb1.png)

![Screen Shot 2022-06-08 at 10 57 59 PM](https://user-images.githubusercontent.com/74343792/172754746-89ab3577-ffc8-42e1-aa7c-eddec1609f0a.png)

1. Create a Pull request (PR)

2. Wear a hat of another developer for a second, and act as a reviewer.

3. If the reviewer is happy with your new feature development, merge the code to the master branch.

4. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts)



to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

![Screen Shot 2022-06-08 at 11 06 51 PM](https://user-images.githubusercontent.com/74343792/172755745-bf94d9be-c9da-484f-a348-f92e10073f10.png)

Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.

```
git checkout main

cd ..

git status

git pull

```
![Screen Shot 2022-06-09 at 12 16 36 AM](https://user-images.githubusercontent.com/74343792/172763063-ee635169-809e-4254-b3ff-e8c72188aba8.png)

## Step 7 – Run first Ansible test


![Screen Shot 2022-06-09 at 12 20 08 AM](https://user-images.githubusercontent.com/74343792/172763406-e470710e-c229-4382-98af-ba84b600bf9e.png)

Click this to open a remote window. click on configure SSH Hosts. 

![Screen Shot 2022-06-09 at 12 26 57 AM](https://user-images.githubusercontent.com/74343792/172764128-e691c5a5-40b2-48e3-aa69-d4e5c10b8651.png)

Click on the first one. 

```
Host jenkins-ansible
    HostName 54.234.247.251
    User ubuntu
    IdentityFile /Users/adekunlebakare/Downloads/toheeb.pem
```
save the file. 

![Screen Shot 2022-06-09 at 1 10 22 PM](https://user-images.githubusercontent.com/74343792/172905313-9fcb1f26-902f-44d9-9548-1d07f0ca3f0a.png)


Click file, open folder, click ok.

![Screen Shot 2022-06-09 at 1 14 14 PM](https://user-images.githubusercontent.com/74343792/172905844-f6f3e8a1-ff8e-44be-afe9-3c02b6ef7390.png)

Run the ansible command on the iterm

```
ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml
```
![Screen Shot 2022-06-09 at 2 10 12 PM](https://user-images.githubusercontent.com/74343792/172915767-bf739710-9f2e-4a07-bec5-d4a516ea2747.png)

You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version

Your updated with Ansible architecture now looks like this:

Lets check for Webserver-2

```
ssh ec2-user@private key
```
```
which wireshark
```
```
wireshark --version
```
![Screen Shot 2022-06-09 at 2 40 33 PM](https://user-images.githubusercontent.com/74343792/172921030-d69ed646-eb6d-4c8f-8b21-7b40c28911b6.png)

![Screen Shot 2022-06-09 at 2 44 05 PM](https://user-images.githubusercontent.com/74343792/172921350-c25b0ec9-64ef-4e22-8e38-fee31ed31300.png)

Optional step – Repeat once again
Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

```
git branch
```
![Screen Shot 2022-06-08 at 9 29 20 PM](https://user-images.githubusercontent.com/74343792/172744955-9ad6e547-ad79-433b-8d2d-35b00ac54932.png)

Go back to yout branch

```
git checkout prj-11
```
On the playbooks, common. yml add the following

```
- name: create directory, file and set timezone on all servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: create a directory
      file:
        path: /home/sample
        state: directory

    - name: create a file
      file:
        path: /home/sample/ansible.txt
        state: touch

    - name: set timezone
      timezone:
        name: USA/Columbus
  ```
Click save. commit the changes as shown in the picture below.

![Screen Shot 2022-06-09 at 3 07 37 PM](https://user-images.githubusercontent.com/74343792/172925240-2e725284-9ea0-425b-b046-211b8eef0796.png)

Click the tick mark to commit the chnage. Click the ... and click push. Go back to your git hub and follow the prompts of "compare and pull"

Congratulations
You have just automated your routine tasks by implementing your first Ansible project! There is more exciting projects ahead, so lets keep it moving!
