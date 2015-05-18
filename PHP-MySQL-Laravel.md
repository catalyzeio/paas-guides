# Getting Started with Catalyze and PHP-MySQL

# Prerequisites

## Tools
To get started with Catalyze and PHP-MySQL, you will need the following tools:
* [git](http://git-scm.com/)
* [Catalyze CLI](https://github.com/catalyzeio/catalyze-paas-cli)

## Sample Application

The examples below can be found [here](https://github.com/catalyzeio/php-example-app).  Fork and clone the example application to create your own working copy to deploy to Catalyze. 

## Local Testing

Local testing of your application can be done by following the instructions here.

## Catalyze Sign Up

The examples below assume you have a contract with Catalyze and have an environment that was provisioned by Catalyze Support.

Terms
* Environment ID: a GUID assigned to your environment by Catalyze. Eg:  `9cdde031-5342-4a0d-949c-31253227bd12`
* Environment Label: a name YOU picked for your environment.  Eg: `MyHealthApp-Production`

# Using the CLI

## Associating your code

Make sure you have [git](http://git-scm.com/) and [Catalyze CLI](https://github.com/catalyzeio/catalyze-paas-cli) installed and your environment lable ready.

Change directory to a working copy of your code or a fork of the [example application](https://github.com/catalyzeio/php-example-app) and run the following commands, entering your Catalyze username and password when prompted: 

```
# catalyze associate MyHealthApp-Production
Username:
Password:
"catalyze" remote added.
#
```

The cli added a git remote so you can now push code to catalyze.

```
# git remote -v
catalyze	ssh://git@git.sandbox.catalyzeapps.com:2222/csb0155-app01.git (fetch)
catalyze	ssh://git@git.sandbox.catalyzeapps.com:2222/csb0155-app01.git (push)
origin	https://github.com/catalyzeio/php-example-app.git (fetch)
origin	https://github.com/catalyzeio/php-example-app.git (push)
#
```

Your remotes will be unique to your origin and environment.

## Deploying your code for the first time

Ensure your code has a [Procfile](https://github.com/catalyzeio/php-example-app/blob/master/Procfile).  This file tells Catalyze 

The first time that you push your code to Catalyze, we require that you notify support@catalyze.io that you have pushed code and that your application finished building successfully. We will let you know when your application has finished the initial deployment. After the initial deployment, every time code is pushed to Catalyze using the command below, the application will be updated automatically.

To push your code to Catalyze, simply run the command below from within your working copy:

```
# git push catalyze master
... Lots of Build Output Here ...
remote: ---> XXXXXXX
remote: Finalizing Build (Note: This can take a few minutes to complete)....................................................
remote: Complete. Built Successfully!
#
```


## Modifying Environment Variables

# Updating your code

## Consuming the database URL from an Environment Variables

## Creating a task to setup the database schema

## Logging to the Catalyze ELK stack
