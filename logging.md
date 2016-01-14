---
title: Logging Guide
---

# Getting started with logging in the Catalyze Platform

## Synopsis
Logging is an essential part HIPAA compliance, and the following sections of HIPAA apply directly to logging.  

* Section 164.308(a)(5)(ii)(C) “Log-in Monitoring”  calls for monitoring the systems touching patient information for login and access.  The requirement applies to “login attempts” which implies both failed and successful logins.
* Section 164.312(b) “Audit Controls”  broadly covers audit logging and other audit trails on systems that deal with sensitive health information. Review of such audit logs seem to be implied by this requirement.
* Section 164.308(a)(1)(ii)(D) “Information System Activity Review” prescribes review of various records of IT activities such as logs, systems utilization reports,  incident reports and other indications of security relevant activities.

Logging can be easily implemented for any of these configurations; Python + PostgresSQL, Node + MongoDB, PHP + MySQL, and  Ruby + PostgresSQL. It is important not only to store logs but to understand how to effectively filter and extract metrics from your logs.  Catalyze uses Elasticsearch, Logstash and Kibana to capture and visualize both application and database logs. More information can be found [here](https://www.elastic.co/).

## Pre-requisites
You have a PaaS account with Catalyze. If you don't, you can sign up for a 30-day trial [here](https://catalyze.io/signup).

You have a provisioned environment with a deployed application by following one of the following guides. [Node + MongoDB Guide](https://resources.catalyze.io/paas/paas-guides/node-mongo/), [PHP + MySQL Guide](https://resources.catalyze.io/paas/paas-guides/php-mysql/),  [Rails + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/rails-postgres/) or [Python + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/python-postgres/).

## Shipping your application logs

#### Python, PostgresSQL and Django
See [Python + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/python-postgres/).

Add this code snippet to settings.py: 
```
#settings.py
LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'handlers': {
        'file': {
            'level': 'DEBUG',
            'class': 'logging.StreamHandler'
        },
    },
}
```
#### Node and MongoDB
See [Node + MongoDB Guide](https://resources.catalyze.io/paas/paas-guides/node-mongo/). 

Insert ```console.log() ```whenever you want to log something.  
For example:
```
console.log('I want to log this thing!');
```

#### PHP, MySQL and Laravel
See [PHP + MySQL Guide](https://resources.catalyze.io/paas/paas-guides/php-mysql/).

Logging works easily right out of the box with Laravel. To Enable logging that works with Catalyze, you just need to edit the /config/app.php config file.

```
/*
|--------------------------------------------------------------------------
| Logging Configuration
|--------------------------------------------------------------------------
|
| Here you may configure the log settings for your application. Out of
| the box, Laravel uses the Monolog PHP logging library. This gives
| you a variety of powerful log handlers / formatters to utilize.
|
| Available Settings: "single", "daily", "syslog", "errorlog"
|
*/

'log' => 'daily',
```
Change the value from 'daily' to 'syslog'.  Below are some examples of how to log  other information within your Laravel application.
```
Log::info('This is some useful information.');
Log::warning('Something could be going wrong.');
Log::error('Something is really going wrong.');
```
More information on logging using Laravel can be found [here](http://laravel.com/docs/5.0/errors).

#### Ruby on Rails and PostgresSQL
See [Rails + Postgres Guide](https://resources.catalyze.io/paas/paas-guides/rails-postgres/). 

You can create a new logger to standard output:
```
log = Logger.new(STDOUT)
```
Or standard error:
```
log = Logger.new(STERR)
```
You can also log debug messages, info, or warnings.
```
log.debug("Created logger")
log.info("Program started")
log.warn("Nothing to do!")
```

## Accessing the logging dashboard
Go to the logging url provided for you. 

`https://{temporary domain}/logging/` 

For example:

`https://pod01163.catalyze.io/logging/`
 
 Or you can log into the Catalyze dashboard and click the logging link under your deployed environment. Log in using your Catalyze credentials.
  
![Logging login prompt](https://catalyze.box.com/shared/static/21gxyjjf8n3v4bo59vbctv17b0ze0nkn.png)

Your dashboard will look something like this.
![Logging Dashboard](https://catalyze.box.com/shared/static/b5cn6i4y9uy02ubj9g67qy5upxxe3y03.png)
## Searching and Filtering
Use the search bar at the top of the page to search for logs.  For example, to filter application logs you can use the query:
```
 syslog_program : supervisord
```
![Logging Query Example](https://catalyze.box.com/shared/static/8obuino907zpdhcivage9awn11zctct1.png)

You can view various time ranges of your logs by highlighting a portion of your events over time bar graph. 
![Logging filtering](https://catalyze.box.com/shared/static/hfe832wrjasujv4ktm01cvevs393iy8u.png)

More information on how to filter and search for logs can be found [here](https://www.elastic.co/guide/en/kibana/current/discover.html).


## Customizing your dashboard
Creating custom dashboards in Kibana can be useful if you want to visualize and  view logs in different ways.  To add a panel click the + button on the left side of a row and select a panel type.

![Logging add panel](https://catalyze.box.com/shared/static/y2gkcl76u5sd7xohyqomg3a15tm9jg42.png)

Configure your new panel by filling in the appropriate fields.  For this example we will set up a pie chart with panel type terms.

![Logging pie chart configuration](https://catalyze.box.com/shared/static/8ox33rd50xf7p6d0nsqo6cy4qbi2zipk.png)


More information on updating and customizing your Kibana dashboard can be found [here](http://www.elastic.co/guide/en/elasticsearch/reference/current/analysis.html).

### Saving  your dashboard
To save your custom dashboard click the save icon in the upper right hand corner, give your dashboard a name and hit enter.

![Save custom dashboard](https://catalyze.box.com/shared/static/n4la8a5q6159ambixk3o9tz17d2r8szk.png)

Now that your custom dashboard is saved you can load it by clicking the load icon in the upper right hand corner and selecting the dashboard you want to load.

### Updating the default dashboard
Once you have created a custom dashboard and saved it you can set it to be your default dashboard by clicking the save icon going to advanced and clicking save as home.  


## Extracting metrics
More information about extracting metrics from your logs can be found [here](https://www.elastic.co/guide/index.html).

## Extracting Messages from Elasticsearch

The guides from [Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/guide/1.x/index.html) will provide the most detail for executing searches with Elasticsearch.  

Using curl and [jq](https://stedolan.github.io/jq/) you can retrieve and filter the syslog messages from Elasticsearch.  The messages are stored as structured data in JSON format.

Elasticsearch can be queried directly by making a request to `https://podhostname/__es`

The Logging dashboard requires authentication to access.  
 
With curl, you can include the `-u USERNAME` option to specify a username. curl will present a password prompt when using the `-u USERNAME` option.  Or, to avoid re-entering the password repeatedly, use a `.netrc` file in the home directory that stores the user name and password and the `-n` option for curl:

~/.netrc:
```
machine podhostname
  login myusername
  password mypassword
```

Example:

~/.netrc:
```
machine pod09999.catalyzeapps.com
  login jamesbrown
  password ifeelgood
```

The index and type of the documents are specified in the path of the uri.

The index name is in the format `logstash-YYYY.MM.DD`.  For example, `logstash-2015.11.09`

The type is "syslog"

To return all the records in Elasticsearch from 2015-11-09, the uril would be `https://podhostname/__es/logstash-2015.11.09/syslog/_search`

The request will return a json document.  You can pipe the results through jq to filter the results and only show the syslog message:

`jq '.hits.hits[] | ._source| .syslog_message'`

The full command would be:

`curl -n -s https://podhostname/__es/logstash-2015.11.09/syslog/_search | jq '.hits.hits[] | ._source| .syslog_message'`

Search parameters can be added to a search by including a json document in the request:

Make a file called `es_params.json` to store the parameters of the request:

es_params.json:
```
{
    "query" : {
        "match" : {
            "syslog_message" : "Error"
        }
    }
}
```
Include the parameters in the request:

`curl -n -s -d @es_params.json https://podhostname/__es/logstash-2015.11.09/syslog/_search | jq '.hits.hits[] | ._source| .syslog_message'`

The results from the request are paginated and by default only 10 results are shown.

Add a `size` query parameter to the uri. Be aware that too many results will sigificantly increase the memory usage of Elasticsearch and negativley impact performance

`curl -n -s -d @es_params.json https://podhostname/__es/logstash-2015.11.09/syslog/_search?size=20 | jq '.hits.hits[] | ._source| .syslog_message'`
