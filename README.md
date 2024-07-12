# PostgreSQL Oracle Cloud Server Setup and Configuration using pgAdmin

This README provides the steps to set up and configure a PostgreSQL server using pgAdmin on Oracle Cloud and linux server. Follow these instructions to ensure your PostgreSQL server is up and running.

## Prerequisites

- Oracle Cloud account
- Access to a Linux server
- Basic knowledge of Linux command line

## Steps

### 1. Connect to the Server

Connect to your server using SSH:
```bash
ssh your_username@your_server_ip
```
## Follow this to add postgreSql
https://www.digitalocean.com/community/tutorials/how-to-install-postgresql-on-ubuntu-20-04-quickstart


## Create a new role with a password
CREATE ROLE myuser WITH LOGIN PASSWORD 'mypassword';
\q and then exit to exit psql.


### 2. Update and Install Required Packages

First, update your package list to ensure you have the latest information about available packages and their dependencies:
```bash
sudo apt-get update
sudo apt-get install postgresql postgresql-contrib iptables-persistent
```
### 3. Configure PostgreSQL
Edit the PostgreSQL configuration files to allow remote connections.

Open postgresql.conf:
```bash
sudo nano /etc/postgresql/14/main/postgresql.conf
```
Find the line listen_addresses and set it to:
```bash
listen_addresses = '*'
```
Save and close the file.

Open pg_hba.conf:
```bash
sudo nano /etc/postgresql/14/main/pg_hba.conf
```
Add the following line to allow remote connections:
```bash
host    all             all             0.0.0.0/0               md5
```
Save and close the file.

### 4. Restart PostgreSQL
Restart the PostgreSQL service to apply changes:

```bash
sudo systemctl restart postgresql
```
5. Configure Firewall
Check the status of the firewall:
```bash
sudo ufw status
```
Allow traffic on port 5432:
```bash
sudo iptables -I INPUT 5 -p tcp --dport 5432 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
sudo netfilter-persistent save
```

### 6. Oracle Cloud Ingress Rules
Add port 5432 to the Oracle Cloud ingress rules to allow external connections.
Protocol: TCP
Port Range: 5432
Source: Your IP or 0.0.0.0/0 (for all IPs)


### 7. Connect to PostgreSQL using pgAdmin
Open pgAdmin and enter the following details:
HostName: ###.###.###.##
Port: 5432
Maintenance Database: #####
Username: ####

### 8. Verify PostgreSQL Service Status
Ensure PostgreSQL service is running:
```bash
sudo systemctl status postgresql
```
## Additional Configuration
### If you need to allow HTTP traffic, add port 80 and 22 to the Oracle Cloud ingress rules as well.

## Connection Command
### You can also connect to the PostgreSQL server using the command line:

psql -h <public-ip-server> -U admin -d postgres

Open your PgAdmin and setup your server and Your ready to Go...
