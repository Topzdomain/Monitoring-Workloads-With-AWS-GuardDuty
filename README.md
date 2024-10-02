<h2 align="center"> MONITORING WORKLOADS ON AWS USING AWS GUARDDUTY</h2>

This project demonstrates how to monitor AWS workloads using AWS GuardDuty. Amazon GuardDuty is a managed threat detection service provided by AWS that continuously monitors and analyzes an AWS environment for malicious or unauthorized activity. It helps detect security threats such as compromised accounts, malicious behaviour, unauthorized access, and potential data exfiltration without the need to deploy or maintain any additional infrastructure.

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/network-architecture.png?raw=true" height="65%" width="85%"/>
</p>
<h5 align="center"> Network Architecture</h5>

<h3 align="left"> SUMMARY</h3>
I have created a VPC and set up the networking to enable me to launch an EC2 instance and create a flow log. The first thing I'll start this project with is enabling GuardDuty on my account, after which I'll create a VPC Flow Log and Cloud Trail to send logs to an S3 bucket. I'll also create a Simple Notification Service (SNS) topic to send notifications to my email address which is the endpoint I'll provide and an event bridge rule to route GuardDuty findings to the SNS topic. To test and validate the setup, I'll add my IP address to GuardDuty's threat list. This would make guard duty pay much attention to suspicious activities from the listed IP addresses and flag them as findings while the SNS topic would send notifications of the findings to the email address I provided. Some of the actions I'll carry out include launching an EC2 in the VPC with the flow log enabled, terminating the instance and accessing an S3 bucket. These actions would become a part of the findings GuardDuty would generate.


<h3 align="left"> Enabling GuardDuty</h3>

I searched for guard duty in the services search bar and clicked on "Get Started". On the next page "Enable GuardDuty" to enable AWS GuardDuty then I did some configurations on it.

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/enabling-guardduty1.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/enabling-guardduty2.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/enabling-guardduty3.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/enabling-guardduty4.png" height="65%" width="85%"/>
</p>

In the left-hand navigation pane, I clicked on settings and under "Findings export options", I edited the frequency of notification to 15 mins.

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/guardduty-settings1.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/guardduty-settings2.png" height="65%" width="85%"/>
</p>

The next step is creating the VPC Flow log which the VPC would need to send to an S3 bucket (that is the approach I want to take). Therefore, I need to create the S3 bucket first.

<h3 align="left"> Creating the S3 Bucket</h3>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/s3-bucket-creation1.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/s3-bucket-creation2.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/s3-bucket-creation3.png" height="65%" width="85%"/>
</p>

<h3 align="left"> Creating the VPC Flow Log</h3>

To create the VPC flow log, I clicked on the VPC that I created and under the flow logs, I created a flow log. While creating the flow log, I chose S3 bucket as the destination for the generated logs and provided the arn of the S3 bucket (flow-logs-for-guardduty) I created in the step above.

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/flow-log-creation1.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/flow-log-creation2.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/flow-log-creation3.png" height="65%" width="85%"/>
</p>


<h3 align="left"> CloudTrail</h3>

AWS CloudTrail is a service that enables governance, compliance, and operational auditing of AWS accounts. It logs, continuously monitors, and retains account activity related to actions across an AWS infrastructure, providing visibility into API activity and who is doing what in an AWS environment. CloudTrail helps answer key questions like who made changes, what resources were affected, and when those actions occurred.

Creating a Trail To Log Events to S3 Bucket

In the process of creating the CloudTrail, I provided the S3 Bucket I had earlier created as the storage location for the logs.

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation1.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation2.png" height="65%" width="85%"/>
</p>

I scrolled to the button and clicked on "Next"

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation3.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation4.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation5.png" height="65%" width="85%"/>
</p>

<p align="center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/cloudtrail-creation6.png" height="85%" width="65%"/>
</p>

Within 15 - 20 minutes, CloudTrail should start depositing logs into the provided S3 Bucket.

<h3 align="left"> Creating SNS Topic</h3>

