# Capstone Project
 

## Project overview
Example Social Research Organization is a (fictitious) nonprofit organization that provides a website for social science researchers to obtain global development statistics. For example, visitors to the site can look up various data, such as the life expectancy for any country in the world over the past 10 years.

Shirley Rodriguez, a researcher at the organization, developed the website. She thought it would be valuable to share the data that she had gathered with other researchers. Shirley stores the data in a MySQL database, and the data is available through a PHP website that she built. She initially published the site through a commercial hosting company that provides limited support for technical issues and security.

Over the past year, Shirley’s website has grown in popularity. As a result of increased traffic, she started receiving complaints that the site is not as responsive as it used to be. She also experienced an attempted ransomware security breach. The security breach was unsuccessful, but her supervisor, Mateo Jackson, suggested that Shirley investigate new ways to host the website.

Shirley heard about Amazon Web Services (AWS), and initially moved her website and database to an EC2 instance that runs in a public subnet. She also runs an instance of MySQL on the same EC2 instance.

Shirley approached your team to make sure that her current design follows best practices. She wants to make sure that she has a robust and secure website. One of your colleagues started the process of migrating the site to a more secure implementation, but they were reassigned to another project. Your tasks are to complete the implementation, make sure that the website is secure, and confirm that the website returns data from the query page.

The following summary lists the solution requirements, and provides a diagram of the current environment.

### Solution requirements

- Provide secure hosting of the MySQL database
- Provide secure access for an administrative user
- Provide anonymous access to web users
- Run the website on a t2.micro EC2 instance, and provide Secure Shell (SSH) access to administrators
- Provide high availability to the website through a load balancer
- Store database connection information in the AWS Systems Manager Parameter Store
- Provide automatic scaling that uses a launch template
     

### The following parameters are used by the PHP application to connect to the database:

- /example/endpoint
- /example/username
- /example/password
- /example/database

These parameter values are case sensitive.

# Finished Diagram Results
![Alt text](AWS-Capstone-Project/diargram.png "Infrastructure")


# Step 0:  Inspect the archtecture 
- Inspect the example VPC. 
- Inspect the subnets. 
- Inspect the Security Groups.
- Inspect the AMI.  


# Step 1: Create a Cloud9 IDE




# Step 2: Get the Project Assets 
1. Clone the repository:
```sh
   wget https://aws-tc-largeobjects.s3.us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/capstone-project/Countrydatadump.sql
   wget https://aws-tc-largeobjects.s3-us-west-2.amazonaws.com/ILT-TF-200-ACACAD-20-EN/capstone-project/Example.zip
   ```
2. Extract the files to the Apache www folder:
```sh
   chown ec2-user Example.zip
   unzip Example.zip -d /var/www/html/
   ```
   
# Step 3: Install a web server on Amazon Linux 2
- My choice is LAMP web server.

### LAMP (Linux, Apache HTTP server, MySQL database, and PHP) stack
Original tutorial for LAMP Stack <a href='https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html'> Tutorial: Install a LAMP web server on Amazon Linux 2 </a>

```sh
sudo yum -y update
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2

sudo yum install -y httpd mariadb-server
sudo systemctl start httpd

sudo systemctl enable httpd
sudo systemctl is-enabled httpd
```




- Open port 80 from the security group of the Cloud9 EC2 instance
- Get the cloud9 EC2 public instance IP address and test that you can access the website 

# Step 4: Create a MySQL RDS database instance 
with the following specifications.
- [ ] Create a db subnet group 
- [ ] Databasetype: MySQL
- [ ] Template: Dev/Test
 - [ ] DBinstanceidentifier: Example
 - [ ] DB instance size: db.t3.micro
 - [ ] Storage type: General Purpose (SSD)
 - [ ] Allocatedstorage: 20GiB
 - [ ] Storageautoscaling: Enabled
 - [ ] Standbyinstance: Enabled
- [ ]  Virtualprivatecloud: ExampleVPC
- [ ]  Databaseauthenticationmethod: Passwordauthentication 
- [ ]  Initialdatabasename: (please remember the name!)
- [ ]  Enhancedmonitoring: Disabled

# Step 5: Create an Application Load Balancer
- Create target group 
- Create an auto scaling group 
- Lunch Web Instances in the private subnet

# Step 6: Importing the data into the RDS database
 _Importing the data into the RDS database instance from CLoud9 or by accessing the web instance via bastion host
 1. get the SQLDump file:
 

 2. connect to the RDS database, run this command:
```sh
mysql -u admin -p --host <rds-endpoint>
 ```
 3. Test that you can access the RDS DB 
 ```sh
 use exampledb;	
show tables; 

 ```
 
  
4. Import the data into the RDS database.
```sh
mysql -u admin -p exampledb --host <rds-endpoint>  < Countrydatadump.sql       
```
# Test the ALB 
- Test data was imported 
```sh
 use exampledb;	
show tables; 
select * from countrydata_final; 
 ```

# Step 7: Configure the system parameters in Parameter Store Systems Manager

Add the following parameters to the Parameter Store and set the correct values:
1. /example/endpoint 
2. /example/username   
3. /example/password  
4. /example/database 



### Summary of the tasks:
- Step 0:  Inspect the archtecture
- Step 1: Create a Cloud9 IDE
- Step 2: Get the Project Assets 
- Step 3: Install a web server on CLoud9 (LAMP in my case)
- Step 4: Create a MySQL RDS database instance 
- Step 5: Create an Application Load Balancer 
- Step 6: Importing the data into the RDS database 
- Step 7: Configure the system parameters in Parameter Store Systems Manager 