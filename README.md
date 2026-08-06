# AWS Serverless Audio Processing Pipeline

##  Project Overview

This project demonstrates a fully event-driven serverless architecture on AWS.

The application provides REST APIs using Amazon API Gateway to upload audio and text files.

- Audio files are automatically converted into text using Amazon Transcribe.
- Text files are automatically converted into speech using Amazon Polly.

The complete workflow is fully serverless using Amazon S3 Event Notifications, AWS Lambda, and managed AWS services.

## Architecture

![Architecture](architecture/architecture.png)

## 🔄 Project Workflow

### Audio Processing Pipeline

1. User uploads an MP3 file using the REST API.
2. Amazon API Gateway uploads the file to Amazon S3.
3. Amazon S3 triggers an AWS Lambda function.
4. Lambda starts an Amazon Transcribe job.
5. Amazon Transcribe generates a transcript (.json) in the output S3 bucket.

### Text-to-Speech Pipeline

1. User sends text through the REST API.
2. Amazon API Gateway uploads the text to Amazon S3.
3. Amazon S3 triggers an AWS Lambda function.
4. Lambda invokes Amazon Polly.
5. Amazon Polly generates an MP3 file and stores it in the output S3 bucket.

##  AWS Services Used

- Amazon API Gateway
- Amazon S3
- AWS Lambda
- Amazon Transcribe
- Amazon Polly
- AWS IAM
- Amazon CloudWatch

---

##  Features

- REST API using API Gateway
- Direct Amazon S3 Integration
- Event-Driven Architecture
- Speech-to-Text using Amazon Transcribe
- Text-to-Speech using Amazon Polly
- AWS Lambda Automation
- API Key Authentication
- CloudWatch Logging

## 🔐 API Authentication

This API is protected using an Amazon API Gateway API Key.

Clients must include a valid API key in the request header.

### Header

```http
x-api-key: YOUR_API_KEY
```

Requests without a valid API key will be rejected with an HTTP 403 Forbidden response.


📤 Example Request

For example, to upload an audio file:

### Upload Audio

```http
PUT /upload/{bucketname}/{filename}
```

Headers

```http
x-api-key: YOUR_API_KEY
Content-Type: audio/mpeg
```

### Upload Text

```http
POST /text-to-speech/{bucketname}/{filename}
```

Headers

```http
x-api-key: YOUR_API_KEY
Content-Type: application/json
```

Body

```json
{
  "text": "Hello from AWS Serverless!"
}
```
# IAM Roles Used

This project uses AWS IAM Roles to securely grant permissions to AWS services.

## API Gateway Execution Role

Permissions:

- Amazon S3 PutObject

Purpose:

- Upload audio files to Amazon S3
- Upload text files to Amazon S3

---

## Lambda (Amazon Transcribe)

Permissions:

- Amazon Transcribe
- Amazon S3
- CloudWatch Logs

Purpose:

- Trigger Amazon Transcribe when an audio file is uploaded to Amazon S3.
- Store the generated transcript in an output S3 bucket.

---

## Lambda (Amazon Polly)

Permissions:

- Amazon Polly
- Amazon S3
- CloudWatch Logs

Purpose:

- Generate MP3 audio from uploaded text.
- Save the generated audio file to Amazon S3.
