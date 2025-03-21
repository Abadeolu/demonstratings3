🚀 Static Website Hosting with AWS S3 and GitHub CI/CD

View project on AWS: http://demonstratings3.s3-website.eu-west-2.amazonaws.com/

This project demonstrates how to host a static website using Amazon S3 and set up a Continuous Integration/Continuous Deployment (CI/CD) pipeline using GitHub Actions. Every time changes are pushed to the main branch, the website is automatically updated and deployed to the S3 bucket.

Features
Static website hosted on Amazon S3

Public access enabled via bucket policies

Automated deployment using GitHub Actions

Secure AWS access using IAM roles and access keys

Setup and Deployment
1. Create an S3 Bucket
Navigate to AWS S3 and create a new bucket.

Enable Static Website Hosting in the bucket properties.

Upload your static website files (HTML, CSS, JS).

2. Configure Bucket Policy
Modify the bucket policy to allow public access.

Example bucket policy:

json
Copy
Edit
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::your-bucket-name/*"
    }
  ]
}
3. Set Up CI/CD with GitHub Actions
Create an IAM user with AmazonS3FullAccess permissions.

Generate an access key and secret key.

Add the access keys to GitHub repository secrets (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY).

Create a .github/workflows/deploy.yml file to automate the deployment.

Example GitHub Actions Workflow (deploy.yml):
yaml
Copy
Edit
name: Deploy to S3

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v2

    - name: Deploy to S3
      run: |
        aws s3 sync . s3://your-bucket-name --delete
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        
