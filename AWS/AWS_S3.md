# Amazon S3

### What is S3?

S3 stands for simple storage service. It is Amazons version of blob storage.Blobs (files/data) need to be stored in buckets (place to store blobs). And buckets can be accessed from anywhere globally. You cannot have buckets inside of buckets, like how you can have folders within a folder, creating file paths.

Blobs are stored in an unstructured manner inside the bucket, but You can however organise your files/data as you wish before uploading them into an S3 bucket. And the buckets can be themselves organised just not the data in them (like just throwing toys into a box).

The blobs are made private by default, but can be made public. When this is done they are given a url/endpoint, so they can be accessed from anywhere. When a request is made to access a blob you are charged, even if the request is denied a charge is made. Therefore, it is cheaper keep it private, and making public on if necessary or make sure to monitor your buckets, and manage security.



### Uses
* Store unstructured data
* Host static website on the cloud


### Learning to use CRUD methods

CRUD (Create, Read, Update, Delete). We will be implementing CRUD methods with S3 via AWS CLI. 


### Using AWS CLI to use S3

* #### Step 1: Create a vm instance

We need to create an EC2 instance. Using Ubuntu 22.0.4 AMI. For our security groups, we need the SSH port open, so we can SSH in, it doesn't matter if we have HTTP open as we are testing, but it is not necessary. 

* #### Step 2: Set AWS CLI & python version 3 or above 


```sudo apt-get install python3``` (to install python3 if machine doesn't have the correct version can check with python3 --version)

```sudo apt-get install python3-pip -y``` (allows to install pip which is the python package manager, so we can then install aws cli)

```sudo pip install awscli``` (use pip to install aws cli, so we can access aws and use s3 by use aws & s3 commands and sub commands)

```aws configure``` (allows us to start procedure to log in). Be aware once we have logged in via our instance, the credentials are saved. Stopping and restarting our instance will not cause us to have to log in again. This poses a security and permissions issue. So if you can clean the credentials off, before stopping the instance.

```alias python=python3``` (allows to associate a command with one of our choosing, so we can use our associated command instead of the default command)

* #### Step 3: Implement CRUD method with AWS CLI

We can check we have awscli by using the aws ```--version``` command. 

AWS commands start with ```aws``` and on Azure with ```az```.

Create an s3 bucket ```aws s3 mb s3://tech258-nick-first-bucket --region <region e.g. eu-west-1>```. If no region is specified the default region used on login will be used.

List an s3 bucket ```aws s3 ls s3://tech258-nick-first-bucket```

```echo This is the first line in a test file > test.txt (create a file)```

```aws s3 cp test.txt s3://tech258-nick-first-bucket``` (upload file to bucket) 

```aws s3 sync s3://tech258-nick-first-bucket .``` (download from bucket into current directory)

```aws s3 rm s3://tech258-nick-first-bucket/test.txt``` (remove file from cloud) 

```aws s3 rb s3://tech258-nick-first-bucket``` (rm bucket, but has to be empty)

```aws s3 rb s3://tech258-nick-first-bucket --force``` (rm bucket and everything in it)

### Implementing CRUD with Python Boto3

* #### First steps

While in our instance we must first install python Boto3 using the pip command, we also use sudo incase we may face permission issues. ```sudo pip install boto3``` 

To use python boto3 in our scripts we must first import it in the script with ```import boto3 ```

We can use nano as our text editor, and we create our script by adding .py. ```nano list-bucket.py```. We enter the editor and can create our script.

We execute the script by naming python3 then our script name ```python3 list-bucket.py```.

The python script to list a bucket is:
```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# List all S3 buckets
response = s3.list_buckets()

# Print bucket names
for bucket in response['Buckets']:
    print(bucket['Name'])
```
The python script to create a bucket is:

```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name
bucket_name = 'tech258-nick-test-boto3'

# Create the bucket
s3.create_bucket(Bucket=bucket_name)
```
You may need to specify the region, as permission issues may occur as the default region you may not have permissions. 

```
import boto3

# Specify the AWS region
region_name = 'eu-west-1'  # Replace with your desired region

# Create an S3 client with the specified region
s3 = boto3.client('s3', region_name=region_name)

# Now you can perform S3 operations using the 's3' client


# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name
bucket_name = 'tech258-nick-test-boto3'

# Create the bucket
s3.create_bucket(Bucket=bucket_name)
```




