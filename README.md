# AWS Serverless Word Count Automation Using Lambda, S3 & SNS

## Project Overview

This project demonstrates how AWS services can be used to build a serverless, event-driven automation solution. A Python-based AWS Lambda function is triggered automatically whenever a text file is uploaded to an Amazon S3 bucket. The function reads the file, calculates the total word count, and publishes the result to an Amazon SNS topic, which sends an email notification containing the result.

The main goal of this project was to gain practical experience with AWS serverless services and understand how event-driven components work together without the need for any manually managed servers.

## Architecture

```
Text File Uploaded
        ↓
Amazon S3 Bucket
        ↓
S3 ObjectCreated Event
        ↓
AWS Lambda Function (Python + Boto3)
        ↓
Word Count Calculated
        ↓
Amazon SNS Topic
        ↓
Email Notification Sent
```

## AWS Services Used

* Amazon S3
* AWS Lambda
* Amazon SNS
* Amazon CloudWatch
* AWS IAM

## How It Works

1. An Amazon S3 bucket is created to store uploaded text files.
2. An Amazon SNS topic is created with an email subscription to deliver notifications.
3. An AWS Lambda function is created in Python and configured with an execution role that grants access to S3, SNS, and CloudWatch Logs.
4. The S3 bucket is configured with an event trigger so that any new object upload automatically invokes the Lambda function.
5. When a file is uploaded, the Lambda function retrieves the file from S3, counts the words it contains, and publishes the result to the SNS topic.
6. SNS delivers an email notification containing the word count.
7. Amazon CloudWatch is used to log each invocation for monitoring and troubleshooting.

## Implementation

### Step 1 – Resource Setup

An S3 bucket was created to serve as the storage location for uploaded text files. An SNS topic was also created, with an email subscription added and confirmed so that notifications could be delivered.

### Step 2 – Configuration

The Lambda function's execution role was configured with the permissions required to read objects from S3, publish messages to SNS, and write logs to CloudWatch. The function's timeout was increased from the default 3 seconds to allow enough processing time for larger files.

### Step 3 – Service Integration

An S3 event notification was configured to trigger the Lambda function on `ObjectCreated` events. The SNS topic ARN was added to the Lambda function code so that the function could publish results to the correct topic. This integration allows the three services to work together without any manual intervention once a file is uploaded.

### Step 4 – Testing

The solution was tested by uploading sample text files to the S3 bucket. Each upload automatically triggered the Lambda function, which calculated the word count and published it to SNS. A confirmation email containing the word count was received for each test file, and CloudWatch logs were reviewed to confirm successful execution.

## Screenshots

<img width="1415" height="396" alt="image" src="https://github.com/user-attachments/assets/57422891-224f-4bd3-a464-1fb50df04017" />

* **Amazon S3 Bucket** – Shows the S3 bucket configured to store uploaded text files.

<img width="1398" height="474" alt="image" src="https://github.com/user-attachments/assets/bb17a999-9414-4ce1-8671-b433481138fa" />

* **Amazon SNS Topic** – Shows the SNS topic and confirmed email subscription.

<img width="874" height="593" alt="image" src="https://github.com/user-attachments/assets/05bdcdc1-7ad2-473d-82f4-2fe817b01b8b" />
<img width="441" height="102" alt="image" src="https://github.com/user-attachments/assets/55ffe429-4751-49ea-9f35-6755c603b124" />

* **AWS Lambda Function** – Shows the Lambda function configuration and deployed code.

<img width="1657" height="387" alt="image" src="https://github.com/user-attachments/assets/e091e4c5-792b-4720-930d-2cc3669cdce8" />

* **S3 Trigger Configuration** – Shows the S3 event trigger attached to the Lambda function.

<img width="1444" height="217" alt="image" src="https://github.com/user-attachments/assets/71fdc4a9-a58f-4dc0-9ee5-679ff70d3d6e" />

* **CloudWatch Logs** – Shows a successful function invocation and execution details.

* **Uploaded File** – Shows a sample text file uploaded to the S3 bucket.

<img width="1080" height="1227" alt="image" src="https://github.com/user-attachments/assets/a1257e53-6070-4cad-9813-a2aefbdc87de" />

* **Email Notification** – Shows the email received containing the word count result.

## Challenges and Troubleshooting

During development, the Lambda function initially failed to deliver notifications after being triggered. Reviewing the CloudWatch logs showed an authorization error when the function attempted to publish to the SNS topic. Investigation showed that the execution role was missing the required SNS publish permission. This was resolved by attaching the appropriate SNS permissions to the Lambda execution role, after which the function was able to publish successfully and the email notifications were delivered as expected.

This issue reinforced the importance of reviewing CloudWatch logs as a first step when a serverless function does not behave as expected, and highlighted how AWS IAM permissions directly control which actions a service can perform.

## Security Considerations

* **IAM roles and least privilege** – The Lambda execution role was scoped to only the permissions required for this project (S3 read access, SNS publish access, and CloudWatch Logs write access), rather than granting broader account-level access.
* **No hardcoded credentials** – The function relies on its IAM execution role for authentication rather than embedded access keys.
* **Private S3 bucket** – The S3 bucket used for uploads is not publicly accessible; Lambda accesses it directly through its execution role.

## What I Learned

Through this project, practical experience was developed with:

* Building event-driven, serverless architecture using AWS Lambda
* Connecting Amazon S3, AWS Lambda, and Amazon SNS into a single automated workflow
* Configuring IAM roles and permissions following least-privilege principles
* Writing Python functions using Boto3 to interact with AWS services
* Using Amazon CloudWatch to monitor and troubleshoot serverless applications
* Diagnosing and resolving permission-related errors in a live AWS environment

## Outcome

The project was successfully completed and tested. Uploading a text file to the S3 bucket automatically triggered the Lambda function, calculated the correct word count, and delivered an email notification through SNS, confirming that the end-to-end serverless workflow functioned as intended.

## Future Improvements

Possible future improvements could include:

* Adding Infrastructure as Code (e.g., AWS CloudFormation or Terraform) to deploy the solution repeatably
* Extending the Lambda function to support additional file types beyond plain text
* Adding CloudWatch Alarms to alert on function errors or failures
* Storing word count results in a DynamoDB table for historical tracking in addition to the email notification

---

**Note:** This repository does not contain any AWS account IDs, credentials, access keys, or lab-specific resource ARNs. Replace any placeholder values in the code with your own resource identifiers before deploying.
