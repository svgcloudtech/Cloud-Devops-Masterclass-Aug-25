# 🚀 AWS CloudWatch & CloudTrail Integration with Slack (via Lambda)

## 🎯 Objective
In this lab, students will learn how to:
- Capture AWS events (EC2, S3, CloudTrail activity)  
- Route logs to **CloudWatch Logs**  
- Trigger a **Lambda function** on specific events  
- Send formatted notifications to **Slack channels**  

---

## 🛠️ Prerequisites
- AWS account with IAM permissions for CloudWatch, CloudTrail, Lambda, and S3/EC2  
- Slack workspace with admin rights to create an **Incoming Webhook**  
- Basic knowledge of IAM roles & Lambda environment variables  

---

## 📌 Steps

### 1. Setup Slack Webhook
1. Go to [Slack API Webhooks](https://api.slack.com/messaging/webhooks).  
2. Create a new **Incoming Webhook** for your channel.  
3. Copy the **Webhook URL** (you’ll need this for Lambda).  

---

### 2. Enable CloudTrail
1. Open **CloudTrail** in AWS Console.  
2. Create a **trail** and enable it for **all regions**.  
3. Store logs in a dedicated **S3 bucket** (example: `cloudtrail-student-labs`).  

---

### 3. Create CloudWatch Log Group
1. Go to **CloudWatch → Logs → Log Groups**.  
2. Create a new log group, e.g., `/aws/cloudtrail/slack-notify`.  

---

### 4. Configure Lambda Function
1. Go to **AWS Lambda → Create function**.  
   - Runtime: **Python 3.x** (recommended)  
   - Permissions: Attach a role with **CloudWatchLogsReadOnlyAccess** and **AmazonSNSFullAccess**.  

2. Add environment variable:
   ```text
   SLACK_WEBHOOK_URL = https://hooks.slack.com/services/XXXX/XXXX/XXXX
   ```

3. Paste the Lambda code:

   ```python
   import json
   import urllib3
   import os

   http = urllib3.PoolManager()
   slack_url = os.environ['SLACK_WEBHOOK_URL']

   def lambda_handler(event, context):
       message = json.dumps(event, indent=2)
       payload = {
           "text": f"🚨 AWS Event Alert 🚨\n```{message}```"
       }
       encoded_msg = json.dumps(payload).encode('utf-8')
       resp = http.request('POST', slack_url, body=encoded_msg, headers={'Content-Type': 'application/json'})
       print({
           "status_code": resp.status,
           "response": resp.data
       })
   ```

---

### 5. Create CloudWatch Rule
1. Open **CloudWatch → Rules → Create Rule**.  
2. Select **Event Source → AWS API Call via CloudTrail**.  
   - Example: `RunInstances` (EC2 launch), `PutObject` (S3 upload).  
3. Set **Target → Lambda function** (your Slack notifier).  

---

### 6. Test the Flow
- Launch an **EC2 instance** or upload a file to **S3**.  
- Check your **Slack channel** for the notification.  

---

## ✅ Expected Output
Students should see real-time alerts in Slack, such as:

```
🚨 AWS Event Alert 🚨
{
  "eventName": "RunInstances",
  "eventSource": "ec2.amazonaws.com",
  "userIdentity": { ... },
  "requestParameters": { ... }
}
```

---

## 📚 Additional Exercises
- Modify Lambda to send only **critical events**.  
- Format messages with Slack **blocks** for better readability.  
- Integrate **SNS → Lambda → Slack** as an alternative flow.  
