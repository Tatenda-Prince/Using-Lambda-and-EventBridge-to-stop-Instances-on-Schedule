# Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule
"Functions of Lambda"

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/8cd1da20ea65fe94545f9021fcb6ef667f907f66/Images/Screenshot%202024-12-24%20124719.png)

# Intro

Boto3 is a Python library for interacting with Amazon Web Services that can be used to improve your skills in automation processes to increase efficiency.

Boto3 is a game changer, allowing you to automate tasks, build custom scripts and manage AWS resources from the command line. Its comprehensive API support for various AWS services has made it easy to integrate Python scripts with AWS.

Today we will create a Lambda function that uses the Boto3 library to automate the task of stopping specific EC2 Instances. The Lambda function would be triggered by a Amazon EventBridge Schedule, which will schedule the function to run at a specific time each day

# Background

# Boto3

Boto3 provides a Python API for AWS infrastructure services. Using the SDK for Python, you can directly create, update and delete AWS resources using Python scripts.

# AWS Lambda

Lambda is a serverless compute service that runs your code as functions in response to events and automatically manages the underlying compute resources for you.

# AWS EventBridge

EventBridge is also a serverlesss event bus service that makes it easy to connect your applications with data from a variety of sources. It can be used to build an application that reacts to events from SaaS applications, AWS services or Custom applications.

# Prerequisites

WS Account with an IAM User

Basic knowledge of the Python Programming Language

Basic knowledge and use of an Interactive Development Environment

# Use Case

Your DevOps engineering team at Up The Chels Tech often uses a development lab to test releases of our application. Your Managers are complaining about the rising cost of the development lab and need to save money by stopping the (for this example) 3 EC2 Instances after all engineers have clocked out.

You need to ensure that only the Development Instances are stopped and make sure nothing in Production is interfered with. We only need to stop running Instances that have the “Environment: Dev” tag.

You’ve decided to use a Lambda function using the Python runtime with EventBridge to schedule the function to run after 7pm daily as no one should be working in the Dev environment past 7pm.

# Step 1: Set up Lambda Function and IAM Role

Navigate to AWS Lambda in the AWS Management Console and click “Create Function”.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/45c79ef783332d50e84af42cf5988d761f65d2fe/Images/Screenshot%202024-12-24%20130130.png)

Select “Author from scratch”, name the function, then choose Python 3.7 or greater Runtime.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/41de6dae535d82886821b57dcb50aa253aec07a9/Images/Screenshot%202024-12-24%20130217.png)

You will then change the default execution role and use an exiting role. To create this role select “IAM Console”.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/5768c5b73a0f09ce3d7c3e291d9ae2cecb43bb97/Images/Screenshot%202024-12-26%20123016.png)

A new window should open up. In that new window, select “AWS service”, select “Lambda”, then click “Next:Permissions” at the bottom right.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/54aa2398ef40049450e65297ab95d68d3e4084b8/Images/Screenshot%202024-12-26%20123326.png)


Click “Create policy”, then select the “JSON” table to edit the policy. Copy and paste the JSON policy below in the policy box, then click “Next:Tags”.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/a1ff3f6f27952021f8d5c2625c5d359322e77183/Images/Screenshot%202024-12-24%20130533.png)


Continue by clicking “Next: Review”. Name the policy, then click “Create policy”.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/9fd4f098cf33036ccd87c07857e74e6c6555f450/Images/Screenshot%202024-12-24%20130736.png)

Head back to create the role. Make sure to refresh the policies to include the new policy just created. Now search for the policy, select it, then click “Next:Tags”.

Continue to “Review”. Name and describe the role, then click “Create role”.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/45d55ebb85c4c75d8d8cb12fc06b9332227ac3bd/Images/Screenshot%202024-12-24%20131007.png)


Head back to the Lambda’s “Create function” window. Refresh the existing roles, select the role previously created, then click “Create Function”.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/9d40d557771cdcf21c9c016f839e3dc855e8bb5a/Images/Screenshot%202024-12-24%20131101.png)


# Step 2: Deploy and Test Lambda Function

In the overview window of your Lambda Function, below is the code we are going to use for the function’s code. You can also view it or clone it from my GitHub repo.

This code uses the “boto3” Python library to interact with AWS services. In the “lambda_handler” function, we loop through all Instances to get their current state and tags.

For each Instance tag, we loop through to find specific Instances that have the “Dev” tag and state is currently “running”. If both conditions are satisfied, we proceed to stop that Instance.

Lastly, the function returns “success” for us to know it ran successfully.

Proceed by copying the code above and pasting it your Lambda Function’s code tab, as seen below —


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/00efa17f19ac5240b048a65e062563e626f7eafa/Images/Screenshot%202024-12-24%20131600.png)

Next, we will click “Deploy” to deploy the function’s code to the Lambda service, then click “Test” to test out the function based on a test case.

For “Test event action”, select “Create a new event”, then name the event. We can use the JSON code below to test our Lambda function.

Click “Save” to save the Test event.

{ 
  "key1": "value1",
  "key2": "value2",
  "key3": "value3",
}


![image alt]()

















