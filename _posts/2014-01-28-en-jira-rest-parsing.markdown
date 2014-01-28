---
layout: post
title: How to start working with jira rest
lang: en
date: 2014-01-28 23:59
categories:
    - en
summary: login to jira through rest, fetch some info on tickets, transform json with jq
tags:
- jira
- rest
- jq
- json
---

Jira
---------------------
JIRA is a proprietary issue tracking product, developed by Atlassian, used for bug tracking, issue tracking and project management([Remote API Reference][url-jira-remoting]). Jira support remoting services to make my life easier([Jira remoting][url-jira-remoting]). REST+JSON allow me to do a number of automatisaion in jira. This page is more notes for me on how to start with curl+jira+rest+json. for this page I am using jira v5.1.4 

Login
----------------------
Create bash script `login.sh` and do not forget make it excutabe `chmod u+x login.sh`:

```
#!/bin/bash
#login.sh file to get tokens from jira

#read information from command line -s to hide what you typing
read -sp "Password? "  var_password

#hit server to get cookies. change MY_JIRA_USER_NAME and MY_JIRA_SERVER.
curl -c cookie_jar.txt -H "Content-Type: application/json" -d "{\"username\" : \"MY_JIRA_USER_NAME\", \"password\" : \"$var_password\"}" https://MY_JIRA_SERVER/rest/auth/latest/session
#cookies should be stored cookie_jar.txt in working dir

```
After this step it is in working dir created cookie_jar.txt file that contains security tokens to query jira without sending user name and password in each request. 


Test Query 
----------------------
Create bash script `fetch_ticket.sh` and do not forget make it excutabe `chmod u+x fetch_ticket.sh`:

```
#!/bin/bash
#fetch_ticket.sh file to get ticket details from jira

#read ticket/issue number from console
read -p "issue number? "  var_ticket

#hit server to get ticket info in json format. Change  MY_JIRA_SERVER.
curl -b cookie_jar.txt -c cookie_jar https://MY_JIRA_SERVER/rest/api/2/issue/$var_ticket
#result should be printed out in console
```

As result you will see huge amount information that you can parse using json parsers. Json response will look like this:

```
{
  "expand": "renderedFields,names,schema,transitions,operations,editmeta,changelog",
  "id": "1234",
  "self": "https://.../rest/api/2/issue/1234",
  "key": "abs-1234",
  ....
}
```

Query by Custom Filter 
----------------------
Create bash script `fetch_filter.sh` and do not forget make it excutabe `chmod u+x fetch_filter.sh`:

```
#!/bin/bash
#fetch_filter.sh file to get  details on multiple tickets from jira

#Change MY_SPRINT_ID and MY_JIRA_SERVER
var_tickets=`curl -b cookie_jar.txt -c cookie_jar.txt -X POST -H "Content-Type: application/json" --data '{"jql":"issuetype in (Bug, \"New Feature\") AND Sprint = MY_SPRINT_ID ORDER BY Rank ASC, reporter ASC, key DESC","fields":["id","key","components","summary", "issuetype","customfield_1234"]}'  https://MY_JIRA_SERVER/rest/api/2/search` 

#transform for more readable format with jq
echo $var_tickets |jq '[.issues[] | {"Backlog Item ID":.key,  Component:[.fields.components[].name] | tostring,Type:.fields.issuetype.name,Summary:.fields.summary, "Custom Int":.fields.customfield_1234}]'

#result should be printed out in console
```

This script will fetch info using curl. for jql you can copy/paste what ever filter that you want to use in your automation. in fields key you should list all the attribute you want to get from jira. If you do not add fields section you will be 
flood with ticket information, that for debuging could be useful too.

second step transform json to make it simpler. It could be used any parser. One of them is jq ([JQ manual][url-jq-manual])

```
[
  {
    "Custom Int": 2,
    "Summary": "As user I want do that",
    "Type": "Bug",
    "Component": "[\"MY_COMPONENT\"]",
    "Backlog Item ID": "abc-1234"
  },
  {
    "Custom Int": 1,
    "Summary": "As user I want do this",
    "Type": "Task",
    "Component": "[\"OTHER_COMPONENT\"]",
    "Backlog Item ID": "abc-1235"
  }
]
```



Links
---------------------
[url-what-is-jira]: http://en.wikipedia.org/wiki/JIRA         "What is jira?"
[url-jira-remoting]: https://developer.atlassian.com/display/JIRADEV/Remote+API+Reference "Remote API Reference"
[url-jq-manual]: http://stedolan.github.io/jq/manual/         "JQ Manual"

