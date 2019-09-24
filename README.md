# Welcome to the ELK wiki!

![](https://www.guru99.com/images/tensorflow/082918_1504_ELKStackTut1.png)

**[E]lasticsearch (ES)** - like the google for searching data
* -optimizes searches
* -uses rest API to request the data
* -can use many diff databases
* -databases (e.g postgreSQL) are only used to store data

**[L]ogstash (LS)** - data processing pipeline + data enrichment
* -process logs and sends to ES
* -data processing pipeline
* -data is handled as events (log file entries, ecommerce orders, chat messages, etc)
* -events are then processed by LS + shipped off to other destinations (ES, kafka queue, email message, HTTP endpoint, etc)
* -LS pipeline has 3 stages:
1. `inputs: can make use of a "plugin"  (e.g, a file, LS will read events from a file, sending events over HTTP, look up rows in a relational database, listen to a kafka queue, etc)`

1. `filters: all about processing events; can parse CSV, XML, JSON format + data enrichment (looking up IP and resolving geolocation or look up data in a relational database)`

1. `outputs (stashes): where events are sent. `

* -in a nutshell, LS receives events from 1 or more inputs, processes events, and sends them to stashes-
* -scalability > horizontally
* -designed in proprietart markup format similar to JSON
* -NOT only a markup language as we can add conditional statements and make a pipeline dynamic


**e.g  1: processing access logs from a web server**

![pic1](https://user-images.githubusercontent.com/31395956/65475363-50be2f00-de4d-11e9-8256-a52ea61d0c68.png)

we can instruct LS to process to read the access log, line by line, and process each line as an event.
there is an input plugin we can use called "file".
However, we can use a tool named **Beats** which is better designed for this task.

once LS has received a line, we can process it.

a line from an access log, has various pieces in it. BUT LS just receives the line as a **text string**.


so, we need to parse this text string to make sense of it (_structuring this unstructured data_)

We can do this buy writing a **Grok** pattern. This is like a **Regex** (regular expressions)

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcR52EV6AttLFuQP4zWmzw6M8S2o9jSVFGkba57CDEORWTBG3d_tmg)

This helps _match pieces of text_, and store them within fields.

This means we can have a unique field for the status code, request path, the IP address, etc.

![](https://static-www.elastic.co/v3/assets/bltefdd0b53724fa2ce/bltd908f368097df15a/5d0d59ed970556dd5800ed8b/diagram-logstash-filters.svg)


This is great because we don't want to send a long text string off to the stashes!

**[K]ibana (KBN)**
* -used for visualizations and dashboards,
* metrics, KPIs
* -calls the data using rest API 



**X-Pack (XP)** - adds functionality to to ES and KBN

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRrz_fWlEJiV9xmSqYDRvuYtmglLXH8nqJvonK1Z-RAA58Y6aoP)

1. offers security - authorization + authentication to ES+KBN
* authentication > can integrate with LDAP, AD , etc
* can also configure users and roles, and what they can access
2. monitoring - performance of ELK (cpu , mem usage, disk space, etc)
* can also set up alerts to detect problems (not limited to ELK monitoriing)- can be email, notified on **Slack**, etc


**e.g you can monitor if your CPU usage it high, application errors reach a threshold or detect anomalies like a user signing into multiple countries (diff geolocated IPs) within an hour**

3. you can export KBN dashboards and reports as PDFs on demand, or schedule them - this can also have triggered based reports
* can also have your own personal logo on reports
* can also export as CSV files

4. ML - interface is KBN and functionality is XP

![pic2](https://user-images.githubusercontent.com/31395956/65476315-7b5db700-de50-11e9-82d7-aa4bb58b951c.png)


* you can detect abnormalities in data based on wat the neural networks belives is NORMAL. 
* this can be coupled with alerting to have a ML job monitoring and alerting


5. **Graph**- _related data + works out of box/ no config needed_

* popular != to relevant

e.g. those who enjoy rolling stones might not like Mozart
the key factor is **Relevance**, NOT popularity

* exposes an API to integrate this into apps
* provides a plugin for KBN where u can use graphs
* UI > drill down , nav, explore related data


6. **Elastic Search SQL (ES SQL)**

* -ES queries written in Query DSL (JSON object defining the query)
* -might be too verbose
* -can send SQL queries to ES over HTTP or **JDBC driver**
* -ES translates the SQL query to Query DSL format behind scenes

![pic3](https://user-images.githubusercontent.com/31395956/65476913-81ed2e00-de52-11e9-8aa1-b78d7a10f13a.png)

* -the Translate API respondes with the Query DSL format that was from SQL
* basically, if you only know SQL, you can write your SQL query, translate it, and see what the Query DSL format looks like to start


**Beats** - collection of data-shippers

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSKMVcU6D0mq-kuelVB3cy25jsWORpmQhEaOwS1hBbFpIEenFo3lw)


* lightweight agents, with single purpose, you install on servers ---> sends data to LS or ES

e.g 1. **FileBeat** sends log entries to ES + LS. this comes shipped with modules for **common log files** (nginx, apache web server)

e.g 2. **MetricBeat** collects system level or service metrics
