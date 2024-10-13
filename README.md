
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
    [Add screenshot of CodeCommit repository creation page]
    
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

4. **Clone the Repository:**
    - Open CloudShell in the AWS Console.
    - Use the following command to clone the repository, replacing `<repository_url>` with the HTTPS URL of your CodeCommit repository:
        
        ```
        git clone <repository_url>
        
        ```
        
    - Enter your generated credentials (Git username and password) when prompted.
    
    **Screenshot: Cloning the Repository in CloudShell**
    [Add screenshot of CloudShell cloning the repository]

5. **Copy Code from S3:**
    - Navigate into the cloned repository directory in CloudShell:
        
        ```
        cd <your_directory_name>
        
        ```
        
    - Execute the following command to copy the code from the AWS-provided S3 bucket. Remember to change the region in the URL if you are not using US West (Oregon):
        
        ```
        aws s3 cp s3://ttt-wildrydes/wildrydes-site ./ --recursive
        ```
        
    
    **Screenshot: Copying Code from S3 Bucket**
    [Add screenshot of CloudShell copying code from S3]

6. **Commit and Push the Code:**
    - Add all the copied files to the Git staging area:
        
        ```
        git add .
        
        ```
        
    - Commit the changes:
        
        ```
        git commit -m "Initial commit"
        
        ```
        
    - Provide your Git username and email address when prompted (this is the IAM user, not the Git credentials).
    - Push the changes to the CodeCommit repository:
        
        ```
        git push
        
        ```
        
    - Enter your Git credentials again when prompted.
        - Note if you get an error while using the git push, try this :
            
            ```
            git remote set-url origin <repository_url>
            ```
            
            ```
            #Verify that the remote URL has been updated:
            git remote -v
            
            #The output should have your repository_url as orgin
            ```
### 2. Hosting the Website with AWS Amplify

1. **Create a New Amplify App:**
    - Go to the AWS Amplify service in the Console.
    - Click "Deploy an app”.
    - Select "CodeCommit" as the source and choose your repository and branch.
        - screenshot
    - Click "Next" and name your app (e.g., `demoweb`).
    - **Enable "Allow Amplify to automatically deploy files" for continuous deployment**.
    - Create a new service role and click "Next".
    - Review the settings and click "Save and deploy".
    
    **Screenshot: Creating Amplify App**
    [Add screenshot of creating an Amplify app connected to CodeCommit]

2. **Wait for Deployment:**
    - Amplify will provision the necessary infrastructure, build the application, and deploy it.
    - Once the deployment is successful, you will see green checkmarks in the Amplify console.
        
    
    **Screenshot: Amplify Deployment Success**
    [Add screenshot of Amplify console showing successful deployment]

3. **Test the Deployed Website:**
    - Open the website using the provided link in the Amplify console. You should see the basic Wild Rides website.
    
    **Screenshot: Basic Wild Rides Website**
    [Add screenshot of the initial Wild Rides website]
    
4. **Verify Continuous Deployment:**
    - Make a small change to the `index.html` file in your CodeCommit repository (e.g., modify some text).
    - Commit and push the change.
    - Amplify will automatically detect the change and redeploy the application.
    - Refresh the website in your browser to see the updated content.

### 3. User Authentication with Cognito

1. **Create a Cognito User Pool:**
    - Open the Cognito service in the AWS Console.
    - Click "Create a user pool".
    - Select "Cognito User Pool" and click "Next".
    - Choose "Username" as the sign-in attribute and proceed with default settings.
    - Configure password policy and multi-factor authentication as needed (for this project, MFA is disabled).
    - Enable self-registration.
    - Name the user pool `demoweb` (or similar) and create an app client named `demoweb-app`.
    - Review the settings and create the user pool.
    
    **Screenshot: Create Cognito User Pool**
    [Add screenshot of the Cognito user pool creation page]

2. **Note User Pool and Client IDs:**
    - Copy the **User Pool ID** from the "General information" section.
    - Then navigate to "App integration".
    - Copy the **App client ID** from the "App clients and analytics" section.
    
    **Screenshot: Cognito User Pool and Client IDs**
    [Add screenshot showing the User Pool ID and App client ID in Cognito]

