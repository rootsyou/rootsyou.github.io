---
layout: post
title: SqlMap Basics
description: Automated SQL exploitation of vulnerable webapps 
date: 2025-04-04 15:01:35 +0300
image: '/images/07.jpg'
tags: [Linux]
toc: true
---

Let's examine a trivial example where a login page looks up ``` username ``` & ```password``` from a database without sanitising input. It executes a query as follows:

  ```sql 
  SELECT * FROM users WHERE username = 'John' AND password = 'Un@detectable444';
  ```

An attack could be be made by injecting SQL in the password field:

```html
  Username: `John`
  Password: `abc' OR 1=1;-- -`
``` 

This executes the following query as a result:

```sql
  SELECT * FROM users WHERE username = 'John' AND password = 'abc' OR 1=1;-- -';
```


  * `'` terminates the string and allows an OR statement to be injected
  * `1=1` returns true so the original query executes as intended 
  * `-- -` 


### Automated SQL Injection
SQLMap is an automated tool for detecting and exploiting SQL injection vulnerabilities in web applications

* `sqlmap --wizard` provides a guided wizard rather than comand line flags
* `--dbs` Extract database names
* `-D database_name --tables` extract tables from a db
* `-D database_name -T table_name --dump` dump records from a table

First step is to look for a vulnerable URL or request

* `http://sqlmaptesting.thm/search?cat=1`
  Some URL’s use a GET parameter to retrieve data
* Any web app uing a GET param in the URL can be tested with `-u` flag
  `sqlmap -u http://sqlmaptesting.thm/search/cat=1`

