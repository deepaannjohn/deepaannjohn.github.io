---
title: Set up a website using github pages and circleci
date: "2020-01-17T23:46:37.121Z"
---

This blog is created using [Gatsby](https://www.gatsbyjs.org/) and 
hosted in [GitHub Pages](https://pages.github.com/). This blog is built and 
deployed using [circleci](https://circleci.com/). The source code is present in 
[GitHub](https://github.com/).

**Steps to set up a blog like this:**

Login to you GitHub account

Create a repository, repository name should be username.github.io
In my case, it was deepaannjohn.github.io

Clone the project and create an index.html file in the root folder.
Write 'Hello World' in index.html file.
Push the changes to master.
Fire up a browser and go to https://username.github.io
Your static web page has loaded and shows Hello World.

GitHub Pages has hosted your html file for you. So whatever you have in this 
repo will be hosted for you by GitHub. It will host / publish only from the master 
branch.

I used Gatsby to build my blog. Gatsby is a blazing fast modern site generator. 
So, I have the Gatsby project in another branch of the same repo.

So, now, create another branch 'dev'.

Create a Gatsby project in the dev branch. To do that,
Install gatsby-cli first. 
Then generate the site using the command 'gatsby new gatsby-site'
Then, 'cd gatsby-site'
And 'gatsby build'
More info here [gatsby quick start](https://www.gatsbyjs.org/docs/quick-start/)

This will create a 'public' folder in the gatsby project's root directory. This is the
static site which we need to host in github pages master branch. 

We will use circleci to build the Gatsby project for every commit to the 'dev'
branch then deploy the contents of the public folder to the master branch. Once it's 
in master branch, GitHub pages will publish it for us.

To set up circleci, login to circleci using your github account. The entire git repo is accessible
in circleci. Click on the option to 'Set up' the project. Select the right options (os and language). 
circleci will generate a config.yml file for us. This config.yml file should be present in the root 
folder of the gatsby project in 'dev' branch. 

In the gatsby project (dev branch), create a folder, .circleci and paste this ocnfig.yml file over there.
This file is only a template and it should be edited so that eveytime you commit to the dev branch, 
circleci with use this config.yml file to build and deploy your project.

My config.yml file is available in the dev branch once you clone it.
https://github.com/deepaannjohn/deepaannjohn.github.io

Now circleci, automatically creates an ssh key when you set up the project. But this will not have access to 
commit to the master. So I generated a user key from the settings of my project in circleci, which gives 
commit access to master branch of my project.