3. **Update Application Configuration:**
    - Go back to your CodeCommit repository and open the `config.js` file in the `js` folder.
    - Replace the placeholder values for `userPoolId` and `userPoolClientId` with the IDs you copied.
    - Update the `region` to match your AWS region.
    - **Save and commit the changes**.
    
    **Screenshot: Updating config.js**
    [Add screenshot of updating userPoolId, userPoolClientId, and region in config.js]

4. **Test Registration and Login:**
    - Once Amplify has redeployed the application, refresh the Wild Rides website.
    - Click the "Giddy Up" button. You should see the registration/login modal.
    - Register a new user. Cognito will send a verification email.
    - Enter the verification code and complete the registration.
    - Log in with the newly created user credentials.
    -

### 4. Ride Sharing Functionality

### 4.1 Setting up DynamoDB

1. **Create DynamoDB Table:**
    - Navigate to the DynamoDB service in the AWS Console.
    - Click "Create table".
    - Name the table `rides` (or similar).
    - Set the partition key to `rideId` with the data type "String".
    - Create the table.
    
    **Screenshot: Creating DynamoDB Table**
    [Add screenshot of the DynamoDB table creation page]
    
2. **Note the Table ARN:**
    - Once the table is active, click into it.
    - Expand "Additional info" and copy the **Amazon Resource Name (ARN)**.
    
    **Screenshot: DynamoDB Table ARN**
    [Add screenshot showing the DynamoDB table ARN]
    

### 4.2 Creating the Lambda Function

1. **Create an IAM Role for Lambda:**
    - Go to the IAM service and select "Roles".
    - Click "Create role".
    - Choose "AWS service" as the trusted entity type and select "Lambda" as the service.
    - Click "Next" and search for the policy "AWSLambdaBasicExecutionRole".
    - Attach this policy and click "Next".
    - Name the role `wild-rides-lambda` (or similar) and create it.
    
    **Screenshot: Creating IAM Role for Lambda**
    [Add screenshot of the IAM role creation page for Lambda]
    
2. **Add DynamoDB Write Permissions to the Role:**
    - Open the newly created `wild-rides-lambda` role.
    - Go to "Add permissions" -> "Create inline policy".
    - Select "DynamoDB" as the service.
    - Choose the "PutItem" action to allow writing to the DynamoDB table.
    - Under "Resources", select "Specific" and click "Add ARN".
    - Paste the copied DynamoDB table ARN and click "Add ARNs".
    - Click "Next", name the policy "DynamoDBWriteAccess", and create it.
    
    **Screenshot: Adding DynamoDB Permissions to Role**
    [Add screenshots showing the process of adding DynamoDB write permissions to the Lambda role]
    
3. **Create the Lambda Function:**
    - Go to the Lambda service in the AWS Console.
    - Click "Create function" and choose "Author from scratch".
    - Name the function `request-unicorn` (or similar).
    - **Select the Node.js 16.x runtime (important!)**.
    - Choose "Use an existing role" for the execution role and select the `wild-rides-lambda` role.
    - Create the function.
    
    **Screenshot: Creating the Lambda Function**
    [Add screenshot of the Lambda function creation page]
    
4. **Update Lambda Function Code:**
    - Replace the default Lambda function code with the provided code from the YouTube video description (link in source).
    - **Make sure to update the table name in the `recordRide` function if you used a different name for your DynamoDB table**.
    
    **Screenshot: Lambda Function Code**
    [Add screenshot showing the updated Lambda function code]
    
5. **Deploy the Lambda Function:**
    - Click the "Deploy" button to save and deploy the changes.
    
    **Screenshot: Deploying the Lambda Function**
    [Add screenshot of the Lambda function deployment button]
    
