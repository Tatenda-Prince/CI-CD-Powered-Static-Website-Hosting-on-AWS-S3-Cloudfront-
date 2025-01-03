# Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront
"A Pipeline To The Cloud"

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/6e9a8346108dca161a5456b5d3fbb7018e20b340/Images/Screenshot%202025-01-03%20120312.png) 

# Intro 

Today we will take this further to incorporate DevOps practices as I show you how we can incorporate a CI/CD pipeline to automate the deployment of our Resume Website whenever we make changes to the code.

We will be utilizing AWS CodePipeline, GitHub and Amazon S3! This is going to be a fun one so let’s get to it!

# Background

# Amazon S3

Amazon Simple Storage Service, is a highly scalable and popular cloud storage solution offered by Amazon Web Services (AWS), widely used for storing and retrieving data on the internet.

# Continuous Integration (CI)

Continuous Integration is a practice that involves developers making small changes and checks to their code which helps streamline code changes. When practicing CI, new code changes to an app are regularly built, tested, and merged to a shared repository. It provides a solution to the problem of having too many branches of an app in development at once that might conflict with each other, thereby increasing time for developers to make changes and contribute to improved software.

# Continuous Deployment/Delivery (CD)

Continuous Delivery is the automated delivery of completed code to environments for example, testing or development. CD provides an automated and consistent way for code to be delivered to these environments.

Continuous Deployment is the next step of Continuous Delivery. Every change from a repository that passes the automated tests is automatically placed in production, where it is usable by customers. It improved the issues of overloading operations teams with manual processes that slow down app delivery to customers.


# AWS CodePipeline (CodePipeline)

CodePipeline is a fully managed continuous delivery service on AWS that you can use to automate CI/CD pipelines for fast and reliable application and infrastructure updates.

# GitHub

GitHub, as the name suggest, is a Hub, a central site that offers developers a cloud-based service to store and manage code as well as track and control changes to code. GitHub makes it possible to leverage Git in the cloud at a central site.

# Prerequisites

AWS account and IAM user (Free Tier)

Custom Website files (HTML, CSS)

GitHub Account

# Objectives

1.Create a GitHub repository.

2.Create a S3 bucket.

3.Create a CI/CD Pipeline.

4.Deploy the Pipeline and verify that you can reach your static website. Make an update to the code in GitHub and verify the change in your static website.

5.Add CloudFront as a CDN for your static website

# Use Case

You work as a Developer at Chelsea Football Club! Now you want to update the official club Website to add the new features for everyone else to see. Currently when you make updates, you manually edit your  Website files on your local system, delete the old file versions from S3, then upload the updated changes to the S3 bucket. You’ve now decided to move towards manually automate the deployment of the changes to the website by creating a CI/CD pipeline using AWS CodePipeline and GitHub and Amazon S3.

# Step 1: Create a new repository in GitHub with Resume Website files.

Head to your GitHub account and create a new repository by clicking “New”.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/631b94d444bccd7af07500d8b0823eb3b839c038/Images/Screenshot%202025-01-03%20120627.png)

Give the repository a name and description. Set it to “Public”, select “Add a README file”, then click “Create repository”.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/bc0546d56cea475859d8bcddd314f79d90d66ef1/Images/Screenshot%202025-01-03%20135628.png)

Navigate to “Add file”, then upload your Custom Website files.

Note — It’s good practice to create a new branch when making changes to a repo e.g. commits, pull requests and merges, however for demonstrative purposes, we’ll work on the main branch.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/72b93beec24e8a6586ad45632c8163f4ef13f8da/Images/Screenshot%202025-01-03%20120950.png)

After choosing your files, add a commit description, then click “Commit changes”.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/3a582be775dd1ae01efd60feb56ddf2d3794f4c4/Images/Screenshot%202025-01-03%20121357.png)

You should now be able to see listed in your repo, your uploaded Resume Website files, as seen below. Now, let’s proceed to Step 2: Emptying your previous S3 bucket!

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/3d24857c199df4758f8cec4aec7366617825e5a9/Images/Screenshot%202025-01-03%20121422.png)



# Step 2: Create and Configure an S3 Bucket

1. Go to S3>Create bucket and name your bucket.
   

   ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/6136cf07df4e355b023ef7a0f786141f95d9a0b0/Images/Screenshot%202025-01-03%20122211.png)
   

3. Uncheck Block all public access and acknowledge. Keep the default settings and click Create button.
   

   ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/a06e94ffb5d6c56850ac1aa43fd498e1692e5906/Images/Screenshot%202025-01-03%20122237.png)
   

5. Go to Properties tab and scroll all the way to the bottom to Edit static website hosting section. Enable Static website hosting and choose Host a static website as Hosting type. Enter the Index document name and then click Save changes button.
   

   ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/fa0c28883a5c3601349653f51f47e5482b8be161/Images/Screenshot%202025-01-03%20122413.png)


7. Go to Permissions tab>Edit bucket policy. Paste below bucket policy and then click Save changes button. Make sure you update the “Resource”:”arn: your arn/*” with your s3 bucket arn and add /* so that you add all files within your bucket.
   

   ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/326125a52136b8c8231f0c2c35cde72f7ee6566d/Images/Screenshot%202025-01-03%20122659.png)


# Step 3: Connect GitHub Account to CodePipeline

Navigate to the CodePipeline service and “Create pipeline”.

Name your pipeline and select “New service role”. Leave the rest of the settings at default, then click “Next”.

![image alt]()


For the source stage, select “GitHub (Version 2)” as our source provider.

We can now connect our to our GitHub by selecting “Connect to GitHub”. Name the connection, then “Connect to GitHub”. You’ll need to sign in to your GitHub account to authorize the connection.

![image alt]()


Now that you have are connected to your GitHub, we can continue to Step 4: Configuring CodePipeline.


# Step 4: Configure CodePipeline and deploy CI/CD pipeline





   








