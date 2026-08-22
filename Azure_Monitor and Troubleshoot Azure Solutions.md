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
