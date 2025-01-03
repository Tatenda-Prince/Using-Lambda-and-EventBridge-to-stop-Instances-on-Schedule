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


Click “Create policy”, then select the “JSON” table to edit the policy. Copy and paste the JSON policy below in the policy box, then click “Next:Tags”. Check the JSON fILE IN GITHUB.

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


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/756d74ebf4f3ddf4934b66038dd396ae38f6a1db/Images/Screenshot%202024-12-24%20133952.png)


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


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/ea75e658754bce0fcde864d9746e9a32e9ec0bbc/Images/Screenshot%202024-12-24%20131650.png)


We can now test our function by clicking ‘Test”. A “success” response, along with other details from the function execution in the function logs should display in the “Executing results” tab.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/795ee064740e5ae1d1e82cf39b8d28a5dea861e8/Images/Screenshot%202024-12-26%20125511.png)

Now that we’ve set up and created our Lambda function and also tested its successful functionality, we can now proceed to Step 3 — Using EventBridge to schedule our function to run at a set time of the day and days of the week.

# Step 3: Create an EventBridge Schedule to schedule the Lambda function

Navigate to EventBridge, select “EventBridge Schedule” then click “Create Rule”.

Name and describe (optional) your schedule and set “Schedule group” to default.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/1cd68df05c816ddf21d12c5bee04405df949bbbc/Images/Screenshot%202024-12-24%20131943.png)

For “Schedule pattern” select “Recurring schedule” since we want the Lambda function to execute at 7pm every working day.

Select “Cron-based schedule” and use the cron expression as seen below, then click “Next”.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/42dca648f630a310ff2d99e70d54ae3ef2224e3f/Images/Screenshot%202024-12-24%20132118.png)


Select “AWS Lambda — Invoke”, choose your Lambda function, then click “Next”.


![imagea alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/8947ec13d1b1d8b88fbaeaf58ee484e5dc95a29b/Images/Screenshot%202024-12-24%20132241.png)

Continue to the “Review and create schedule”, then click “Create schedule”.

You should now be able to see the new Schedule just created in EventBridge.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/305ad20c1d73f7dd740deb8f0a6adcb256b51ad3/Images/Screenshot%202024-12-24%20132342.png)


Now that we’ve scheduled the execution of our Lambda function, we can proceed to Step 4 — Automating the launching of Dev EC2 Instances.

# Step 4: Automating Dev EC2 Instances launch

Before we can test our Lambda Function and EventBridge operational functionality, we can use the source code below to create a Python script to automate the launch of three new EC2 Instances with tags “Environment: Dev”.

For our test to be successful, the three Development Instances created should stop once the Lambda function runs according to the schedule set by EventBridge.

You can also view this code or clone the repo from my GitHub.


After creating the script and running it, you should be able to see three new Development EC2 Instances created in the EC2 dashboard. If you select either one of the EC2 Instances and click on the “Tags” tab, you should notice a key:value pair tag — “Environment: Dev”.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/96e81bc5eee2b89bc17d6d05c18c8b60ac925a04/Images/Screenshot%202024-12-24%20133901.png)


We can now proceed to Step 5 — Verifying the functionality of our Lambda function and EventBridge.


# Step 5: Verify Lambda Function and EventBridge functionality

Navigate back to EventBridge and edit the Schedule created earlier. We need to make changes to the next scheduled time so we don’t have to wait until 7pm to verify if the Instances were stopped.

To accomplish this effectively, change the “Schedule pattern” occurrence to “One-time schedule”, set the “Date and time” to the current date and the time to 3–5 minutes in the future to limit wait time.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/04598e1231afd1dfa52250e41bcbc0df21cf7768/Images/Screenshot%202024-12-24%20134249.png)


Continue by clicking “Next”, then click on “Save schedule” once you reach the “Review and save schedule” step.

After the time has elapsed, navigate to the EC2 dashboard and verify that the three Development Instances have been stopped or are in the stopping state.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/4909338f0b99146fbb68e3661837871d6a5620e5/Images/Screenshot%202024-12-24%20134636.png)


# Success!

You’ve successful created a Lambda function that stops all Development EC2 Instances and integrated it with Amazon EventBridge to schedule the function to run at a specified time.


# More Insights

In your Lambda Function, select the “Monitor” tab, then click “View CloudWatch logs”.

![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/0a83466fb14a25b893b87371dd3c82e044e0652d/Images/Screenshot%202024-12-24%20134732.png)


In the CloudWatch window, you should see that the last event log stream’s “last event time” matches with the time we set from EventBridge, which was the last time the Lambda function was invoked.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/e7eb8a1a98040f498b906ed4b61fd88d84dac322/Images/Screenshot%202024-12-24%20134844.png)

Further more, if you select the Lambda function’s log stream, you can see more detailed information when the Lambda function was executed.


![image alt](https://github.com/Tatenda-Prince/Using-Lambda-and-EventBridge-to-stop-Instances-on-Schedule/blob/5eeac5de44fdc1b3c16623190358b5993ebb4f98/Images/Screenshot%202024-12-24%20134903.png)

# Congratulations!
You’ve reached the end. You’ve created a Lambda function triggered by an Amazon EventBridge schedule, which schedules the function to run at a specific time each day.


































