# AWS

[stackoverflow](https://stackoverflow.com/users/10180167/subhojeet)

## ARCHITECTURE

   ![architecture](https://github.com/Subhojeetr/AWS/blob/master/AWS%20Logging%20Architecture.png)

Steps:

1) Create a **SNS topic**
2) Create **AWS API Gateway**
   - Create a **Resource**.
     - Create a **POST** Method.
      
     ![alt text](https://github.com/Subhojeetr/AWS/blob/master/src/gatewayAPI.bmp)
     
     
     - Configure it to use **SNS** service:
    
      ![alt text](https://github.com/Subhojeetr/AWS/blob/master/src/configuretosns.png)


     - **Map the header** as below:
    
    
     ![alt text](https://github.com/Subhojeetr/AWS/blob/master/src/MapQueryString.bmp)
     
3) Subcribe **SNS Topic** to **SQS Standard Queue** (As Of now SNS is not compatible to SQS FIFO)
    [Check:](https://stackoverflow.com/questions/41388783/end-to-end-sns-to-sqs-fifo-messages)
    
4) Create a **Lambda** function, set **SQS** as the trigger. Using python **boto3** to parse the event to write it to **s3**.

 > **SAMPLE CODE:**
``` python
import json
import boto3

def lambda_handler(event, context):
    # TODO implement
    bucket_name = "bucketfromlambdatos3"
    s3 = boto3.resource("s3")
    string = parse_event(event)
    encoded_string = string.encode("utf-8")
    file_name = "test.txt"
    s3_path = "/100001/20180223/" + file_name
    s3.Bucket(bucket_name).put_object(Key=s3_path, Body=encoded_string)
```


