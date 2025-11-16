# Project:SSH-log-analysis-using-Splunk
# Objective
The goal of this project is to analyze SSH authentication logs to detect:
1. Successful logins (who connected, from where)
2. Failed login attempts (possible brute-force or password spraying)
3. Multiple failed authentication attempts (indicators of brute-force)
4. Connections without authentication (potential scanning or incomplete sessions)

# Environment Setup
# Tools Used
1. Ubuntu Server
2. Splunk Enterprise
3. SSH Service (OpenSSH). Download the SSH log file:https://drive.google.com/drive/folders/1BL-kVlc3yCRcAH8NnDmAyRm_3j_2mNLM?usp=sharing

# Splunk Configuration
1. Download Splunk Enterprise
2. Install using: sudo dpkg -i splunk*.deb
3. Start Splunk: sudo /opt/splunk/bin/splunk start
4. Login via browser at http://localhost:8000

# Preparation
1. Log in to your Splunk Enterprise
2. Go to Apps > Search & Reporting
3. Click Add Data → Upload
4. Select the provided ssh_log.json file and upload it
5. Choose sourcetype = _json so Splunk automatically extracts fields
6. Index it under a new index, e.g., ssh_logs
7. Review and click on start searching

# Step by Step Guide
# 1. Ingest and Parse logs 
1. Upload ssh_log.json into Splunk
2. Ensure the following fields are extracted correctly:
event_type (Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication),
    auth_success (true/false/null),
    auth_attempts
    id.orig_h (source IP),
    id.resp_h (destination host)
3. Run a validation search: index=ssh_log | stats count by event_type

# 2. Analyze Failed Login Attempts
Identify all failed login attempts: index=ssh_logs event_type="Failed SSH Login" | stats count by id.orig_h
1. Highlight the top 10 source IPs generating failed logins
2. Create a bar chart visualization for failed login attempts per source IP

# 3. Detect Multiple Failed Authentication Attempts (Brute Force)
Search for multiple failed attempts in logs: index=ssh_logs event_type="Multiple Failed Authentication Attempts"| stats count by id.orig_h, id.resp_h
1. Detect repeated failures (e.g., more than 5 attempts).
2. Configure a Splunk alert: Trigger when any IP attempts more than 5 logins within 10 minutes

# 4. Track Successful Logins
Search for successful logins: index=ssh_logs event_type="Successful SSH Login"| stats count by id.orig_h, id.resp_h
1. Compare successful logins against prior failed attempts (to detect compromised accounts)
2. Create a dashboard panel showing top source IPs for successful logins

# 5. Spot Suspicious Connections Without Authentication
Search for unauthenticated SSH connections:index=ssh_logs event_type="Connection Without Authentication"| stats count by id.orig_h
1. Create a timechart visualization to monitor such events over time: index=ssh_logs event_type="Connection Without Authentication"| timechart count by id.orig_h
Identify repeated unauthenticated attempts — potential indicators of port scanning or SSH probing.

# Conclusion
The SSH log analysis using Splunk helped detect suspicious login attempts and revealed multiple brute-force attempts. By leveraging dashboards, and visual analytics, this project demonstrates effective real-world log monitoring and security visibility.





