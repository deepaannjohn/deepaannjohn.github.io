---
title: Jenkins 2 Fundamentals
date: "2020-03-020T23:50:37.121Z"
---

Why Jenkins?
* Automate the deployment
* Conitunous Integration (eg: build the PRs and validate)
* Continuos Deployment

Cruise Control (2001) - build, unit test etc , used XML config files
Hudson (released by Sun in 2005)
2010 - Oracle acquires Sun and applies for Hudson Trademark
Jan 2011 - Project renamed to Jenkins by community because of issues with Oracle
In 2014 - Cloudbees started supporting enterprise Jenkins  
April 2016 - Jenkins 2 Released

Jenkins Authentication:
1. LDAP integration possible (Manage Jenkins-> Configure Global Security)
2. Jenkins database

Jenkins Authorization
Two popular options:
* Logged in user can do anything
* Fine grained project based control possible

**Free style job**

**Set up jenkins job for nodejs app **
Sample node js Project for Demo
https://github.com/deepaannjohn/my-node-app

Create a new job / item in Jenkins
Select free style job option
Select source Code Managemnt -> Git -> repo url -> save

Installing Node on Jenkins
If we want to be able to build and test our node application, we have to first install Node on Jenkins! To do this, navigate to Manage Jenkins -> Manage Plugins -> Search for NodeJS and install the plugin.
Now navigate to Manage Jenkins -> Global Tool Configuration and look for the NodeJS heading. Install whatever version of NodeJS you require and click save:

Set up Build Environment 
Build -> Add Build step -> windows / shell script -> npm install
save

Build Now

Workspace in Jenkins:
To keep the files isolated for each job
A space in jenkins to checkout code and do other operations.
The build files will be there

**Using Pipelines in Jenkins**
In V2, pipelines are used more as they are more flexible.
Crete new Jenkins job (Pipeline job)
Here, we use pipeline script which is written in groovy, Use pipeline syntax option helper
Use the pipeline syntax option -> select bat / shell script -> genertae pipeline script -> copy script
Use the pipeline syntax option -> select nodejs environment -> genertae pipeline script -> copy script

note: pipeline syntax helper helps to genrate groovy scripts.

"node" 
From pipeline syntax genrator / helper, click on 'node' step. The following is generated:
node {
    // some block
}
This node is where the work will happenm workspace is created. Without node, the pipeline job wont run.

**Node allocation -> Master-agent model:**
Master node: The server where we run Jenkins
Master node has a pool of executors that can execute builds. When we click on 'Build Now', we need an executor to do the work. 
Master node has 'by default' has two executors (configurable) and these executors do the actual work. 
Sometimes, one master is not enough, then you can configure Agents (or slaves) which can run executor tasks. 

The node step allocates an agent an execuotor on one of the agents. 

Node step is required for pipeline jobd only. This is enabaled for flexibility so that multiple jobs can run in parallel. 

Example of a flexible pipeline workflow which makes use of 'node' step:
nodejs app - 
node 1 -  build, test the app and executor ends
then manual approval of deployment
node 2 - release the application 
This will not hold any of the executors unnecessarily

**Stages in pipeline:**
Totally optional but super useful.
This gives a quick overview of the execution, rather than checking the detailed console output, we can check the stages to follow along the execution.
Again use pipeline syntax for generating and understanding the details.
This gives a nice view of the pipeline / exection.

**Visualizing Test Results**
Pipeline Syntax helper-> 'step: General build Step -> Publish JUnit test result report -> specify the location of the JUnit file

**Plugins:**
Few useful plugins:
Code Coverage (Cobertura) - Install, then generate syntax through pipeline helpee and add in Pipeline
Blue Ocean - New UI, better experince for Pipelines

Pipeline as code:
JenkinsFile at the root of the repo
Pipeline  Script from SCM - 











