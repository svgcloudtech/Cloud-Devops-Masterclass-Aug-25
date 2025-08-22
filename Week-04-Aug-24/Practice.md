Day 2 – CloudWatch & CloudTrail

## CloudWatch Basics

### Console Steps:
1. Go to **AWS Console → CloudWatch → Metrics**
2. Select **EC2 → Per-Instance Metrics**
3. View CPUUtilization for your EC2 instance
4. Go to **Alarms → Create Alarm**
   - Metric: CPUUtilization
   - Condition: Greater than 70% for 5 minutes
   - Action: Send notification to SNS topic
5. Go to **Logs → Create Log Group** and stream EC2 logs with CloudWatch Agent

### CLI Steps:
```bash
aws cloudwatch put-metric-data --metric-name StudentMetric --namespace Student/EC2 --value 5

aws cloudwatch put-metric-alarm   --alarm-name HighCPU   --metric-name CPUUtilization   --namespace AWS/EC2   --statistic Average   --period 300   --threshold 70   --comparison-operator GreaterThanThreshold   --dimensions Name=InstanceId,Value=i-123456   --evaluation-periods 1   --alarm-actions arn:aws:sns:us-east-1:123456789012:MySNSTopic
```

**Expected Outcome:**  
- Metrics visible in CloudWatch  
- Alarm triggers when CPU > 70%

---

## CloudTrail Basics

### Console Steps:
1. Go to **AWS Console → CloudTrail → Trails**
2. Click **Create Trail**
   - Name: `student-trail`
   - Apply to all regions
   - Store logs in new S3 bucket
   - Enable CloudWatch Logs integration (optional)
3. Perform activity: Start/Stop EC2 instance
4. Go back to **Event History** to see activity

### CLI Steps:
```bash
aws cloudtrail create-trail --name student-trail --s3-bucket-name student-trail-logs
aws cloudtrail start-logging --name student-trail
aws cloudtrail lookup-events --max-results 5
```

**Expected Outcome:**  
- CloudTrail records all AWS API activity  
- Events visible in S3 and Event History

---

## Combined Example – Security & Monitoring

1. Launch an EC2 instance  
2. Enable **detailed monitoring** in CloudWatch  
3. Set a **CPU Alarm** (>70%)  
4. Start/Stop instance → verify **CloudTrail logs** show activity  
5. Confirm CloudTrail events visible in **CloudWatch Logs** if integrated  

**End Goal:**  
By the end of Week 4, students should be able to:  
- Launch and secure EC2 instances  
- Manage EBS volumes and snapshots  
- Distribute traffic with ELB  
- Configure Auto Scaling Groups  
- Apply EC2 security best practices  
- Monitor metrics with CloudWatch  
- Trace user/API activity with CloudTrail  