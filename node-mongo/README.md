# Deploying a Node+Mongo Application on the Catalyze Platform

## Prerequisites

## Local Node.js Application

## Building and Deploying the Application

To deploy the application, the git remote must be set up using the [Catalyze CLI](https://github.com/catalyzeio/catalyze-paas-cli).

1. Associate the local project with your provisioned environment.

   ```
   $ catalyze associate my-node-app
   ```

   This will add a new git remote named `catalyze` to that local repo. It will ask for credentials - these are the username and password that you use in the dashboard.

2. Push master to catalyze to build your code.

   ```
   $ git push catalyze master
   ```

3. You should see build output after you push. After pushing, you can check the environment status - the build status should now be finished.

   ```
   $ catalyze status
   environment state: deployed
    app01 (size = c1, build status = finished, deploy status = None)
    mongo01 (size = c1, image = mongodb, status = running)
   ```

3. Ask Catalyze to deploy that build to your environment. After it's deployed, the `deploy status` will change to `running`.

## Mongo Integration

## Redeploying the Application with Mongo

All that is required to rebuild a codebase with changes and redeploy is a single push.

```
$ git push catalyze master
```

No need to talk to Catalyze - the redeploy will happen automatically if the build is successful.

## Working with Logs

In the Catalyze dashboard, the temporary URL for the application is visible on $screen:

> TODO screenshot

Using this URL, a Kibana + ElasticSearch interface is exposed at `/log`.

> TODO screenshot

TODO link to ELK docs


## Redeploying with Log Integration

Just another push to redeploy.

```
$ git push catalyze master
```
