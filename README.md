# DynamoDB-Lambda-Triggers

# Instructions

## Stage 1 - Creating the DynamoDB table

Head to the DynamoDB dashboard: [https://ap-southeast-2.console.aws.amazon.com/dynamodbv2/home?#tables](https://ap-southeast-2.console.aws.amazon.com/dynamodbv2/home?#tables)

## Stage 2 - Populating the table

Now you get to add as many cats as you like to the `cat-adoption` table.

## Stage 2a - Querying our data

Head to Explore table items


## Stage 3 - Setting up SNS

Head to the SNS console: [https://ap-southeast-2.console.aws.amazon.com/sns/v3/home?region=ap-southeast-2#/topics](https://ap-southeast-2.console.aws.amazon.com/sns/v3/home?region=ap-southeast-2#/topics)

## Stage 4 - Create the Lambda

Head to the Lambda console: [https://ap-southeast-2.console.aws.amazon.com/lambda/home?region=ap-southeast-2#/functions](https://ap-southeast-2.console.aws.amazon.com/lambda/home?region=ap-southeast-2#/functions)


```python
import boto3

def lambda_handler(event, context):
    sns = boto3.client('sns')
    accountid = context.invoked_function_arn.split(":")[4]
    region = context.invoked_function_arn.split(":")[3]
    for record in event['Records']:
        message = record['dynamodb']['NewImage']
        catName = message['catName']['S']

        response = sns.publish(
            TopicArn=f'arn:aws:sns:{region}:{accountid}:Adoption-Alerts',
            Message=f"{catName} has been adopted!",
            Subject='Cat adopted!',
        )
```

This code basically iterates through all the records that DynamoDB passes to the Lambda function, and then publishes a message to SNS.

## Stage 4a - Add DynamoDB permissions to the Lambda role

Head to the Lambda console: [https://ap-southeast-2.console.aws.amazon.com/lambda/home?region=ap-southeast-2#/functions](https://ap-southeast-2.console.aws.amazon.com/lambda/home?region=ap-southeast-2#/functions)

## Stage 5 - Enabling the DynamoDB Stream

Head back to the DynamoDB console: [https://ap-southeast-2.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-2#table?initialTagKey=&name=cat-adoption&tab=streams](https://ap-southeast-2.console.aws.amazon.com/dynamodbv2/home?region=ap-southeast-2#table?initialTagKey=&name=cat-adoption&tab=streams)

