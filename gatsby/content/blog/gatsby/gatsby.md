---
title: Getting started with GatsbyJS
date: "2020-01-19T22:12:03.284Z"
description: "Some basic inforamtion to get started with GatsbyJS"
---

GatsbyJS is a front end framework to build static websites and 
is based on JAMStack architecture.

[JAMStack architecture](https://jamstack.org/)

GatsbyJS is built on ReactJS, GraphQL, WebPack and JavaScript

ReactJS (to build user interfaces)
GraphQL (Query language to get the data that you want in the format you specify)

Static vs Dynamic Sites / Rendering
How Gatsby works? (Static rendering)
Using GraphQL API, blog data from mark down files are fetched and then Gatsby renders / genrates the final html (during build itself).
This same content (html file) is rendered for any user accessing the site.

Dynamic / Server Side rendering example
Site fetches catalogue information of a company
API to fetch Data fetches data from Database
Pages dynamically genrated / rendered in server
Then passed to the client side

To get started:
Install nodejs and gatsby-cli npm package globally to start developy gatsby based webistes
Use starter templates for a quick start. Starter templates are Boiler plater Gatsby sites.

Select any one starter template
Then create a new gatsby site as below
gatsby new mysite https://github.com/gatsbyjs/gatsby-starter-default.git

To view the site, use the command 'gatsby develop'
You can now view gatsby-starter-default in the browser.
  http://localhost:8000/
⠀
View GraphiQL, an in-browser IDE, to explore your site's data and schema
  http://localhost:8000/___graphql

Gatsby Pages are react components (a reusable piece of Javascript that encapsulates related presentation and logic)

File names and routing in Gatsby
src/pages in Gatsby is mapped to the route
Eg:
index.js    ---->   /
about.js    ---->    /about
contact.js    ---->    /contact
hello.js    ---->    /hello

Static react component pages can be created under src/pages.
Links to these pages are provided in index.js file
Use Gatsby's'Link' function to link internal pages and HTML href for linking external pages

Blogs can be written inside te blog folder as mark down files and Gatsby just publises them.
