# Phishing Website Detection by Machine Learning Techniques

## Objective
A phishing website is a common social engineering method that mimics trustful uniform resource locators (URLs) and webpages. The objective of this project is to train machine learning models and deep neural nets on the dataset created to predict phishing websites. Both phishing and benign URLs of websites are gathered to form a dataset and from them required URL and website content-based features are extracted. The performance level of each model is measures and compared.

## Data Collection
The set of phishing URLs are collected from opensource service called **PhishTank**. This service provide a set of phishing URLs in multiple formats like csv, json etc. that gets updated hourly. To download the data: https://www.phishtank.com/developer_info.php. From this dataset, 5000 random phishing URLs are collected to train the ML models.

The legitimate URLs are obatined from the open datasets of the University of New Brunswick, https://www.unb.ca/cic/datasets/url-2016.html. This dataset has a collection of benign, spam, phishing, malware & defacement URLs. Out of all these types, the benign url dataset is considered for this project. From this dataset, 5000 random legitimate URLs are collected to train the ML models.


## Feature Extraction
The below mentioned category of features are extracted from the URL data:

1.   Address Bar based Features <br>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In this category 9 features are extracted.
2.   Domain based Features<br>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In this category 4 features are extracted.
3.   HTML & Javascript based Features<br>
          &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In this category 4 features are extracted.

## Models & Training

Before stating the ML model training, the data is split into 80-20 i.e., 8000 training samples & 2000 testing samples. From the dataset, it is clear that this is a supervised machine learning task. There are two major types of supervised machine learning problems, called classification and regression.

This data set comes under classification problem, as the input URL is classified as phishing (1) or legitimate (0). The supervised machine learning models (classification) considered to train the dataset in this project are:

* Decision Tree
* Random Forest
* Multilayer Perceptrons
* XGBoost
* Autoencoder Neural Network
* Support Vector Machines



# Deployment Guide

## 1. Login to AWS Console

Start by logging into the AWS Management Console.

## 2. Create IAM User for Deployment

Create an IAM user with specific access permissions:

- **EC2 Access**: Allows access to virtual machines.
- **S3 Bucket**: Allows storing artifacts and models.
- **ECR (Elastic Container Registry)**: Allows saving Docker images in AWS.

### IAM Policy Attachments

Attach the following policies to the IAM user:

1. `AmazonEC2ContainerRegistryFullAccess`
2. `AmazonEC2FullAccess`
3. `AmazonS3FullAccess`

## 3. Create an S3 Bucket

- **Region**: `ap-south-1`
- **Bucket Name**: `scania-sensor-pipeline`

## 4. ECR Repository

Create an ECR repository to store/save Docker images:

- **ECR Repository URI**: `566373416292.dkr.ecr.ap-south-1.amazonaws.com/sensor-fault`

## 5. EC2 Machine Setup

Create an EC2 machine with Ubuntu.

## 6. Install Docker on EC2 Machine

Open your EC2 instance and install Docker:

### Optional

```bash
sudo apt-get update -y
sudo apt-get upgrade
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
```

Configure EC2 as Self-Hosted Runner

Configure EC2 as a self-hosted runner for GitHub Actions:

Go to Settings > Actions > Runners.
Click on New self-hosted runner.
Choose the operating system.
Follow the provided commands to complete the setup.

Setup GitHub Secrets

Add the following secrets to your GitHub repository:

Setup GitHub Secrets

AWS_ACCESS_KEY_ID: Your AWS access key.
AWS_SECRET_ACCESS_KEY: Your AWS secret access key.
AWS_REGION: ap-south-1
AWS_ECR_LOGIN_URI: 566373416292.dkr.ecr.ap-south-1.amazonaws.com
ECR_REPOSITORY_NAME: sensor-fault
MONGO_DB_URL: mongodb+srv://aravind:aravind@cluster0.or68e.mongodb.net/admin?authSource=admin&replicaSet=atlas-desfdx-shard-0&w=majority&readPreference=primary&appname=MongoDB%20Compass&retryWrites=true&ssl=true

Deployment Description
Follow these steps for deployment:

Build Docker Image: Build a Docker image of your source code.
Push Docker Image to ECR: Push the Docker image to the ECR repository.
Launch EC2 Instance: Start your EC2 instance.
Pull Docker Image on EC2: Pull the Docker image from ECR to your EC2 instance.
Launch Docker Image on EC2: Run your Docker image on the EC2 instance.
