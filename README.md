# Dynamic Website Hosting on AWS

This project outlines the process of deploying and hosting a dynamic website on AWS, utilizing a range of AWS services to ensure high availability, scalability, security, and fault tolerance.

## Project Overview

I hosted a Dynamic Website on AWS for a DevOps project and utilized the following resources. I have created a GitHub repository with the reference diagram and deployment scripts. Please use this information to understand the project setup and deployment process.

## Architecture Overview

The website is hosted on Amazon EC2 instances within a Virtual Private Cloud (VPC) that is configured with both public and private subnets spanning two Availability Zones. The infrastructure leverages the following AWS resources:

1. **Virtual Private Cloud (VPC)**: Configured with both public and private subnets that spanned two availability zones.
2. **Internet Gateway**: Deployed to enable connectivity between the VPC instances and the wider internet.
3. **Security Groups**: Established as network firewall mechanisms to control inbound and outbound traffic.
4. **Availability Zones**: Leveraged two Availability Zones to increase system reliability and fault tolerance.
5. **Public Subnets**: Used for hosting infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**: Implemented for secure connections to assets within both public and private subnets.
7. **Private Subnets**: Web servers (EC2 instances) were placed within Private Subnets for enhanced security.
8. **NAT Gateway**: Allowed instances in both the private Application and Data subnets to access the Internet securely.
9. **EC2 Instances**: The website was hosted on these instances.
10. **Application Load Balancer**: Used to evenly distribute web traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
11. **Auto Scaling Group**: Automatically managed EC2 instances to ensure website availability, scalability, fault tolerance, and elasticity.
12. **AWS Certificate Manager**: Secured application communications using SSL/TLS certificates.
13. **Simple Notification Service (SNS)**: Configured to send alerts about activities within the Auto Scaling Group.
14. **Route 53**: Registered the domain name and set up a DNS record for the website.
15. **S3 Bucket**: Used to store application code and assets.

## Deployment

The infrastructure deployment is automated through scripts and configuration files available in a GitHub repository. The repository includes the following components:

- **Reference Diagram**: A visual representation of the AWS infrastructure and its various components.
- **Deployment Scripts**: Scripts that automate the provisioning and configuration of AWS resources.

## Usage

1. Clone the GitHub repository to your local machine.
2. Follow the instructions provided in the repository documentation to set up the required AWS resources.
3. Deploy the website code and assets to the designated S3 bucket.
4. Access the website using the registered domain name through Route 53.

## Bash Script for Server Setup

```bash
# This command indicates that the script should be interpreted and executed using the Bash shell
#!/bin/bash

# This command updates all the packages on the server to their latest versions
sudo yum update -y

# This series of commands installs the Apache web server, enables it to start on boot, and then starts the server immediately
sudo yum install -y httpd
sudo systemctl enable httpd 
sudo systemctl start httpd

# This command installs PHP along with several necessary extensions for the application to run
sudo dnf install -y \
php \
php-pdo \
php-openssl \
php-mbstring \
php-exif \
php-fileinfo \
php-xml \
php-ctype \
php-json \
php-tokenizer \
php-curl \
php-cli \
php-fpm \
php-mysqlnd \
php-bcmath \
php-gd \
php-cgi \
php-gettext \
php-intl \
php-zip

## These commands install MySQL version 8
# Install the MySQL Community repository
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 

# Install the MySQL server
sudo dnf install -y mysql80-community-release-el9-1.noarch.rpm
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
dnf repolist enabled | grep "mysql.*-community.*"
sudo dnf install -y mysql-community-server 

# Start and enable the MySQL server
sudo systemctl start mysqld
sudo systemctl enable mysqld

# This command enables the 'mod_rewrite' module in Apache on an EC2 Linux instance. It allows the use of .htaccess files for URL rewriting and other directives in the '/var/www/html' directory
sudo sed -i '/<Directory "\/var\/www\/html">/,/<\/Directory>/ s/AllowOverride None/AllowOverride All/' /etc/httpd/conf/httpd.conf

# Environment Variable
S3_BUCKET_NAME=aj-project-web-files 

# This command downloads the contents of the specified S3 bucket to the '/var/www/html' directory on the EC2 instance
sudo aws s3 sync s3://"$S3_BUCKET_NAME" /var/www/html

# This command changes the current working directory to '/var/www/html', which is the standard directory for hosting web pages on a Unix-based server
cd /var/www/html

# This command is used to extract the contents of the application code zip file that was previously downloaded from the S3 bucket
sudo unzip shopwise.zip

# This command recursively copies all files, including hidden ones, from the 'shopwise' directory to the '/var/www/html/'
sudo cp -R shopwise/. /var/www/html/

# This command permanently deletes the 'shopwise' directory and the 'shopwise.zip' file.
sudo rm -rf shopwise shopwise.zip

# This command sets permissions 777 for the '/var/www/html' directory and the 'storage/' directory
sudo chmod -R 777 /var/www/html
sudo chmod -R 777 storage/

# This command will open the vi editor and allow you to edit the .env file to add your database credentials
sudo vi .env

# This command will restart the Apache server
sudo service httpd restart
```

## Database Migration Script Using Flyway

Database migration was successfully accomplished using Flyway, an open-source tool that automates the process of applying incremental changes to the database schema.

The following script was used for migrating the database:

```bash
# Flyway Database Migration Script
#!/bin/bash

# Set environment variables for Flyway
FLYWAY_URL="jdbc:mysql://<your-database-url>:3306/<your-database-name>"
FLYWAY_USER="<your-database-username>"
FLYWAY_PASSWORD="<your-database-password>"

# Download Flyway if not already installed
if [ ! -f "flyway-commandline-<version>.tar.gz" ]; then
  wget https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/<version>/flyway-commandline-<version>.tar.gz
  tar -xzf flyway-commandline-<version>.tar.gz
  export PATH=$PATH:$PWD/flyway-<version>/flyway
fi

# Run Flyway migration
flyway -url=$FLYWAY_URL -user=$FLYWAY_USER -password=$FLYWAY_PASSWORD migrate
```

### Explanation of Flyway Migration Steps

1. **Environment Variables**: I set the Flyway environment variables, including the `FLYWAY_URL` for the database connection, along with `FLYWAY_USER` and `FLYWAY_PASSWORD` for authentication.

2. **Flyway Installation**: The script verified whether Flyway was already downloaded. If not, it downloaded the Flyway command-line tool and extracted it. The PATH environment variable was updated to include the Flyway executable.

3. **Database Migration Execution**: The `flyway migrate` command was executed using the provided connection details, ensuring all pending migration scripts from the `sql` directory were applied to the target database.

4. **Migration Scripts**: The required SQL migration scripts were placed in a designated `sql` directory and executed in sequential order (e.g., `V1__initial_setup.sql`, `V2__add_users_table.sql`) to maintain consistency and data integrity.

By following this migration process, the database schema was successfully updated to support the dynamic website hosted on AWS.

## Contributing

Contributions to this project are welcome. If you encounter any issues or have suggestions for improvements, feel free to open an issue or submit a pull request in the GitHub repository.



