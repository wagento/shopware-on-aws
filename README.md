# shopware

# Shopware-Staging

Deploy a staging environment for Shopware where the web server and database server reside on the same instance. This environment is ideal for testing and development purposes. 

## Architecture Overview
<img align="center" src="https://github.com/wagento/shopware/blob/Staging/images/staging1.jpg">

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
![staging2.png](https://github.com/wagento/shopware/blob/Staging/images/staging2.png)
3. Click on the “Create stack” button. 
![staging3.png](https://github.com/wagento/shopware/blob/Staging/images/staging3.png)
4. Select "Choose an existing template" > "Upload a template file" > Choose file. 
![staging4.png](https://github.com/wagento/shopware/blob/Staging/images/staging4.png)
5. Upload the CloudFormation template file and click "Next".
![staging5.png](https://github.com/wagento/shopware/blob/Staging/images/staging5.png)
6. Fill in the details: 

   - **Stack name**: Enter the name of the stack that you want to set. 
   - **Environment Name**: Enter the environment name. 
   - **PrivateSubnet1CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
   - **PrivateSubnet2CIDR**: Enter the private CIDR mask or use the predefined subnet mask. 
   - **PublicSubnet1CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
   - **publicSubnet2CIDR**: Enter the public CIDR mask or use the predefined subnet mask. 
   - **SSHKey**: Select the SSH key. If none exists, create one. 
   - **VpcCIDR**: Enter the VPC CIDR subnet mask or use the predefined subnet mask. 

   ![staging6.png](https://github.com/wagento/shopware/blob/Staging/images/staging6.png)
7. Add tags and select the IAM Role. Click "Next" to deploy all resources. 

#### SSH Key Creation

1. Search for "Key Pair".
   ![staging7.png](https://github.com/wagento/shopware/blob/Staging/images/staging7.png)
2. Click on "Create key pair".
   ![staging8.png](https://github.com/wagento/shopware/blob/Staging/images/staging8.png)
3. Add the name and create the key pair.
   ![staging9.png](https://github.com/wagento/shopware/blob/Staging/images/staging9.png)

#### IAM Role

Add policies to IAM Role.

1. Search for IAM and go to IAM roles.
   ![staging10.png](https://github.com/wagento/shopware/blob/Staging/images/staging10.png)
2. Search for CloudFormation inside the roles page search and select the Role name.
   ![staging11.png](https://github.com/wagento/shopware/blob/Staging/images/staging11.png)
3. Add the AmazonEC2FullAccess policy to the role by clicking "Add permissions" and selecting "Attach policies".
   ![staging12.png](https://github.com/wagento/shopware/blob/Staging/images/staging12.png)
