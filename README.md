# Zabbix - Synology Active Backup for Business task monitoring

## Features
* Discovery of all backup tasks (VM, Physical server, SMB and PC)
* Item - Status of backup task
* Item - Transferred bytes
* Item - Start time backup
* Item - End time backup
* Trigger - Backup operation error

## Installation

### Synology configuration

* Enable SSH on Synology 
* Create the scheduled task on your Synology that exports data from the Active Backup for Business database to a CSV file, paste the two commands in the task (change paths in bold to Active Backup/config location and location for exported CSV files):

>*sqlite3 -header -csv **/volumeX/@ActiveBackup/activity.db** "select config_device_id, device_name, status, transfered_bytes, time_start, time_end  from (SELECT * FROM device_result_table ORDER BY config_device_id) GROUP BY config_device_id;" | awk '{gsub(/\"/,"")};1' > **/volumeX/sharename/ActiveBackupExport.csv***
>
>*sqlite3 -header -csv **/volumeX/@ActiveBackup/config.db** "select device_id, host_name, backup_type from device_table" | awk '{gsub(/\"/,"")};1' > **/volumeX/Server/ActiveBackupHostExport.csv***


* Let this scheduled task run every 10mins (adjust as you wish)

*Synology scheduled tasks: https://www.synology.com/en-global/knowledgebase/DSM/help/DSM/AdminCenter/system_taskscheduler*


### Zabbix configuration
* Import value mapping - **Synology_ABB_ValueMapping.xml**
* Import template - **Synology_ABB_Template.xml**
* Add template to Synology host
* Configure template macros (SSH & CSV path)
