# shopware

# Shopware-Staging

Deploy a staging environment for Shopware where the web server and database server reside on the same instance. This environment is ideal for testing and development purposes. 
![staging1.jpg](/images/sa-team/shopware/staging1.jpg)
## Prerequisites 

### Staging 

1. SSH Key. 

2. AmazonEC2FullAccess permission needs to be added in IAM Role CloudFormation. 

### How to Deploy 

Follow the respective procedures for deploying Shopware in staging and production environments using AWS CloudFormation templates. 

### Steps to Create Environment / Workflow 

**Staging Environment**

Installation of staging environment of Shopware using CloudFormation Template. 

#### Procedure 

1. Login to AWS account. 
2. Search for CloudFormation. 
![staging2.png](/images/sa-team/shopware/staging2.png)
3. Click on the “Create stack” button. 
![staging3.png](/images/sa-team/shopware/staging3.png)
4. Select "Choose an existing template" > "Upload a template file" > Choose file. 
![staging4.png](/images/sa-team/shopware/staging4.png)
5. Upload the CloudFormation template file and click "Next".
![staging5.png](/images/sa-team/shopware/staging5.png)
6. Fill in the details: 

	**Stack name**: Enter the name of the stack that you want to set. 
	**Environment Name**: Enter the environment name. 
	**PrivateSubnet1CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**PrivateSubnet2CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**PublicSubnet1CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
	**publicSubnet2CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
	**SSHKey**: Select the SSH key. If none exists, create one. 
	**VpcCIDR**: Enter the VPC CIDR subnet mask or use the predefined subnet mask. 
7. Add tags and select the IAM Role. Click "Next" to deploy all resources. 
![staging6.png](/images/sa-team/shopware/staging6.png)
#### SSH Key Creation
1. Search for "Key Pair".
![staging7.png](/images/sa-team/shopware/staging7.png)
2.	Click on "Create key pair".
![staging8.png](/images/sa-team/shopware/staging8.png)
3.	Add the name and create the key pair.
![staging9.png](/images/sa-team/shopware/staging9.png)
#### IAM Role
Add policies to IAM Role.
1.	Search for IAM and go to IAM roles.
![staging10.png](/images/sa-team/shopware/staging10.png)
2.	Search for CloudFormation inside the roles page search and select the Role name.
![staging11.png](/images/sa-team/shopware/staging11.png)
3.	Add the AmazonEC2FullAccess policy to the role by clicking "Add permissions" and selecting "Attach policies".
![staging12.png](/images/sa-team/shopware/staging12.png)



# Shopware-Production
Deploy a production environment for Shopware with separate RDS and EC2 instances for enhanced performance, security, and scalability. 

## Production Environment
Installation of Production Environment of Shopware using CloudFormation Template. In this template, we will create two separate RDS and EC2 instances. 

### Architecture Diagram
![production.jpg](/images/sa-team/shopware/production.jpg)
### Prerequisites 

1. SSH Key. 
2. AmazonEC2FullAccess permission needs to be added in IAM Role CloudFormation. 

### Procedure 

1. Login to AWS account. 
2. Search for CloudFormation. 
![production2.png](/images/sa-team/shopware/production2.png)
3. Click on the “Create stack” button. 
![production3.png](/images/sa-team/shopware/production3.png)
4. Select "Choose an existing template" > "Upload a template file" > Choose file.
![production4.png](/images/sa-team/shopware/production4.png)
5. Upload the CloudFormation template file and click "Next". 
![production5.png](/images/sa-team/shopware/production5.png)
6. Fill in the details: 
	**Stack name**: Enter the name of the stack. 
	**DBAllocatedStorage**: Set the database storage. 
	**DBEngine**: Select the database engine. 
	**DBInstanceClass**: Select the server type. 
	**DBName**: Enter the database name. 
	**DBPassword**: Enter the database password. 
	**DBUser**: Enter the database user name. 
	**EnvironmentName**: Enter the environment name. 
	**InstanceType**: Select the EC2 instance type. 
	**KeyName**: Select the key name. 
	**MultiAZDatabase**: Select if multiple availability zone databases are needed. 
	**PrivateSubnet1CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**PrivateSubnet2CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**PublicSubnet1CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**PublicSubnet2CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
	**SSHLocation**: Enter the private CIDR mask or use the predefined subnet mask. 
	**VpcCIDR**: Enter the VPC CIDR subnet mask or use the predefined subnet mask. 
7. Add tags and select the IAM Role. Click "Next" to deploy all resources. 
![production6.png](/images/sa-team/shopware/production6.png)
8. Migrate the database from localhost to RDS: 
	The database here will be deployed in the localhost of EC2 instance. So as we have a separate database created for the application, we need to migrate the database from the localhost to the RDS server, by taking the backup and restoring the same in the RDS server. 
	a. We need to login to the docker instance using the given commands. 
 		 #docker ps –a (it will list the container) 
 		 #docker exec –it containerID bash (replace the containerID with the real container ID) 
	b. Now the credentials are in the .env file of the web root directory copy the same and dump the database using the below command at the desired location 
 		#sudo cd your_desired_location (replace your_desired_location with proper path ) 
 		#mysqldump –uroot –p shopware > shopware.sql 
	c. Now we need to restore the database in the RDS server. 
  	 #mysql –hrdsendpoint –u user –p (replace rdsendpoint with the correct details and user with the correct user name) 
 		 #use shopware 
 		 #source shopware.sql 
 	d. Once the restore is completed, we need to update the .env file with the RDS database hostname, database name user and password. 
 		 #vim .env and then update the details 
     ![production7.png](/images/sa-team/shopware/production7.png)

### SSH Key Creation
1. To create SSH Keys, search for "Key Pair". 
![production8.png](/images/sa-team/shopware/production8.png)
2. Click on "Create key pair". 
![production9.png](/images/sa-team/shopware/production9.png)
3. Add the name and create the key pair. 
![production10.png](/images/sa-team/shopware/production10.png)

### IAM Role
Add policies to IAM Role. 
1. Search for IAM and go to IAM roles
![production11.png](/images/sa-team/shopware/production11.png)
2. Search for CloudFormation and select the Role name. 
![production12.png](/images/sa-team/shopware/production12.png)
3. Add the AmazonEC2FullAccess policy to the role by clicking "Add permissions" and selecting "Attach policies".
![production13.png](/images/sa-team/shopware/production13.png)

