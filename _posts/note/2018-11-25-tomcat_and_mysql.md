---
layout: post
title: About Tomcat and MySQL
categories: MySQL
description: Combining them.
keywords: Tomcat, MySQL
---

# Configuring Tomcat 9 / MySQL 8 on macOS Mojave 10.14.1
  
## Step 1: Install Tomcat 9

This step is inspired by this [post](https://wolfpaulus.com/mac/tomcat/).

### 1.1 Install JDK

#### 1.1.1

First install JDK, I choose to use [Oracle JDK](https://www.oracle.com/technetwork/java/javase/downloads/index.html) for rumors said that openJDK may not work fine with linking the MySQL. Here I installed JDK 11.

#### 1.1.2

After install the pkg file in downloaded dmg, try ```java -version``` and ```javac -version``` in terminal, if the version info returns, the JDK environment  has been deployed successfully.  

### 1.2 Install Tomcat

#### 1.2.1

Them download [Tomcat](https://tomcat.apache.org/download-90.cgi), choose Core, download and check integrity with ```shasum -a 512 /PATH/TO/THE/FILE```, then unzip it.

#### 1.2.2(Optional)

If needed, Tomcat can be symbolic linked to ```/Library/Tomcat``` by using

```bash
# to remove old link if there is a previous version:
sudo rm -f /Library/Tomcat

# create symbolic link:
sudo ln -s /PATH/TO/TOMCAT /Library/Tomcat
```

#### 1.2.3

After place the folder, the ownership and permissions should be configured.

```bash
# change ownership:
sudo chown -R <username> /PATH/TO/TOMCAT

# make  all scripts executable:
sudo chmod +x /PATH/TO/TOMCAT/bin/*.sh
```

#### 1.2.4

The Tomcat should now been set properly and you can run ```/PATH/TO/TOMCAT/bin/startup.sh``` and ```/PATH/TO/TOMCAT/bin/shutdown.sh``` to start or shut it.

#### 1.2.5

Tomcat can be access at http://127.0.0.1:8080 or http://localhost:8080 by default. However, you need to add user to use the manager page, for more please visit [here](https://tomcat.apache.org/tomcat-9.0-doc/manager-howto.html).  
My option is to uncomment the user declairation in ```/TOMCAT/ROOT/conf/tomcat-users.xml``` and modify it as below:

```xml
<role rolename="manager-gui"/>
<role rolename="manager-script"/>
<role rolename="manager-jmx"/>
<role rolename="manager-status"/>
<user username="tomcat" password="tomcat" roles="manager-gui"/>
```

## Step 2: Install MySQL 8

### 2.1 Install MySQL

install MySQL is much easier, just go to the MySQL community version download [page](https://dev.mysql.com/downloads/mysql/) and download the install file (don't forget to verify its integirty).
You may also need to add

```bash
export PATH=$PATH:/usr/local/mysql/bin
```

in ```.bash_profile``` to use MySQL in terminal, or to use it by GUI clients like [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) or [TablePlus](https://tableplus.io) (Free version could 
open two tabs simultaneously).

### 2.2 Run and Test

The installation step should lead you to setup the database root user password, but you can still initialize you database in ```Mac->System Preferences->MySQL```, where you can also stop or start the database service and do configuration as you wish.

Open terminal and use

```bash
mysql -u root -p
```

then input password of root user, then you should enter the MySQL CLI, try create new database and table by using command

```mysql
mysql> create database demo;
mysql> use demo;
mysql> create table user (
    ->   id serial,
    ->   name VARCHAR(20),
    ->   password varchar(20),
    ->   PRIMARY KEY (id)
    ->   );
```

and add some user in it:

```mysql
mysql> insert into user
    -> (id, name, password)
    -> values
    -> ('1', 'alice', 'alice123')
    -> ;
mysql> insert into user (id, name, password) values ('2', 'bob', 'bob456');
```

check if add them successfully:

```mysql
mysql> select * from user;
```

and the output should be

```mysql
+----+-------+----------+
| id | name  | password |
+----+-------+----------+
|  1 | alice | alice123 |
|  2 | bob   | bob456   |
+----+-------+----------+
2 rows in set (0.01 sec)
```

Then the MySQL 8 is ready to go.

## Step 3: Link Them

### 3.1 Connector/J

To connect Tomcat to MySQL, a MySQL [Connector/J](https://dev.mysql.com/downloads/connector/j/) is needed.
Download the platform Independent one and unzip it, put the ```mysql-connector-java-8.X.X.jar``` in ```/PATH/TO/TOMCAT/lib```, and we have the connector settled.

### 3.2 Test Page

This step is inspired by [Tomcat official guide](https://tomcat.apache.org/tomcat-9.0-doc/jndi-datasource-examples-howto.html#MySQL_DBCP_2_Example).  
The [MySQL official guide](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-usagenotes-tomcat.html) to connect maybe out of date, it's not recommended to follow.

#### 3.2.1

First, mkdir ```/PATH/TO/TOMCAT/webapps/test/WEB-INF/lib``` and ```/PATH/TO/TOMCAT/webapps/test/META-INF``` for file placement.

Configure ```context.xml``` in ```META-INF``` as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <Resource name="jdbc/demo" type="javax.sql.DataSource" maxActive="100" maxIdle="30" maxWait="10000" url="jdbc:mysql://localhost:3306/demo" driverClassName="com.mysql.jdbc.Driver" username="user_name" password="user_password" />
</Context>
```

#### 3.2.2

Then, configure ```web.xml``` in ```WEB-INF``` as follows:

```xml
<web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee
http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd" version="2.4">
    <description>MySQL Test App</description>
    <resource-ref>
        <description>DB Connection</description>
        <res-ref-name>jdbc/demo</res-ref-name>
        <res-type>javax.sql.DataSource</res-type>
    </resource-ref>
</web-app>
```

Noticed that here the label ```<web-app>``` ends with ```version="2.4"```, which means we need to install the [JSTL](https://www.oracle.com/technetwork/java/index-jsp-135995.html) and the version should be 1.1.X(not 1.2.X, for Servlet version 2.5 uses JSTL 1.2 and Servlet version 2.4 uses JSTL 1.1), you can get it from [Apache Tomcat Taglibs - Standard Tag Library](https://tomcat.apache.org/taglibs/standard/) or directly download from [here](http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/)(so there's no need to redirect from the [Apache Tomcat Taglibs - Standard Tag Library](https://tomcat.apache.org/taglibs/standard/)).  
Once you have JSTL, copy ```jstl.jar``` and ```standard.jar``` to webapp's ```WEB-INF/lib``` directory.

#### 3.2.3

Finally, Configure ```test.jsp``` in ```webapps/test/``` as follows:

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/sql" prefix="sql" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>

<sql:query var="data" dataSource="jdbc/demo">
    select name, password from user
    <!-- here the user table is the one created in Step 2.2 -->
</sql:query>

<html>

<head>
    <title>Database Test</title>
</head>

<body>

    <h2>Results</h2>

    <c:forEach var="row" items="${data.rows}">
        name: ${row.name}<br />
        password: ${row.password}<br />
    </c:forEach>

</body>

</html>
```

#### 3.2.4

After all the steps finished, start the Tomcat server and open a browser at <http://localhost:8080/test/test.jsp>, there should display the data from the user table.

## Summary

All these steps may seems easy to follow, but they did cost time to deal with unexpected errors and problems, which need patience to search for solutions and answers. After all, Tomcat 9 and MySQL 8 are too new(both become stable this year) so the existing method may not compatible.

### P.S.

This work reminds me about my graduation project, which also cost me lots of efforts to finish, these works and memories are all the treasures of my life, but that's another story.

[homepage](/)
