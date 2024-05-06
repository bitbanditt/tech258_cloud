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

To easily create a file with having to use nano or a file editor ```echo This is the first line in a test file > test.txt (create a file)``` This prints (echo) the line given into (>) the file we created. We can check the contents of the file with ```cat test.txt ``` which prints the file contents to the terminal.

The awscli command flow starts by calling aws and then the command and the sub command. We can get help on the available commands depending on the command e.g. 

* We can get aws commands with ```aws help```.
* An aws command e.g. aws s3 ```aws <command> help```.
* Or the sub command e.g. aws s3 rm ```aws <command> <subcommand> help```

To upload a file to the bucket we use the ```aws s3``` subcommand ```cp``` and specify the file and bucket we want to upload the file to  ```aws s3 cp test.txt s3://tech258-nick-first-bucket```  

To download a file from a bucket to our present directory, we use the ```aws s3``` subcommand ```sync``` and specify the bucket and file we want, then ```.``` which signifies the directory we are currently in ```aws s3 sync s3://tech258-nick-first-bucket/test.txt .``` If we leave the file path blank the entire bucket will be downloaded.

* #### The next commands are dangerous especially the last

To remove a file from the cloud we use the command ```rm``` subcommand ```aws s3 rm s3://tech258-nick-first-bucket/test.txt```  

To remove a bucket we use the ```rb``` subcommand, however the bucket must be empty ```aws s3 rb s3://tech258-nick-first-bucket``` 

To remove a bucket recursively (the bucket and all the contents and files within it) we use the command ```aws s3 rb s3://tech258-nick-first-bucket --force``` 

Be careful using these commands as you do not get asked if you want to delete, it just executes the command.

### Implementing CRUD with Python Boto3

* #### First steps

While in our instance we must first install python Boto3 using the pip command, we also use sudo incase we may face permission issues. ```sudo pip install boto3``` 

To use python boto3 in our scripts we must first import it in the script with ```import boto3 ```

We can use nano as our text editor, and we create our script by adding .py. ```nano list-bucket.py```. We enter the editor and can create our script.

We execute the script by naming python3 then our script name ```python3 list-bucket.py```.

Before writing our script we have to call boto3 ```import boto3```, this is like using python methods. We call the package so we can use the commands.

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
You may need to specify the region, as permission issues may occur as the default region you may not have permissions in. 

Code amended with region specified:

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

To upload data/files to the S3 bucket:

```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name and file to upload
bucket_name = 'tech258-nick-test-boto3'
file_name = 'path_to_file.txt'

# Upload the file
s3.upload_file(file_name, bucket_name, 'path_to_file.txt')
```

To download/retrieve content or files from the S3 bucket.

```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name and file to download
bucket_name = 'tech258-nick-test-boto3'
file_name_in_bucket = 'desired_file_name_in_bucket.txt'

# Specify where to save the downloaded file
local_file_name = 'local_file.txt'

# Download the file
s3.download_file(bucket_name, file_name_in_bucket, local_file_name)
```

To delete content or files from the S3 bucket

```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name and file to delete
bucket_name = 'tech258-nick-test-boto3'
file_name_in_bucket = 'desired_file_name_in_bucket.txt'

# Delete the file
s3.delete_object(Bucket=bucket_name, Key=file_name_in_bucket)
```

To delete the bucket

```
import boto3

# Create an S3 client
s3 = boto3.client('s3')

# Specify the bucket name to delete
bucket_name = 'tech258-nick-test-boto3'

# Delete the bucket
s3.delete_bucket(Bucket=bucket_name)
```





