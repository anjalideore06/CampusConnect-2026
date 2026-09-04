# CampusConnect 2026

A student registration and login web application deployed on an AWS EC2
server using the **LEMP stack**.

## Project Overview

**CampusConnect 2026** provides:

-   Student Registration
-   Student Login
-   Email or Student ID based authentication
-   MariaDB database integration
-   PHP backend processing
-   Nginx web server
-   PHP-FPM for executing PHP scripts
-   Deployment on an AWS EC2 instance
-   Access through the EC2 public IP address

## Technology Stack

  Technology          Purpose
  ------------------- ---------------------
  AWS EC2             Cloud server
  Amazon Linux 2023   Operating system
  Nginx               Web server
  MariaDB 10.5        Database
  PHP 8.5             Backend
  PHP-FPM             PHP process manager
  HTML/CSS            Frontend

## Project Structure

``` text
CampusConnect-2026/
│
├── register.html
├── register.php
├── login.html
├── login.php
└── README.md
```

## Registration Form

The registration page contains the following required fields:

-   Full Name
-   Student ID
-   Email
-   College Name
-   Location
-   Event
-   Password

The form submits the data to `register.php` using the POST method.

## Login Form

The login page contains:

-   Email / Student ID
-   Password
-   Login button

Users can log in using either their registered email address or Student
ID.

### Successful Login

``` text
Welcome to CampusConnect!
```

### Invalid Login

``` text
Invalid username or password.
```

## Database

Database name:

``` text
campusconnect
```

Table name:

``` text
student1
```

### Table Structure

``` sql
CREATE TABLE student1 (
    id INT AUTO_INCREMENT PRIMARY KEY,
    fullname VARCHAR(100) NOT NULL,
    studentid VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    college VARCHAR(150) NOT NULL,
    location VARCHAR(100) NOT NULL,
    event VARCHAR(100) NOT NULL,
    password VARCHAR(255) NOT NULL
);
```

The password column is designed to store a hashed password rather than
plain-text credentials.

## LEMP Environment Setup

### 1. Install Nginx

``` bash
sudo yum install nginx -y
sudo service nginx start
```

### 2. Install MariaDB

``` bash
sudo yum install mariadb105-server -y
sudo service mariadb start
```

### 3. Install PHP

``` bash
sudo yum install php -y
sudo service php-fpm start
```

Install the PHP MySQL/MariaDB driver:

``` bash
sudo yum install php8.5-mysqlnd -y
```

Restart services after configuration changes:

``` bash
sudo service nginx restart
sudo service mariadb restart
sudo service php-fpm restart
```

Check service status:

``` bash
sudo service nginx status
sudo service mariadb status
sudo service php-fpm status
```

## Database Setup

Open MariaDB:

``` bash
sudo mariadb -u root -p
```

Create and select the database:

``` sql
CREATE DATABASE campusconnect;
USE campusconnect;
```

Create the `student1` table using the SQL shown above.

Check the database:

``` sql
SHOW DATABASES;
USE campusconnect;
SHOW TABLES;
DESCRIBE student1;
```

## Deployment Directory

The website files are placed in the Nginx web root:

``` text
/usr/share/nginx/html/
```

Example:

``` bash
cd /usr/share/nginx/html
```

Then place these files in the directory:

``` text
register.html
register.php
login.html
login.php
```

## Nginx and PHP-FPM

Nginx serves the HTML pages and forwards PHP requests to PHP-FPM.

The PHP-FPM service must be running:

``` bash
sudo service php-fpm status
```

The Nginx configuration should contain a PHP location block appropriate
for the installed PHP-FPM socket.

Before restarting Nginx, test the configuration:

``` bash
sudo nginx -t
```

If the test is successful:

``` bash
sudo service nginx restart
```

## AWS Security Group

The EC2 security group should allow:

  Type     Port Source                      Purpose
  ------ ------ --------------------------- -----------------------
  SSH        22 Your IP / required source   Server administration
  HTTP       80 `0.0.0.0/0`                 Public website access

For better security, SSH should preferably be restricted to a trusted IP
instead of allowing it from everywhere.

## Access the Website

After the EC2 instance is running and HTTP port 80 is allowed, open:

``` text
http://YOUR_PUBLIC_IP/
```

For example:

``` text
http://YOUR_PUBLIC_IP/register.html
```

and:

``` text
http://YOUR_PUBLIC_IP/login.html
```

Do not hard-code an old EC2 public IP in the application because public
IPv4 addresses can change when an instance is stopped and started unless
a static address is used.

## Application Flow

``` text
User
  |
  v
register.html
  |
  | POST
  v
register.php
  |
  v
MariaDB
  |
  v
student1 table
```

Login flow:

``` text
User
  |
  v
login.html
  |
  | POST
  v
login.php
  |
  v
MariaDB
  |
  +---- Valid credentials ----> Welcome to CampusConnect!
  |
  +---- Invalid credentials --> Invalid username or password.
```

## Security Notes

-   Do not upload database passwords to GitHub.
-   Do not commit private SSH keys such as `.pem` files.
-   Do not store plain-text passwords in production.
-   Use `password_hash()` when registering users and `password_verify()`
    when logging them in.
-   Use prepared SQL statements to reduce SQL injection risk.
-   Restrict SSH access to trusted IP addresses where possible.
-   Never publish AWS access keys or secret keys in this repository.

## Future Improvements

Possible improvements include:

-   Student dashboard
-   Logout functionality
-   Session-based authentication
-   Admin dashboard
-   Event registration limits
-   Form validation
-   Password reset
-   Email verification
-   Responsive mobile design
-   HTTPS using a domain name and SSL/TLS certificate

## Author

**CampusConnect 2026**

Student Web Application Project\
Built and deployed using AWS EC2 and the LEMP stack.
