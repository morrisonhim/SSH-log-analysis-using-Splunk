# Project:SSH-log-analysis-using-Splunk
# Objective
The goal of this project was to analyze SSH authentication logs to detect:
1. Successful logins (who connected, from where)
2. Failed login attempts (possible brute-force or password spraying)
3. Multiple failed authentication attempts (indicators of brute-force)
4. Connections without authentication (potential scanning or incomplete sessions)

# Environment Setup
# Tools Used
1. Ubuntu Server
2. Splunk Enterprise
3. SSH Service (OpenSSH). [Download the SSH log file](https://drive.google.com/drive/folders/1BL-kVlc3yCRcAH8NnDmAyRm_3j_2mNLM?usp=sharing)

# Splunk Configuration
1. Downloaded Splunk Enterprise
2. Installed using: sudo dpkg -i splunk*.deb
3. Started Splunk: sudo /opt/splunk/bin/splunk start
4. Logged in via a browser at http://localhost:8000

# Preparation
1. Logged in to your Splunk Enterprise
2. Navigated to Apps > Search & Reporting
3. Clicked Add Data → Upload
4. Selected the provided ssh_log.json file and uploaded it
5. Chose sourcetype = _json so Splunk automatically extracted fields
6. Indexed it under a new index, e.g., ssh_logs
7. Reviewed and started searching

# Step by Step Guide
# 1. Ingest and Parse logs 
1. Uploaded ssh_log.json into Splunk
2. Ensured the following fields were extracted correctly:
   + event_type (Successful SSH Login, Failed SSH Login, Multiple Failed Authentication Attempts, Connection Without Authentication)
   + auth_success (true/false/null)
   + auth_attempts
   + id.orig_h (source IP)
   + id.resp_h (destination host)
3. Ran a validation search: index=ssh_log | stats count by event_type

# 2. Analyze Failed Login Attempts
Identified all failed login attempts: index=ssh_logs event_type="Failed SSH Login" | stats count by id.orig_h
+ Highlighted the top 10 source IPs generating failed logins
+ Created a bar chart visualization for failed login attempts per source IP

# 3. Detected Multiple Failed Authentication Attempts (Brute Force)
Searched for multiple failed attempts in the logs: index=ssh_logs event_type="Multiple Failed Authentication Attempts"| stats count by id.orig_h, id.resp_h
+ Detected repeated failures (e.g., more than 5 attempts).
+ Configured a Splunk alert: Triggered when any IP attempted more than 5 logins within 10 minutes

# 4. Tracked Successful Logins
Searched for successful logins: index=ssh_logs event_type="Successful SSH Login"| stats count by id.orig_h, id.resp_h
+ Compared successful logins against prior failed attempts (to detect compromised accounts)
+ Created a dashboard panel showing top source IPs for successful logins

# 5. Spotted Suspicious Connections Without Authentication
Searched for unauthenticated SSH connections:index=ssh_logs event_type="Connection Without Authentication"| stats count by id.orig_h
1. Created a timechart visualization to monitor such events over time: index=ssh_logs event_type="Connection Without Authentication"| timechart count by id.orig_h
2. Identified repeated unauthenticated attempts — potential indicators of port scanning or SSH probing.

# Conclusion
The SSH log analysis using Splunk helped detect suspicious login attempts and revealed multiple brute-force attempts. By leveraging dashboards, and visual analytics, this project demonstrates effective real-world log monitoring and security visibility.





