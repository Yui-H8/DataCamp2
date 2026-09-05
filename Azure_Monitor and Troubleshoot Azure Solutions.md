# Monitor and Troubleshoot Azure Solutions
### Description

This course focuses on teaching developers how to effectively monitor, observe, and troubleshoot Azure-based applications and services. It is designed for learners who build cloud solutions and need clear visibility into how their systems behave in real-world scenarios, from resource health to application performance.

---

### Exploring Azure Monitor
Your team wants to monitor the "heartbeat" of its Azure Storage Account to ensure customer orders and promotional content are being processed smoothly. To do this, you'll use the Metrics feature in Azure Monitor to view real-time transaction activity.

1. After your Azure Portal becomes visible and available to use:
   * Navigate to Storage accounts.
   * Open the pre-existing storage account and navigate into Monitoring in the left hand menu.

2. Let's now investigate the metrics
   * Head into Metrics.
   * Confirm your scope is set to your pre-existing storage account.
   * Use the Metric dropdown to select Transactions.
   * Set the time range to last 30 minutes using the time range selector.
   * Observe the chart.

3. Which type of visualization is shown by default when you select a metric in the metrics tab within monitoring?

Answer: Line Chart

*Correct! A line chart is shown by default in Azure Monitor Metrics, helping you visualize changes in a metric’s value over time. For a broader understanding of monitoring fundamentals, see: Azure Monitor fundamentals on Microsoft Learn*

### Responding to performance issues
Your team's web application occasionally experiences high CPU usage that impacts customer experience. The development team wants to be notified immediately when CPU usage exceeds 80 percent for five minutes so they can respond before it becomes a major problem. Which Azure Monitor component should they configure?

Answer: Alarts

*Correct! Alerts continuously track metrics and logs, triggering notifications like emails or webhooks when thresholds are crossed. This helps your team respond quickly and prevent small issues from becoming major problems.*

### Monitoring multiple resources
A software company's operations team manages multiple Azure resources, including storage accounts, web applications, and databases. They want to view the health and performance of all these resources in a single location and share this view with their team members. Which Azure Monitor component should they use?

  Answer: Dashboards

  *Correct! Dashboards bring together key data from your resources into a single view. You can pin metrics, logs, and alerts to get a complete picture of your application's health. Dashboards are fully customizable and shareable with your team.*

---
### Collect metrics through storage interactions
You manage a storage account that hosts marketing materials and customer documents. To maintain application health and performance, you need to understand how your storage is being used. Your task is to generate activity on the storage account by uploading and downloading files, then use the Metrics Explorer to view the automatically collected transaction, ingress, and egress metrics that Azure tracks for you.

1. Generate storage activity to create metrics:
   * Navigate to your Storage account and create a Container named "Monitor".
   * Upload at least three files from Desktop/Resources to generate ingress metrics.
   * Download each uploaded file to generate egress metrics
```
Hint
In the Azure Portal, search for your Storage account and open it.
Go to Data storage, then choose Containers.
Create a Container named "Monitor"
At the top, click Upload.
Click Browse for files and select multiple files from Desktop/Resources (choose at least three files).
Click Upload and wait for the upload to complete.
After upload, click on one of the files to open it.
At the top, click Download.
Save the file to your computer.
Repeat the download process for the other uploaded files.
```
2. View transaction metrics:
   * In your Storage account, under Monitoring, open Metrics.
   * In the Metrics Explorer, select "Transactions" as the metric.
   * Observe the chart showing your recent storage activity.
3. Add multiple metrics for comparison:
   * In the Metrics Explorer, add "Ingress" as a second metric.
   * Add "Egress" as a third metric.
   * Compare the three metrics on the same chart to understand how your storage operations affected data transfer.
4. Adjust the time range for detailed view:
