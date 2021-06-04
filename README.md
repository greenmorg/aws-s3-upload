# S3 presigned URLs with SAM, auth and sample frontend

This example application shows how to upload objects to S3 directly from your end-user application using Signed URLs.

To learn more about how this application works, see the AWS Compute Blog post: https://aws.amazon.com/blogs/compute/uploading-to-amazon-s3-directly-from-a-web-or-mobile-application/


```bash
.
├── README.MD                   <-- This instructions file
├── frontend                    <-- Simple JavaScript application illustrating upload
├── getSignedURL                <-- Source code for the serverless backend
```

## Requirements

* AWS CLI already configured with Administrator permission
* [AWS SAM CLI installed](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) - minimum version 0.48.
* [NodeJS 12.x installed](https://nodejs.org/en/download/)

### Installing the application
Configure AWS CLI with access key and secret key: https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-config
```
cd .. 
sam deploy --guided
```
When prompted for parameters, enter:
- Stack Name: s3Uploader
- AWS Region: your preferred AWS Region (e.g. us-east-1)
- Accept all other defaults.

This takes several minutes to deploy. At the end of the deployment, note the output values, as you need these later.

- The APIendpoint value is important - it looks like https://ab123345677.execute-api.us-west-2.amazonaws.com.
- **The upload URL is your endpoint** with the /uploads route added - for example: https://ab123345677.execute-api.us-west-2.amazonaws.com/uploads.

### Configure the API Key
- Open the AWS Console and go to API Gateway Service
- Open API for this application
- Go to Usage Plans
- Create new Usage Plan:
    - Enter Name, Disable Throttling and Quota, Press "Next"
    - Add Stage: Choose API and a Stage, Press  "v" button, Press Next
    - Press "Create an API Key and Add to usage Plan": Enter Name, Press Save, Press Done
- Go to API Keys
- Find new key, open it and press Show button, save the key for later use

### Deploy Frontend
- Open frontend/index.html
- Replace "PLACE API ENDPOINT URI HERE" with API Endpoint on the line "const API_ENDPOINT = 'PLACE API ENDPOINT URI HERE'", save the file
- Open the AWS Console and go to S3 Service
- Create New Bucket for the Frontend: Enter name, Disable "Block all public access" 
- Upload frontend/index.html file to the root of the bucket
- Click on the index.html in the bucket and enable public access

### Testing with the frontend application

- Open the index.html URI in browser, add "?key=API_KEY_VALUE" to the URI and refresh the page
- Use web form to upload a file to S3
- Check that file was uploaded

