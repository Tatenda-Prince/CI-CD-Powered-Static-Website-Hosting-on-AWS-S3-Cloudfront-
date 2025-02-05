# Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront


"A Pipeline To The Cloud"

# Technical Architecture

![image alt] 

# Intro 

Today we will take this further to incorporate DevOps practices as I show you how we can incorporate a CI/CD pipeline to automate the deployment of our Resume Website whenever we make changes to the code.

We will be utilizing AWS CodePipeline, GitHub and Amazon S3! This is going to be a fun one so let’s get to it!

# Background

## Amazon S3

Amazon Simple Storage Service, is a highly scalable and popular cloud storage solution offered by Amazon Web Services (AWS), widely used for storing and retrieving data on the internet.

## Continuous Integration (CI)

Continuous Integration is a practice that involves developers making small changes and checks to their code which helps streamline code changes. When practicing CI, new code changes to an app are regularly built, tested, and merged to a shared repository. It provides a solution to the problem of having too many branches of an app in development at once that might conflict with each other, thereby increasing time for developers to make changes and contribute to improved software.

## Continuous Deployment/Delivery (CD)

Continuous Delivery is the automated delivery of completed code to environments for example, testing or development. CD provides an automated and consistent way for code to be delivered to these environments.

Continuous Deployment is the next step of Continuous Delivery. Every change from a repository that passes the automated tests is automatically placed in production, where it is usable by customers. It improved the issues of overloading operations teams with manual processes that slow down app delivery to customers.


## AWS CodePipeline (CodePipeline)

CodePipeline is a fully managed continuous delivery service on AWS that you can use to automate CI/CD pipelines for fast and reliable application and infrastructure updates.

## GitHub

GitHub, as the name suggest, is a Hub, a central site that offers developers a cloud-based service to store and manage code as well as track and control changes to code. GitHub makes it possible to leverage Git in the cloud at a central site.

# Prerequisites

1.AWS account and IAM user (Free Tier)

2.Custom Website files (HTML, CSS)

3.GitHub Account

# Objectives

1.Create a GitHub repository.

2.Create a S3 bucket.

3.Create a CI/CD Pipeline.

4.Deploy the Pipeline and verify that you can reach your static website. Make an update to the code in GitHub and verify the change in your static website.

5.Add CloudFront as a CDN for your static website

# Use Case

You work as a Developer at Chelsea Football Club! Now you want to update the official club Website to add the new features for everyone else to see. Currently when you make updates, you manually edit your  Website files on your local system, delete the old file versions from S3, then upload the updated changes to the S3 bucket. You’ve now decided to move towards manually automate the deployment of the changes to the website by creating a CI/CD pipeline using AWS CodePipeline and GitHub and Amazon S3.

# Step 1: Create a new repository in GitHub with Resume Website files.

## Head to your GitHub account and create a new repository by clicking “New”.

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

1. Go to S3 Create bucket and name your bucket.
   

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/6136cf07df4e355b023ef7a0f786141f95d9a0b0/Images/Screenshot%202025-01-03%20122211.png)
   

2. Uncheck Block all public access and acknowledge. Keep the default settings and click Create button.
   

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/a06e94ffb5d6c56850ac1aa43fd498e1692e5906/Images/Screenshot%202025-01-03%20122237.png)
   

3. Go to Properties tab and scroll all the way to the bottom to Edit static website hosting section. Enable Static website hosting and choose Host a static website as Hosting type. Enter the Index document name and then click Save changes button.
   

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/fa0c28883a5c3601349653f51f47e5482b8be161/Images/Screenshot%202025-01-03%20122413.png)


