---
title : "Monitoring and Logging"
date : 2025-08-12
weight : 7
chapter : true
pre : " <b> 7. </b> "
---

**Contents:**
- [Overview](#overview)
- [Enable Amazon CloudWatch Logs](#enable-amazon-cloudwatch-logs)
- [Enable Container Insights](#enable-container-insights)
- [Viewing Logs and Metrics](#viewing-logs-and-metrics)
- [Integrating Alerts (Alarms)](#integrating-alerts-alarms)

---

#### Overview

Monitoring and logging are essential for tracking the health and performance of your application and infrastructure.  
In this workshop, we will use:
- **Amazon CloudWatch Logs** to store and analyze logs from containers.
- **CloudWatch Container Insights** to monitor CPU, Memory, Network, and Storage usage.
- **CloudWatch Alarms** to receive notifications when issues occur.

---

#### Enable Amazon CloudWatch Logs

1. Go to **ECS Console** → **Cluster** → Select your service.
2. In the **Task Definition**, check the log driver configuration:
   - Select `awslogs` as the log driver.
   - Log Group name: `/ecs/fargate-workshop`
   - Region: `us-east-1` (or your chosen AWS region).
3. Save and redeploy the service.
![Amazon CloudWatch Logs](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/07/01.png)

#### Enable Container Insights

1. Go to **CloudWatch Console** → **Container Insights**.
2. Select **Enable** for your ECS Cluster.
3. Container Insights will automatically collect:
   - CPUUtilization
   - MemoryUtilization
   - NetworkRxBytes / NetworkTxBytes
   - StorageReadBytes / StorageWriteBytes

---

#### Viewing Logs and Metrics

1. Open **CloudWatch Console** → **Logs** → Select the Log Group `/ecs/fargate-workshop`.
2. View container logs (stdout, stderr).
3. Go to **CloudWatch Metrics** → ECS → Cluster Metrics to see CPU, RAM, and Network statistics.
4. Filter by specific services or tasks for detailed analysis.

---

#### Integrating Alerts (Alarms)

1. Go to **CloudWatch Console** → **Alarms** → **Create alarm**.
2. Select a metric, for example: `ECS/ContainerInsights – CPUUtilization`.
3. Set a threshold, e.g., CPU > 80% for 5 minutes.
4. Choose an SNS Topic to receive email notifications when the alarm is triggered.
5. Save and activate the alarm.

---

**Expected Outcome:**
- You can view logs for each container to debug issues.
- Monitor application performance in real-time.
- Receive notifications when the system encounters issues or exceeds resource thresholds.

---
