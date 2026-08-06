# AWS Serverless Audio Processing Pipeline Architecture

## High Level Architecture

```text
                      User
                        │
        ┌───────────────┴────────────────┐
        │                                │
        ▼                                ▼
 PUT /upload                    POST /text-to-speech
        │                                │
        └───────────────┬────────────────┘
                        │
                Amazon API Gateway
                        │
                        ▼
                  Amazon S3 Bucket
                ┌────────┴────────┐
                │                 │
                ▼                 ▼
         Audio Uploaded     Text Uploaded
                │                 │
          S3 Event Trigger   S3 Event Trigger
                │                 │
                ▼                 ▼
      Lambda (Transcribe)  Lambda (Polly)
                │                 │
                ▼                 ▼
      Amazon Transcribe    Amazon Polly
                │                 │
                ▼                 ▼
      Transcript (.json)    Audio (.mp3)
                │                 │
                └────────┬────────┘
                         ▼
                    Output Bucket
```

## Workflow

### Audio Pipeline

User uploads an MP3 file using API Gateway.

↓

The file is stored in Amazon S3.

↓

S3 Event triggers AWS Lambda.

↓

Lambda starts Amazon Transcribe.

↓

Amazon Transcribe generates a JSON transcript.

---

### Text-to-Speech Pipeline

User uploads text through API Gateway.

↓

The text file is stored in Amazon S3.

↓

S3 Event triggers AWS Lambda.

↓

Lambda invokes Amazon Polly.

↓

Amazon Polly generates an MP3 file.

↓

The generated audio is stored back in Amazon S3.
