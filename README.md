# Shopware-Production

Deploy a production environment for Shopware with separate RDS and EC2 instances for enhanced performance, security, and scalability. 

## Production Environment
Installation of Production Environment of Shopware using CloudFormation Template. In this template, we will create two separate RDS and EC2 instances. 

### Architecture Diagram
<img align="center" src="https://github.com/wagento/shopware/blob/main/images/Production1.jpg">

### Prerequisites 

1. SSH Key. 
2. AmazonEC2FullAccess permission needs to be added in IAM Role CloudFormation. 

### Procedure 

1. Login to AWS account. 
2. Search for CloudFormation. 
   ![production2.png](https://github.com/wagento/shopware/blob/main/images/production2.png)
3. Click on the “Create stack” button. 
   ![production3.png](https://github.com/wagento/shopware/blob/main/images/production3.png)
4. Select "Choose an existing template" > "Upload a template file" > Choose file.
   ![production4.png](https://github.com/wagento/shopware/blob/main/images/production4.png)
5. Upload the CloudFormation template file and click "Next". 
   ![production5.png](https://github.com/wagento/shopware/blob/main/images/production5.png)
6. Fill in the details: 

   - **Stack name**: Enter the name of the stack. 
   - **DBAllocatedStorage**: Set the database storage. 
   - **DBEngine**: Select the database engine. 
   - **DBInstanceClass**: Select the server type. 
   - **DBName**: Enter the database name. 
   - **DBPassword**: Enter the database password. 
   - **DBUser**: Enter the database user name. 
   - **EnvironmentName**: Enter the environment name. 
   - **InstanceType**: Select the EC2 instance type. 
   - **KeyName**: Select the key name. 
   - **MultiAZDatabase**: Select if multiple availability zone databases are needed. 
   - **PrivateSubnet1CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
   - **PrivateSubnet2CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
   - **PublicSubnet1CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
   - **PublicSubnet2CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
   - **SSHLocation**: Enter the private CIDR mask or use the predefined subnet mask. 
   - **VpcCIDR**: Enter the VPC CIDR subnet mask or use the predefined subnet mask. 

   ![production6.png](https://github.com/wagento/shopware/blob/main/images/production6.png)
   
7. Add tags and select the IAM Role. Click "Next" to deploy all resources. 

8. Migrate the database from localhost to RDS: 
   The database here will be deployed in the localhost of EC2 instance. So as we have a separate database created for the application, we need to migrate the database from the localhost to the RDS server, by taking the backup and restoring the same in the RDS server. 

   a. We need to login to the docker instance using the given commands: 
      ```bash
      docker ps –a # it will list the container 
      docker exec –it containerID bash # replace the containerID with the real container ID
      ```
      
   b. Now the credentials are in the .env file of the web root directory. Copy the same and dump the database using the below command at the desired location:
      ```bash
      sudo cd your_desired_location # replace your_desired_location with proper path 
      mysqldump –uroot –p shopware > shopware.sql
      ```
      
   c. Now we need to restore the database in the RDS server:
      ```bash
      mysql –hrdsendpoint –u user –p # replace rdsendpoint with the correct details and user with the correct user name
      use shopware 
      source shopware.sql
      ```
      
   d. Once the restore is completed, we need to update the .env file with the RDS database hostname, database name user and password:
      ```bash
      vim .env # update the details
      ```

   ![production7.png](https://github.com/wagento/shopware/blob/main/images/production7.png)

### SSH Key Creation

1. To create SSH Keys, search for "Key Pair". 
   ![production8.png](https://github.com/wagento/shopware/blob/main/images/production8.png)
2. Click on "Create key pair". 
   ![production9.png](https://github.com/wagento/shopware/blob/main/images/production9.png)
3. Add the name and create the key pair. 
   ![production10.png](https://github.com/wagento/shopware/blob/main/images/production10.png)

### IAM Role

Add policies to IAM Role.

1. Search for IAM and go to IAM roles.
   ![production11.png](https://github.com/wagento/shopware/blob/main/images/production11.png)
2. Search for CloudFormation and select the Role name. 
   ![production12.png](https://github.com/wagento/shopware/blob/main/images/production12.png)
3. Add the AmazonEC2FullAccess policy to the role by clicking "Add permissions" and selecting "Attach policies".
   ![production13.png](https://github.com/wagento/shopware/blob/main/images/production13.png)
