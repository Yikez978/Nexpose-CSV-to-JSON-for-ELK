# Nexpose-CSV-to-JSON-for-ELK
Prep Nexpose CSV reports to JSON format for ELK

I pull SQL reports from Nexpose - and I wanted to manipulate the data and make it easier to correlate with other data.
I also wanted to use this as an opportunity to learn more about ELK, sed, and bash.
This script pushes the data to ELK

This code is tested on CentOS 6.7 and Ubuntu 14.04.3

To run this script requires the following software to be successful:

Nexpose Reporting User<br>
User: reporting<br>
Roles: Custom<br>
Privileges:<br>
* Appear on Ticket and Report Lists
* View Site Asset Data
* View Group Asset Data
* Create Reports
* Use Restricted Report Sections

All reports must be created by the user in order to be run by this user so that the report appears in the specified user directory.<br>
On the Nexpose VM I installed the following in addition to Nexpose.<br>
Install Python, install  python-pip, install pip csvkit<br>
Install mmv for mcp - centOS http://li.nux.ro/download/nux/dextop/el6/x86_64/mmv-1.01b-16.el6.nux.x86_64.rpm<br>

There are several directories involved.<br>
1) The original Nexpose report directory located under<br>
	/opt/rapid7/nexpose/nsc/reports/<user>/<reportname>/report.csv<br>
2) The new nexpose report directory - with the report renamed to the reportname directory<br>
	/opt/jsondata/reports/nexpose/SQL-Report_YYYY-MM-DD_HH-MM-SS_report.csv<br>
3) The json report directories - named according to type (in my case I have two reports - one for scans and one for exceptions)<br>
        /opt/jsondata/reports/json/scan and /opt/jsondata/reports/json/exception<br>
4) The processed directory where processed reports are placed.<br>
        /opt/rapid7/nexpose/nsc/reports/<user>/processed/<br>
  
What this script does:
  
1) Batch rename all reports under specified user to foldername.csv using mcp and then moves them to a new unprocessed reporting directory.<br>
2) Process all files in new nexpose reporting directory to json format and moves them to the json reporting directory using csvkit's csvjson.<br>
3) Adds the json formatted index header for each event based on type.<br>
3) Uses curl API call to Elasticsearch to inject the json files that were created.<br>

This is a work in progress and should not be used in production.
I will provide mappings and the SQL queries used for the csv report.

  
  