4. Go to Permissions tab>Edit bucket policy. Paste below bucket policy and then click Save changes button. Make sure you update the “Resource”:”arn: your arn/*” with your s3 bucket arn and add /* so that you add all files within your bucket.
   

 ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/326125a52136b8c8231f0c2c35cde72f7ee6566d/Images/Screenshot%202025-01-03%20122659.png)


# Step 3: Connect GitHub Account to CodePipeline

Navigate to the CodePipeline service and “Create pipeline”.

Name your pipeline and select “New service role”. Leave the rest of the settings at default, then click “Next”.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/c19778a0626b967c5c0d6b136c4452eac0b60ada/Images/Screenshot%202025-01-03%20123237.png)


For the source stage, select “GitHub (Version 2)” as our source provider.

We can now connect our to our GitHub by selecting “Connect to GitHub”. Name the connection, then “Connect to GitHub”. You’ll need to sign in to your GitHub account to authorize the connection.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/2eef690b3899cda234bdebf25970271bda065b41/Images/Screenshot%202025-01-03%20123709.png)


Now that you have are connected to your GitHub, we can continue to Step 4: Configuring CodePipeline.


# Step 4: Configure CodePipeline and deploy CI/CD pipeline

Proceed by adding the “Repository name” and the “Branch name”. All other settings can remain at default, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/a645755aafc2a6057f2a7dacf05f810ea3562394/Images/Screenshot%202025-01-03%20123832.png)


We will skip the build stage by clicking “Skip build stage”.

For the “Add deploy stage”, select “S3” for the deploy provider, then the region your S3 bucket was created in, then your Resume Website bucket. Ensure to select “Extract file before deploy”, then click “Next”.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/26f032abe65ba0fcd9dda23074546e7e2da26ddd/Images/Screenshot%202025-01-03%20124200.png)

Review over the pipeline, then proceed to “Create pipeline”. Wait for the pipeline to be created.


## Success!

If everything was done correctly, you should see a success message, as shown below.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/7a70f59ba5e9c03fe7c80acf2a8a19b0ea5c2780/Images/Screenshot%202025-01-03%20124314.png)


# Congratulations!

You have just created a CI/CD pipeline for your  Website, using AWS CodePipeline, GitHub and Amazon S3!

If you head to your S3 bucket that host’s your Resume Website files, you should see the newly updated files, including the “READMe.md” file. This verifies that our CI/CD pipeline was able to make updates aligning with our GitHub repo.

We can now proceed to the final Step: Verifying our CI/CD pipeline!

# Step 5: Verify functionality of CI/CD Pipeline

Before we can fully verify the functionality of the CI/CD pipeline, we first need to verify that we can view our Custom Website from our custom s3 endpoint domain in our browser.


1.Go to S3Your bucket and confirm if you have README.md and storage.html file.


![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/154dbf335f32138055654aaf498c777b4c7bd76a/Images/Screenshot%202025-01-03%20124546.png)
  
  

2. Click Properties and scroll to the bottom and copy the Bucket website endpoint address and paste in in the web browser.
   

 ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/7dafd4cb2ebf3063b23bfc34f25842f77287dcc6/Images/Screenshot%202025-01-03%20124620.png)
  


  ## Our Custom Website should be displayed, as seen below, showing my sample website
  

  ![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/3dd8d485397f7a16cd47abcc9d0f516972bdf897/Images/Screenshot%202025-01-03%20124840.png) 

  Great! We are able to view our Custom Website!
  
  
Now, we will make an update to the code in GitHub to verify that the pipeline is triggered and the CI/CD pipeline is functional. This will be as easy as simply making a change to the “README.md” file, since any change to the files should trigger the workflow.


3.Go to Github repository and edit the file. I added "WE ARE THE BEST CLUB IN LONDON"

Click commit changes.

![images alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/a582bf0efdff9c931f673836510502b267b22e06/Images/Screenshot%202025-01-03%20125418.png)



Now refresh the webpage. Yes, the changes have been made. This is the power of CodePipeline!


![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/dd6d87e605f12193dd52a014f5900316148ceb55/Images/Screenshot%202025-01-03%20125528.png)

After making changes, we can head back to CodePipeline. If you select your pipeline, you should notice that the pipeline has been triggered just recently.

Also, if we click “History” on the left pane of the CodePipeline dashboard, we can view the “Executing history” and see that it been triggered twice. The initial, when we set up the pipeline and the second time, when we made changed to the “README.md” file.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/b09bc06173b6226a7741ca13bf791a3408e6a001/Images/Screenshot%202025-01-03%20125644.png)


# Bonus

## Add CloudFront as a CDN for your static website.

1.Go to CloudFront>Create distribution>Choose the Amazon S3 bucket you just created


![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/927b509a5b6926a27a07dd0f0749564202a144fc/Images/Screenshot%202025-01-03%20132647.png)



2. Keep the default settings and click Create Distribution.

3. Open the CloudFront Distribution you just created and click General tab and copy the Distribution domain name.

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/012291d73bfddee1f34bfa65e467188844b9a6bd/Images/Screenshot%202025-01-03%20132736.png)


4.Paste the address and add your file name as shown below.

```link
https://d2vt2o429stw25.cloudfront.net/
```

![image alt](https://github.com/Tatenda-Prince/Hosting-A-Static-Website-With-CI-CD-Pipeline-GitHub-And-S3-Cloudfront/blob/568e32d3f4003004801d1287eceb01614ea9269a/Images/Screenshot%202025-01-03%20132754.png)


# Success!

If everything was done correctly, you’ve just successfully created a CI/CD pipeline to automate the deployment of a Custom Website using AWS CodePipeline, GitHub and Amazon S3!









  
 











   








