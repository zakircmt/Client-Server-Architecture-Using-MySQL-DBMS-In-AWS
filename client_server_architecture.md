# Task - Implement a Client Server Architecture using MySQL Database Management System (DBMS)

## The following instructions were followed to implement the above task:

### Step 1. Create and configure two linux-based virtual servers (EC2 instance in AWS)

- ```mysql server```
- ```mysql client```

__1.__ Two EC2 Instances of t2.micro type and Ubuntu 24.04 LTS (HVM) was lunched in the us-east-1 region using the AWS console.

__mysql server__

![Lunch Instance](./images/1.png)
![Lunch Instance](./images/2.png)
![Lunch Instance](./images/3.png)
![Lunch Instance](./images/4.png)
![Lunch Instance](./images/9.png)

__mysql client__

![Lunch Instance](./images/5.png)
![Lunch Instance](./images/6.png)
![Lunch Instance](./images/7.png)
![Lunch Instance](./images/8.png)
![Lunch Instance](./images/10.png)

The security group inbound rule for both instances was configured with the default SSH on port 22 with source from anywhere.

![Lunch Instance](./images/11.png)

![Lunch Instance](./images/12.png)

__2.__ Attached SSH key named __my-server-key__ to access the instance on port 22


## Step 2 - On ```mysql server``` Linux Server, install MySQL Server software

__1.__ The private ssh key permission was changed for the private key file and then used to connect to the instance by running

```bash
chmod 400 my-server-key.pem
```
```bash
ssh -i "my-server-key.pem" ubuntu@3.90.136.186
```
Where __username=ubuntu__ and __public ip address=3.90.136.186__

![Connect to instance](./images/13.png)

__2.__ __Update and upgrade Ubuntu__

```bash
sudo apt update && sudo apt upgrade -y
```
![Update ubuntu](./images/14.png)
![Update ubuntu](./images/14-1.png)

__3.__ __Install MySQL Server software__

```bash
sudo apt install mysql-server -y
```
![Install mysql](./images/15.png)

__4.__ __Enable mysql server__

```bash
sudo systemctl enable mysql
```
![Enable mysql](./images/16.png)


## Step 3 - On ```mysql client``` Linux Server install MySQL Client software.

__1.__ __Connect to the instance__

```bash
ssh -i "my-server-key.pem" ubuntu@3.81.81.50
```
Where __username=ubuntu__ and __public ip address=3.81.81.50__

![Connect to instance](./images/17.png)

__2.__ __Update and upgrade Ubuntu__

```bash
sudo apt update && sudo apt upgrade -y
```
![Update ubuntu](./images/18.png)
![Update ubuntu](./images/18-1.png)

__3.__ __Install MySQL Client software__

```bash
sudo apt install mysql-client -y
```
![Install mysql](./images/19.png)


## Step 4 - Use ```mysql server's``` local IP address to connect from ```mysql client```.

By default, both of the EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using __local IP addresses__. Use ```mysql server's``` local IP address to connect from ```mysql client```. MySQL server uses ```TCP port 3306``` by default so it has to be opened by creating a new entry in __inbound rules__ in __mysql server__ Security Groups.
For extra security, access to __mysql server__ by all IP addresses was not allowed, only the specific local IP address of __mysql client__ was allowed.

![server security rule](./images/20.png)


## Step 5 - Configure MySQL server to allow connections from remote hosts.

__Befor the configuration stated above, the following were implemented:__

__1.__ The security script of MySQL was run on __mysql server__ by running the command:

```bash
sudo mysql_secure_installation
```
![secure mysql](./images/21.png)

__2.__ __Access MySQL shell__

```bash
sudo mysql
```
![mysql shell](./images/22.png)

__3.__ __On mysql server, create a user named ```client``` and a database named ```test_db```__.

```bash
CREATE USER 'client'@'%' IDENTIFIED WITH mysql_native_password BY 'ZakirCmt$';

CREATE DATABASE test_db;

GRANT ALL ON test_db.* TO 'client'@'%' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

![DB](./images/23.png)

__4.__ __Now, configure MySQL server to allow connections from remote hosts__.

```bash
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
Locate ```bind-address = 127.0.0.1```

Replace ```127.0.0.1``` with ```0.0.0.0```


![bind-address](./images/24.png)

__5.__ __After configured MySQL server, Restart Mysql service__.

```bash
sudo systemctl restart mysql
```
![command](./images/25.png)


## Step 6 - From ```mysql client``` Linxus Sever, connect remotely to ```mysql server``` Database Engine without using SSH. The mysql utility must be used to perform this action.

```bash
sudo mysql -u client -h 172.31.42.190 -p
```
![Client to DB](./images/26.png)


## Step 7 - Check that the connection to the remote MySQL server was successfull and can perform SQL queries.

```bash
show databases;
```
![show db](./images/27.png)




At this point, this project is successfully complete.
This deployment is a fully functional MySQL Client-Server set up.



