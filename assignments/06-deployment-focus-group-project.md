# Project 4: Deployment Focus (team)

**THIS IS NOT THE FINAL VERSION FOR SPRING 2020** W

**WORK AHEAD AT YOUR OWN RISK**

## Phase One

1. Create an amazon ec-2 instance (micro) using the Amazon AMI version of linux
2. Login to server with pem key you create (and will need to `chmod 400`) using this command, for example, `ssh -i otf.pem ec2-user@ec2-54-209-155-106.compute-1.amazonaws.com`\
3. Install python 3: `sudo yum install python3 -y`
4. Install virtualenv `sudo pip3 install virtualenv`
5. `mkdir github`
6. `cd github`
7. `mkdir virtualenvs`
8. `virtualenv --python=python3 virtualenvs/myenv`
9. `source virtualenvs/myenv/bin/activate`
10. `sudo yum install git`
11. `git clone https://github.com/chaoss/augur`
12. `cd augur`
13. `sudo yum install postgresql postgresql-server postgresql-devel postgresql-contrib postgresql-docs`
14. `sudo postgresql-setup initdb`
15. `sudo vim /var/lib/pgsql/data/pg_hba.conf`


Update the bottom of the file, which will read something like this, by default:
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     ident
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
# IPv6 local connections:
host    all             all             ::1/128                 ident
```

To read this:
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD

# "local" is for Unix domain socket connections only
local   all             all                                     trust
local   all             all             127.0.0.1/32            md5
# IPv4 local connections:
host    all             all             127.0.0.1/32            ident
host    all             power_user      0.0.0.0/0               md5
host    all             other_user      0.0.0.0/0               md5
host    all             storageLoader   0.0.0.0/0               md5
# IPv6 local connections:
host    all             all             ::1/128                 md5
```


Now that we've updated the authorization settings, we need to update PostgreSQL to enable remote connections to the database. At the command line enter:

16. `sudo vim /var/lib/pgsql/data/postgresql.conf`
17. Uncomment line 59:
```
#listen_addresses = 'localhost'          # what IP address(es) to listen on;
```
18. And update the line to enable connections from any IP address:
```
listen_addresses='*'
```
19. And uncomment line 63:
```
#port = 5432
```
So it reads
```
port = 5432
```
20. Now start the server: `sudo service postgresql start`
21. Create your Augur database
```
    sudo su - postgres 
    psql
    postgres=# create database augur;
    postgres=# create user augur with encrypted password 'mypass';
    postgres=# grant all privileges on database augur to augur;

```
22. Exit the Postgres interface and return to be your user. 
```
postgres-# \q
postgres@js-104-101:~$ exit
logout
(project4) sgoggins@js-104-101:~/augur$ 

```
23. Install NodeJS: (Notice the "." at the start of line 2. Important)
``` 
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash`
. ~/.nvm/nvm.sh
nvm install node

## Test that Node.js is installed and running correctly by typing the following at the command line.

node -e "console.log('Running Node.js ' + process.version)"
```
24. `git checkout dev` to switch to Augur's dev branch. 
24. Install and compile augur by doing a `make install`
```
**********************************
Setting up the database configuration...
**********************************

If you need to install Postgres, the downloads can be found here: https://www.postgresql.org/download/


1) Would you like create the Augur database, user and schema LOCALLY?
2) Would you like to add the Augur schema to an already created Postgres 10 or 11 database (local or remote)?
3) Would you like to connect to a database already configured with Augur's schema? 

Please type the number corresponding to your selection and then press the Enter/Return key.
Your choice: 2
Please set the credentials for your database.
Database: augur
Host: localhost
Port: 5432
User: augur
Password: mypass

Additionally, you'll need to provide a valid GitHub API key.
** This is required for Augur to gather data ***
GitHub API Key: (get yours here: https://github.com/settings/tokens)


The Facade data collection worker needs to clone repositories to run its analysis.
Would you like to use an existing directory, or create a new one?
1) Create a new directory
2) Use an existing directory

Please type the number corresponding to your selection and then press the Enter/Return key.
Your choice: 1
** You MUST use an absolute path. **
Desired directory name: /home/ec2-user/repos/
That directory already exists. Continuing...

**********************************
Generating configuration file...
**********************************

2020-02-21 18:08:22 ip-172-31-87-9.ec2.internal augur[6361] INFO Couldn't open augur.config.json, attempting to create.
augur.config.json successfully created
-- ----------------------------
CREATE SCHEMA augur_data;
CREATE SCHEMA
CREATE SCHEMA augur_operations;
CREATE SCHEMA
CREATE SCHEMA spdx;
CREATE SCHEMA
-- create the schemas

```

**/home/ec2-user/repos/**

```
Schema created
Would you like to load your database with some sample data provided by Augur?
1) Yes
2) No

Please type the number corresponding to your selection and then press the Enter/Return key.
Your choice: 1
```

```
Would you like to install Augur's frontend dependencies?

1) y
2) n

Please type the number corresponding to your selection and then press the Enter/Return key.
Your choice: 1

```

12. Have these steps completed before class on Tuesday, October 8. **If you have installation or setup issues, submit them to the Slack Channel.** 

## Phase Two
1. Clone your project repository in computationalmystic on GitHub. For example: https://github.com/computationalmystic/project4-group17 would be the repository for group 17 on this project, if there was a group 17. Your group will be between 1 and 16. Groups are listed on Canvas. These are different groups than your last project, and you should already know that.
2. Create a "production" and "test" branch for your repository. 
3. Change the default branch from "master" to "production"
4. Have each member of your team create their own branch
5. Each member of the team should perform the following steps in their branch
    - Create a "to do list" for writing code to implement the design document you turned in for the last assignment. It should be comprehensive, listing each class and method that should be created. 
    - Name your document 'lastname-teamnumber.md'. Mine would be goggins-17.md in that example. 
    - Use headings to organize subsets of work 
    - Put a copy of the team design document you are using, or a link to it, at the top of your file
6. When you have finished with your branch, issue a pull request and have one of your team mates merge it. 
7. Together, build a final document called 'overview.md', which includes the following: 
    - List of core objects from your to-dos (integrate you lists, identify any overlaps, smooth out overlaps [i.e., do not list login twice, make one list], submit a consolidated list.)
    - A short 3-5 sentence description of what your next steps would be if you were building this project as a team. 





