# docker-mysql
This repo contains scripts to manage and use mysql docker repos:

##Rational
When using Docker containers for your applications, it makes sense to separate the mysql database into its own
container. It is also desirable for the mysql container to make its data persistent - otherwise, each time the
container is stopped, all the data will be lost. 

In addition to these essential features, it may also be desirable to establish more than one mysql container.
There may be many reasons to do so, but one example would be maintaining one container for production,
and a second container for development or demo/training purposes. Without docker, having two separate mysql
volumes would be not possible (or at least highly impractical). With docker, doing so is fairly simple.

Setting up and using mysql containers in the manner described is not as straightforward as one would think.
While the actual code is quite simple, there are a lot of pitfalls regarding volumes and container instances
that can be quite tricky to navigate. These scripts are meant to simply that process. The scripts themselves
are very simple and also serve as examples to create other scripts as needed.

##Setup and Configuration
1) Decide where you want your database directory(s) to go. The location is arbitrary, but allows you to decide
where to store the mysql directory, which normally is stored here: `/var/lib/mysql`.

2)Next, clone this repo.

3) Rename the `config.sh.sample` to `config.sh` and edit it. You want to define three variables:
```bash
# specify your 'root' username/password chosen when creating the mysql docker container
DB_GLOBAL_USER="root" 
DB_GLOBAL_PASS="test123"
# define the directory chosen in step 1, to be used for your persistent mysql volumes
DB_VOL_DIR="/home/USER/mysql_vols"
```

4) For each mysql container desired, execute `./run CONTAINER_STUB`. "CONTAINER_STUB" is the prefix given which
will uniquely name your mysql container. The name of the container follows this pattern: CONTAINER_STUB-mysql.
Remember this prefix as it will be used in every command. You only need to run a container once- that creates it
and then starts it. When you need to start the container at a later time, you simply execute:
`$ docker start CONTAINER_STUB-mysql`

##Usage/Commands
###addDB
Create a new database in mysql, along with the user/password for that database. The user may be existing or a new user.

Usage: `./addDB CONTAINER_STUB DBNAME USER PASS`
  where CONTAINER_STUB is the prefix for a named mysql container. These containers follow the naming convention of:
    CONTAINER_STUB-mysql
    
###export
Export a database, saving to a .sql file within the current directory.
  Usage: `./export CONTAINER_STUB DB_NAME`

###import
Import a .sql file into an existing database.
  Usage: `./import CONTAINER_STUB DBNAME FILENAME`

###run
This command will build and start a new mysql container. You only need to invoke it once for each container
desired. After this, the container can be started or stopped using the `docker start/stop` commands.
  Usage: `./run CONTAINER_STUB`
  
###runmysql
Run the mysql command-line program for the given container.
  Usage: `./runmysql CONTAINER_STUB`
  
###runshell
Run a bash shell within the mysql container.
  Usage: `./runshell CONTAINER_STUB`