The next thing is to create the Simple Notification Service (SNS) Topic after which I'll subscribe the SNS topic to email notification. After the subscription has been created, I'll need to head over to my email and confirm the subscription. 

SNS Topic

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/sns-topic.png" height="65%" width="85%"/>
</p>

I clicked on the "Create topic" button at the bottom of the page after entering the topic name.

SNS Email Subscription

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/sns-subscription.png" height="65%" width="85%"/>
</p>

I clicked on the "Create subscription" button at the bottom of the page after entering my email address.

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/email-confirmation1.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Email Sent by SNS for Confirmation</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/email-confirmation2.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Subscription Confirmed</h5>


<h3 align="left"> EventBridge Rule Creation</h3>

Amazon EventBridge (CloudWatch Events previously) is a serverless event bus service that allows the creation of scalable event-driven applications. It helps in routing events from various sources like AWS services, SaaS applications, or custom applications to targets such as Lambda functions, Step Functions, or SQS queues. It allows different components or microservices to communicate with each other in a loosely coupled manner by using events.

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule1.png" height="65%" width="85%"/>
</p>

I clicked on the "Next" button at the bottom of the page after entering the event rule name.

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule2.png" height="65%" width="85%"/>
</p>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule3.png" height="85%" width="65%"/>
</p>

Under the "Event pattern", I selected "Custom patterns" then I copied the pattern sample for GuardDuty from the AWS documentation page (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings_cloudwatch.html) and pasted it in the textbox. I don't want SNS sending me notifications for severities lesser than 5, so I deleted all the figures below 5.

Event Pattern Code
```commandline
{
  "source": [
    "aws.guardduty"
  ],
  "detail-type": [
    "GuardDuty Finding"
  ],
  "detail": {
    "severity": [
      5,
      5.0,
      5.1,
      5.2,
      5.3,
      5.4,
      5.5,
      5.6,
      5.7,
      5.8,
      5.9,
      6,
      6.0,
      6.1,
      6.2,
      6.3,
      6.4,
      6.5,
      6.6,
      6.7,
      6.8,
      6.9,
      7,
      7.0,
      7.1,
      7.2,
      7.3,
      7.4,
      7.5,
      7.6,
      7.7,
      7.8,
      7.9,
      8,
      8.0,
      8.1,
      8.2,
      8.3,
      8.4,
      8.5,
      8.6,
      8.7,
      8.8,
      8.9
    ]
  }
}
```

In the "Select target(s)" page, I selected AWS service, my target SNS topic and selected the topic from the drop-down (since I want GuardDuty findings routed to my SNS topic by EventBridge). Next, I clicked on the additional settings to configure how I want the findings to appear in my email, as JSON is not read-friendly. So in the "Configure target input" dropdown, I selected "Input transformer" and then clicked on "Configure input transformer". In the textbox for "Target input transformer" and "Template", I pasted the sample format code in the AWS documentation page (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings_cloudwatch.html) into the respective boxes, clicked on the "Confirm" button at the bottom of the page to confirm the input transformer settings then clicked on "Next" on the select targets page.

JSON FORMAT CODE

```commandline
{
    "severity": "$.detail.severity",
    "Account_ID": "$.detail.accountId",
    "Finding_ID": "$.detail.id",
    "Finding_Type": "$.detail.type",
    "region": "$.region",
    "Finding_description": "$.detail.description"
}
```

CUSTOMIZED EMAIL NOTIFICATION CODE

```commandline
"AWS <Account_ID> has a severity <severity> GuardDuty finding type <Finding_Type> in the <region> region."
"Finding Description:"
"<Finding_description>. "
"For more details open the GuardDuty console at https://console.aws.amazon.com/guardduty/home?region=<region>#/findings?search=id%3D<Finding_ID>"
```

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule4.png" height="85%" width="65%"/>
</p>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule5.png" height="85%" width="65%"/>
</p>
<h5 align="center"> Configuring Input Transformer</h5>

