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
