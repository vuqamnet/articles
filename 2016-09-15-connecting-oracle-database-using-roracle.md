---
layout: post
title:  "Connecting to an Oracle Database using ROracle"
date:   2016-09-15
categories: R
tags: R ROracle Oracle
author: "Ali Rqiq"
---

> More that one way to allow you to connect to an Oracle Database using R. You can set up a database connection using RODBC, RJDBC, ORE and ROracle. ROracle is a R package that allows access to oracle database very quickly. This package is publicly open sourced and available on CRAN.

This is just some few examples to help you start connecting to an Oracle database and executing some SQL statements from R interface such as RStudio. Please note that you need to install the package ROracle and set up Oracle Instant Client. http://www.oracle.com/technetwork/database/features/instant-client/index-097480.html

~~~ R
## Load ROracle
library(ROracle)
 
## Create an Oracle instance and create one connection
drv <- dbDriver("Oracle")
 
## Create the connection string
host <- "my_db_server"
port <- 1521
sid <- "xe"
connect.string <- paste(
  "(DESCRIPTION=",
  "(ADDRESS=(PROTOCOL=tcp)(HOST=", host, ")(PORT=", port, "))",
  "(CONNECT_DATA=(SID=", sid, ")))", sep = "")
 
## Use username/password authentication to connect to Database
con <- dbConnect(drv, username = "hr", password = "hrhr", dbname=connect.string)
 
## Run a SQL statement by first creating a resultSet object rs
rs <- dbSendQuery(con, "select * from DEPARTMENTS")
 
## Fetch records from the resultSet into a data.frame
data <- fetch(rs)
 
## Extract all rows
dim(data)
data
 
## Free resources occupied by result set
dbClearResult(rs)
 
## Unload the driver
dbUnloadDriver(drv)
 
## GetInfo methods
dbGetStatement(rs)
dbHasCompleted(rs)
dbGetInfo(rs)
 
## DBI Drive rinfo
names(dbGetInfo(drv))
 
## DBI Connection info
names(dbGetInfo(con))
 
## DBIResultinfo
names(dbGetInfo(rs))
 
## Read the table DEPARTMENTS
dbReadTable(con, "DEPARTMENTS")
 
## Disconnect from the Database
dbDisconnect(con)
 
## Create a table
createTable <- "CREATE TABLE HR.TEST_TABLE (
         employee_number      NUMBER(5) NOT NULL,
         employee_name      VARCHAR2(15) NOT NULL
          )"
 
dbGetQuery(con, createTable)
 
## Insert statement
ins.data <- data.frame(employee_number = c(7770, 7771), employee_name = c('Ali', 'Youssef'))
 
# Good to create a resultSet object in case for example we want to add more data
resultSet <- dbSendQuery(con, "insert into HR.TEST_TABLE (employee_number, employee_name) values(:1,:2)", data = ins.data)
 
## Insert more data
ins.more.data <- data.frame(employee_number = c(7772), employee_name = c('Dawood'))
execute(resultSet, data = ins.more.data)
 
## Database Commit
dbCommit(con) 
 
## Clear result set when done with the query
dbClearResult(resultSet)
~~~
{: .language-R}