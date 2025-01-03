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





