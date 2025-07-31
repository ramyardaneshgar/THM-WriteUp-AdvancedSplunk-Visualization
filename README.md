# Splunk-Dashboards & Reports
Splunk Dashboards and Reports - Data aggregation, visualization, and proactive alerting in a SOC enviornment.

By Ramyar Daneshgar

## **1. Introduction**
This write-up details my approach to organizing data, creating reports, building dashboards, and setting up alerts within Splunk as part of the **SOC Level 2 - Advanced Splunk** module. The objective was to simulate a real-world SOC environment where data aggregation, visualization, and proactive alerting are critical for identifying and responding to security incidents. 

The target machine IP used was **10.10.229.129**, and the lab environment provided data from simulated logs (VPN servers, network servers, and web servers). The exercise offered hands-on exposure to Splunk’s capabilities, mimicking challenges encountered in a production SOC environment.

---

## **2. Organizing Data in Splunk**
### **Context and Actions**
When accessing Splunk, the first step was to gain visibility into the available data sources. To do this, I used the command:

``` 
index=* 
```

This command retrieved events across all indices, with the time range set to **All Time**. While useful in a lab environment for exploration, this approach comes with significant caveats. Broad searches like `index=*` can be resource-intensive and impractical in production environments, especially when dealing with millions of logs.

### **Lessons Learned**
- In a production SOC, avoid using `index=*` without filters to prevent search performance degradation.
- Always narrow searches by specific indices, time ranges, or fields to optimize resource use and search speed.

---

## **3. Creating Reports for Recurring Searches**
### **Context and Actions**
SOC analysts often require repetitive queries for monitoring purposes, such as identifying specific user behavior or network activity. To automate this process, I created reports to save queries for reuse.

**Example 1: VPN Login Report**
To determine how often each user logged into the VPN, I executed:

```
host=vpn_server | stats count by Username
```

- **Steps Taken**:  
   - Saved the query as a report named `VPN_User_Login_Report` using the **Save As > Report** option.
   - Enabled the **Time Range Picker** for flexibility when running the report with varying time frames.

**Example 2: Network Ports Report**
To identify network ports and their frequency of use, I ran:

```
host=network-server | stats count by port
```

- Observations: The most frequently used port appeared **5 times**.

### **Lessons Learned**
- Creating reports reduces search head load by eliminating the need to rerun identical queries. 
- Reports with time range pickers add flexibility for analysts to focus on specific windows of interest.
- Automating recurring searches is a key best practice for SOC efficiency.

---

## **4. Creating Dashboards for Summarizing Results**
### **Context and Actions**
Dashboards provide quick insights through visual summaries, which are vital for both SOC analysts and stakeholders.

**Example: VPN Login Visualization**
1. I created a dashboard named `VPN_Login_Dashboard` using the **Classic Dashboard** option.  
2. Added the `VPN_User_Login_Report` to the dashboard using **Add Panel > New from Report**.  
3. Applied a **Column Chart** visualization to display user login counts visually.  

**Web Server Status Codes:**
To track HTTP status codes from the web server, I ran the following query:

```
host=web-server | stats count by status
```

- Added this query to the dashboard with a **Line Chart** visualization.  
- Results indicated that **400** status code occurred the least number of times.

**Customizations:**
- Enabled a **Dark Theme** for better visibility.
- Ensured dashboard permissions were set to **Shared in App** to allow access to other users.

### **Lessons Learned**
- Visualizing data through dashboards simplifies anomaly detection. For example, spikes or dips in login attempts or status codes are immediately visible.  
- Combining multiple panels on a single dashboard allows analysts to correlate data across sources efficiently.  
- Proper permission settings ensure dashboards can be shared across the team, promoting collaboration.

---

## **5. Alerting on High-Priority Events**
### **Context and Actions**
Proactive alerting is essential for timely detection and response to security incidents. I configured an alert to monitor excessive login attempts for a specific user, "Sarah," to simulate detecting brute-force activity.

**Steps to Configure Alert**:
1. Narrowed the search to Sarah's login activity:

```
host=vpn_server Username=Sarah | stats count
```

2. Saved the query as an alert:
   - Configured it to trigger if the login count exceeded **5** within an hour.
   - Enabled **Throttle** to suppress duplicate alerts for 60 minutes, reducing alert fatigue.
   - Set a **Trigger Action** to send a high-priority email to `soc@tryhackme.com`.

### **Lessons Learned**
- Alerts provide proactive notifications, enabling SOC teams to respond immediately to potential threats like brute-force attempts.  
- Configuring **Throttle** prevents excessive notifications, which is crucial to avoid overwhelming analysts with redundant alerts.  
- Integrating alerts with trigger actions, such as email notifications, streamlines incident escalation workflows.

---

## **6. Conclusion**
This lab provided practical exposure to Splunk’s core capabilities for search, reporting, dashboarding, and alerting. I gained valuable insights into managing large datasets effectively and optimizing Splunk workflows for a SOC environment.

### **Key Lessons Learned**:
1. **Efficiency in Search**: Avoid broad searches (`index=*`); instead, use targeted filters and time ranges.  
2. **Report Automation**: Save recurring queries as reports to minimize manual intervention and reduce search head load.  
3. **Visual Insights**: Dashboards simplify the identification of trends and anomalies, improving situational awareness.  
4. **Proactive Monitoring**: Alerts ensure critical events are detected and acted upon in real time. Throttling is essential to reduce noise.  

By leveraging these best practices, Splunk becomes a powerful tool for proactive threat detection, log analysis, and security monitoring. This exercise reinforced how crucial it is to implement automation and visualization to enable SOC analysts to focus on high-priority incidents effectively.

--- 

