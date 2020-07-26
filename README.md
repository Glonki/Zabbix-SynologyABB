# Zabbix - Synology Active Backup for Business task monitoring

## Features
* Discovery of all backup tasks
* Item - Status of backup task
* Item - Transferred bytes
* Item - Start time backup
* Item - End time backup
* Trigger - Backup operation error

## Installation

### Synology configuration

* Enable SSH on Synology 
* Create the scheduled task on your Synology that exports data from the Active Backup for Business database to a CSV file, paste this command in the task (change paths in bold to Active Backup location and location for exported CSV file):

>*sqlite3 -header -csv **/volume/@ActiveBackup/activity.db** "select device_name, status, transfered_bytes, time_start, time_end  from (SELECT * FROM device_result_table ORDER BY device_name) GROUP BY device_name;" > **/volume/share/ActiveBackupExport.csv***

* Let this scheduled task run every 10mins (adjust as you wish)

### Zabbix configuration
* Import value mapping - **Synology_ABB_ValueMapping.xml**
* Import template - **Synology_ABB_Template.xml**
* Add template to Synology host
* Configure template macros (SSH & CSV path)
