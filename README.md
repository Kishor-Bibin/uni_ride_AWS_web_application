
# Wild Rides: An End-to-End AWS Web Application

This README provides a step-by-step guide to building the Wild Rides web application, a demonstration project using various AWS services. The application simulates a ride-sharing service, similar to Uber or Lyft, but for unicorns. Users can register, log in, and request unicorn rides using an interactive map.

## Prerequisites

Before you begin, ensure you have the following:

- **AWS Account:** You will need an active AWS account with access to the AWS Management Console.
- **Text Editor or Note-taking Tool:** This will be used for copying and pasting IDs, credentials, and other important information.
- **ArcGIS Account:** An ArcGIS account is required for the mapping functionality. Sign up for a free account at arcgis.com.
- Link : https://www.arcgis.com/home/index.html
![](https://github.com/Kishor-Bibin/uni_ride_AWS_web_application/blob/05e955a8187248c7b52b8c215b6da33486b33d18/Screenshots/Arcgis.png)

## Project Overview

The project uses several AWS services, all of which fall under the AWS Free Tier for new accounts. If you are outside the free tier period, the project should cost no more than a dollar. The following services are used:

- **CodeCommit:** AWS's source control system, similar to GitHub, for storing and managing the application code.
- **S3:** Used to store the initial application code provided by AWS.
- **IAM:** For managing user permissions and creating roles for services to interact with each other.
- **CloudShell:** A browser-based command-line interface within the AWS Console.
- **Amplify:** A service for building and hosting web applications. It simplifies the deployment process and offers features like continuous deployment.
- **Cognito:** Provides user authentication functionality, allowing users to register, log in, and manage their accounts.
- **Lambda:** Serverless compute service for running code in response to events. In this project, Lambda functions handle ride requests and interact with the database.
- **DynamoDB:** A NoSQL database used to store ride request data and unicorn details.
- **API Gateway:** Creates and manages APIs for invoking Lambda functions and other backend services.

## Step-by-Step Instructions

### 1. Setting up the Code Repository

1. **Create a CodeCommit Repository:**
    - Navigate to CodeCommit in the AWS Console.
    - Click "Create repository".
    - Name your repository `wild-rides-site` (or similar) and click "Create".
    
    **Screenshot: Create CodeCommit Repository**
    ![](https://github.com/Kishor-Bibin/uni_ride_AWS_web_application/blob/d3815351e1f14985f61f1c8f3e5240d92c2f93a4/Screenshots/AWS%20Codecommit%20Creation%20page%20.png)
    
2. **Configure IAM Permissions:**
    - Open a new tab in the AWS Console and go to IAM.
    - Navigate to "Users" and click on your IAM user.
    - On the "Permissions" tab, click "Add permissions" -> "Attach policies directly".
    - Search for "AWSCodeCommitPowerUser" and select the policy.
    - Click "Next" and then "Add permissions".
    
    **Screenshot: Attach IAM Policy**
    [Add screenshot of attaching AWSCodeCommitPowerUser policy]
    
3. **Generate Git Credentials:**
    - In your IAM user settings, go to "Security credentials".
    - Scroll down to "HTTPS Git credentials for AWS CodeCommit".
    - Click "Generate credentials" and **securely store the generated username and password**.

