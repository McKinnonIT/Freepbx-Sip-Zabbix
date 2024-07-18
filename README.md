Monitoring FreePBX SIP Channels and Activities with Zabbix
======
### Introduction
This README provides instructions on how to monitor SIP channels and activities from FreePBX using Zabbix, a popular open-source monitoring solution.
### Prerequisites
- Zabbix installed and configured
- FreePBX installed and configured
- Access to the command line interface of FreePBX server
- Access to the Web interface of Zabbix server
- Basic knowledge of Linux command line
### Steps
#### 1. Install Zabbix Agent
Ensure that the Zabbix agent is installed and running on the FreePBX server. Install the Zabbix agent if not already installed.

#### 2. Configure Zabbix Agent
Edit the Zabbix agent configuration file (zabbix_agentd.conf) on the FreePBX server and add the following user parameters
```
UserParameter=outgoing.channels[*],/usr/sbin/asterisk -rx "core show channels verbose" | grep -c "^PJSIP.*"
UserParameter=incoming.channels[*],/usr/sbin/asterisk -rx "core show channels" | grep -Ew "SIP.*PJSIP" | wc -l
UserParameter=internal[*],/usr/sbin/asterisk -rx "core show channels" | grep -Ew "PJSIP.*PJSIP" | wc -l
UserParameter=external[*],/usr/sbin/asterisk -rx "core show channels" | grep "^SIP" | wc -l
UserParameter=parked[*],/usr/sbin/asterisk -rx "core show channels" | grep "park" | wc -l
UserParameter=incoming.main[*],/usr/sbin/asterisk -rx "core show channels" | grep "^SIP/0385209000-in-main" | wc -l
UserParameter=outgoing.main[*],/usr/sbin/asterisk -rx "core show channels" | grep "^SIP/0385209000-out-main" | wc -l
UserParameter=incoming.east[*],/usr/sbin/asterisk -rx "core show channels" | grep "^SIP/0385209000-in-east" | wc -l
UserParameter=outgoing.east[*],/usr/sbin/asterisk -rx "core show channels" | grep "^SIP/0385209000-out-east" | wc -l
```
#### 3. Restart Zabbix Agent
Restart the Zabbix agent on the FreePBX server to apply the changes.

#### 4. Configure the FreePBX host via the Zabbix Web Interface
Create the following items in the Zabbix web interface:
```
Outgoing Channels: Type: Zabbix agent, Key: outgoing.channels[*]
Incoming Channels: Type: Zabbix agent, Key: incoming.channels[*]
Internal Calls: Type: Zabbix agent, Key: internal[*]
External Calls: Type: Zabbix agent, Key: external[*]
Parked Calls: Type: Zabbix agent, Key: parked[*]
Incoming Calls to Main: Type: Zabbix agent, Key: incoming.main[*]
Outgoing Calls from Main: Type: Zabbix agent, Key: outgoing.main[*]
Incoming Calls to East: Type: Zabbix agent, Key: incoming.east[*]
Outgoing Calls from East: Type: Zabbix agent, Key: outgoing.east[*]
```
#### 5. Verify Configuration

Verify that the Asterisk service is running on the FreePBX server.
Restart the Zabbix agent after making any changes.
#### 6. Monitor SIP Channels and Activities

Access the Zabbix web interface to monitor SIP channels and activities from the FreePBX server. Use the created items to make graphs to monitor activity.