No sample event is needed.

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule6.png" height="85%" width="65%"/>
</p>
<h5 align="center"> Entering JSON Format and Transformed Plain Text Format for Findings Sent to the SNS Endpoint (Email)</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/eventbridge-rule7.png" height="65%" width="85%"/>
</p>

I navigated to the "Review and create" page, then clicked on "Create rule"


<h3 align="left"> Test and Validation</h3>
To validate the setup, I create a text document on my machine named "Threat IP List" with my public IP address written into the text file, create a "threat-ip-list01" bucket on S3 and upload the text document "Threat IP List.txt" into the bucket. Next, on the left navigation pane of GuardDuty's Page, I'll click on List and add the information of the S3 bucket into the Threat List, then activate the List for GuardDuty to start generating findings about activities carried out by the IP address(es) provided. I'll wait 5 minutes for GuardDuty to get and analyze the logs of activities I am currently carrying out on the account (since I am technically a threat actor at that moment), populate the findings on the Findings Dashboard, and SNS sending the notifications to my email address for medium and high severity issues. While waiting the 5 mins, I'll be engaging the account, creating and terminating an EC2 instance, navigating S3 Buckets, IAM and engaging the account generally so that GuardDuty can generate findings about those activities.

 
<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/s3-threat-list-bucket1.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Overview of S3 Event Logs Bucket and Threat IP List Bucket</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/s3-threat-list-bucket2.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Threat IP List Uploaded to the Bucket</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/threat-ip-list1.png" height="85%" width="65%"/>
</p>
<h5 align="center"> Providing Details of the S3 Bucket Containing the Threat IP List on GuardDuty</h5>

I clicked on the "Add List" button when I was done providing the details

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/threat-ip-list2.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Activating the Threat IP List</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/guardduty-summary-page.png" height="65%" width="85%"/>
</p>
<h5 align="center"> GuardDuty Summary Page</h5>

Within 5 to 10 minutes of adding the Threat IP List and activating it, GuardDuty started flagging findings of activities carried out by the IP address provided in Threat IP List, with severity ranging from high to low.

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/guardduty-findings-page.png" height="65%" width="85%"/>
</p>
<h5 align="center"> GuardDuty Showing Overview of Findings, Severity and AWS Account Affected</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/high-severity-finding1.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Details of a GuardDuty Findings, Showing the affected resources, severity, Time of Action, Account Affected, Region and Count(Number of Times it was Detected)</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/high-severity-finding2.png" height="65%" width="85%"/>
</p>
<h5 align="center"> Details of a GuardDuty Findings, Showing Type of Action Carried Out by Threat Actor, IP Address, Location and Other Vital Information</h5>

<p align= "center">
<img src="https://github.com/Topzdomain/Monitoring-Workloads-With-AWS-GuardDuty/blob/main/screenshots/sns-email-notification.png" height="65%" width="85%"/>
</p>
<h5 align="center"> GuardDuty Findings Sent by SNS to Email Endpoint</h5>


A list of trusted IP addresses can also be provided in the Trusted IP list. GuardDuty would ignore any activity carried out by those IP addresses whether they are malicious or not. 10,000 IP addresses or CIDR ranges can be provided in a single IP List whether threat or trusted, 6 Lists of 10,000 IP addresses or CIDR ranges are allowed across both Trusted IP Lists and Threat IP Lists. 

Also, whether an IP List is provided or not, GuardDuty would still monitor the account and flag abnormal or malicious findings. This is the best configuration as it monitors the account for both internal and external threats.

A common remediation for flagged findings would be to block the IP address(es) of malicious actors or better still whitelist the IP address(es) that can gain access to the cloud resources from the NACLs, Security Groups or Firewall rules, depending on the architecture and configuration of the cloud resources. Aside from these, other Defence-In-Depth controls such as enforcing MFA, IAM roles and policy configurations can be applied to accounts and resources to keep them secure.

Denial of access to compromised accounts/resources through the security group should be effected while investigating the cause of compromise and trying to restore business continuity.
