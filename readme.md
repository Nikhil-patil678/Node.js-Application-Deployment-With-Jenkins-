# Deployment Of Node.js Application By Using Jenkins Freestyle Project 

> ### This  project  guids  you  how  to  deploying a  nodejs  application using jenkins  freestyle jobs .

## Objective :
 "The objective of this project is automate the deployment of node.js application on ubuntu server using jenkins freestyle project" .

## Prerequisites :

* ### Ubuntu server with :

1. Node.js(v18+)

2. npm(v8+)

3. pm2(v6+)

4. Jenkins installed and running 

5. Jenkins user added to sudoers

## Environment Setup :

* #### Execute the following command to prepare your ubuntu server for jenkins jobs . 

### Step 1 : Switch to jenkins user

#### bash

* sudo su - jenkins
---
### Step 2 : Update Packages 

#### bash 

*  sudo apt-get update -y 
---


### Step 3 : Install node.js 

1. #### Add nodesource repo :

#### bash 

*  curl -fsSL https://deb.nodesource.com/setup_lts.x | bash -

2. #### Install node.js :

#### bash 

*  sudo apt install -y nodejs

---

### Step 4 : Install npm 

#### bash 

* sudo apt install npm 
---


### Step 5 : Install pm2 

#### bash

* sudo npm install -g pm2
---

 ### Step 6 : Verify Installation 

#### bash 

* node -v

* npm -v

* pm2 -v

![my image](./images/Screenshot%202025-08-25%20050811.png)


## Jenkins Job Setup :

* #### We use three jenkins freestyle projects .

### Step 1 : Create first job 

- #### node-pull-job 

   * **Purpose** : Pull latest code from github .

   * **Configuration** : 
     * Source Code Management --> Git  -->  < Repository url >
     
     * Post-build actions --> Add --> Build other projects --> node-install-job 

     * Save the job 

 ![my image](./images/Screenshot%202025-08-25%20174351.png)

    ---

###  Step 2 : Create second job 

- #### node-install-job 

  * **Purpose** : Install node.js dependencies .

  * **Configuration** : 
    *    Build Steps --> Execute Shell :

  #### bash

        cd /var/lib/jenkins/workspace/node-pull-job

        sudo npm install

    * Post-Build Actions --> Add --> Build other projects --> node-deploy-job 

    * Save the job 

![my image](./images/Screenshot%202025-08-25%20174618.png)

 ---
    
### Step 3 : Create Third Job 

- #### node-deploy-job 

  * **Purpose** : Deploy the app and run using the pm2 . 

  * **Configuration** : 
    * Build Steps --> Execute Shell : 

  #### bash 

      cd /var/lib/jenkins/workspace/node-pull-job

      sudo pm2 start app.js --name node-app || pm2 restart node-app

  * **Set Email Notification** :
    * Post-Build Actions --> Add --> Email Notification -> Enter Email 
  * Save the job
  
  ![myimage](./images/Screenshot%202025-08-25%20175233.png)

---

### Step 4 : Build The Job 

* #### Go to the first job and build it . 

* #### Check the output is success .

---
 
## Job Flow : 

node-pull-job --> node-install-job --> node-deploy-job 

## Access Application : 

* ### http://< server-public-ip >: 3000

## Notes : 
* ### Ensure port 3000 is open in server security group .

* ### Use pm2 for production to keep the app running in the background .

## Output : 

![my image](./images/Screenshot%202025-08-25%20073403.png)

## You Now have a Node.js App Deployed With Jenkins Freestyle Jobs .