6. **Test the Lambda Function:**
    - Click the arrow next to "Test" and choose "Configure test event".
    - Name the event "TestRequestEvent".
    - Paste the provided test event JSON code from the YouTube video description.
    - Click "Save" and then "Test".
    - You should see a successful execution with a status code of 201 and unicorn details in the output.
    - Verify that an item has been written to the DynamoDB table.
    
    **Screenshot: Testing the Lambda Function**
    [Add screenshots showing the test event configuration and successful Lambda function execution]
    

### 4.3 Setting up API Gateway

1. **Create a REST API:**
    - Go to the API Gateway service in the AWS Console.
    - Click "Create API" and choose "REST API".
    - Name the API `wild-rides` (or similar).
    - Select "Edge optimized" for the endpoint type.
    - Create the API.
    
    **Screenshot: Creating REST API**
    [Add screenshot of the API Gateway REST API creation page]
    
2. **Create a Cognito Authorizer:**
    - Go to "Authorizers" in the left-hand menu.
    - Click "Create authorizer".
    - Name it `wild-rides-authorizer`.
    - Choose "Cognito" as the type.
    - Select your AWS region and your Cognito user pool.
    - Set the "Token Source" to "Authorization".
    - Create the authorizer.
    
    **Screenshot: Creating Cognito Authorizer**
    [Add screenshot of the Cognito authorizer creation page in API Gateway]
    
3. **Test the Authorizer:**
    - Click into the newly created authorizer.
    - Paste the authorization token you copied earlier (from the `ride.html` page after logging in).
    - Click "Test". You should get a successful response with a status code of 200.
    
    **Screenshot: Testing the Authorizer**
    [Add screenshot of the authorizer test page showing a successful result]
    
4. **Create a Resource and Method:**
    - Go back to the "Resources" tab.
    - Create a new resource named `ride`.
    - **Enable CORS for the resource**.
    - With the `ride` resource selected, create a "POST" method.
    - Choose "Lambda Function" as the integration type and **enable "Lambda Proxy integration"**.
    - Select your `request-unicorn` Lambda function and create the method.
    
    **Screenshot: Creating Resource and Method**
    [Add screenshots showing the process of creating the resource and POST method in API Gateway]
    
5. **Configure Method Request Authorization:**
    - Select the "Method Request" for the POST method.
    - Under "Authorization", select your `wild-rides-authorizer` Cognito user pool.
    - Save the settings.
    
    **Screenshot: Configuring Authorization**
    [Add screenshot showing the authorization settings for the POST method]
    
6. **Deploy the API:**
    - Click the "Deploy API" button.
    - Create a new stage named `dev` (or similar) and deploy.
    
    **Screenshot: Deploying the API**
    [Add screenshot of the API Gateway deployment page]
    
7. **Note the Invoke URL:**
    - Copy the **Invoke URL** for the deployed API stage.
    
    **Screenshot: Invoke URL**
    [Add screenshot showing the invoke URL in API Gateway]
    

### 4.4 Final Code Updates

1. **Update `config.js`:**
    - In your CodeCommit repository, open the `config.js` file again.
    - Replace the placeholder value for `invokeUrl` with the copied API Gateway Invoke URL.
    - Commit the changes.
2. **Update `ride.html`:**
    - Open the `ride.html` file in your CodeCommit repository.
    - Update the ArcGIS version numbers from `4.3` to `4.6` in two places within the file.
    - Commit the changes.

### 5. Testing the Application

1. **Wait for Amplify Deployment:**
    - Allow Amplify to deploy the final code changes.
2. **Test the End-to-End Functionality:**
    - Go to the Wild Rides website and ensure you are logged in.
    - You should now see an interactive map.
    - Zoom in to a location and click on the map to request a unicorn ride.
    - You should see a unicorn animation traveling to your selected location.
    - Verify that a new item is added to the DynamoDB table with the ride details.

### 6. Cleaning Up Resources

**Important:**  After you are finished with the project, follow the instructions in source to delete all the AWS resources you created. This will prevent any unexpected charges.

## Conclusion

You have now successfully built the Wild Rides web application, demonstrating the power and flexibility of various AWS services. Feel free to explore the code further, experiment with different features, and adapt the project to your own ideas.

