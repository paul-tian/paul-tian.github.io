---
layout: default
---

# Configuring Tomcat 9 / MySQL 8 on macOS Mojave 10.14.1
  
## Step 1: Install Tomcat 9

this step is inspired by this [post](https://wolfpaulus.com/mac/tomcat/).
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

Tomcat can be access at http://127.0.0.1:8080 or http://localhost:8080 by default. 
However, you need to add user to use the manager page, for more please visit [here](https://tomcat.apache.org/tomcat-9.0-doc/manager-howto.html). My option is to uncomment the user declairation in ```/TOMCAT/ROOT/conf/tomcat-users.xml``` and modify it as below:

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

[homepage](/)
