# Threat-Hunting-1

## Log analysis and Threat Hunting using Splunk
This lab will demonstrate how i conducted a threat hunt using splunk. <br/>
<br/>
This project showcases my threat hunting capabilities using Splunk, where I analyzed logs to detect and investigate patterns associated with malicious activity. It highlights my ability to identify suspicious behaviors, correlate events, and derive meaningful insights from log data to support effective security monitoring and incident investigation. <br/>

## Steps i Followed

<b>Step 1: Find suspicious running processes</b><br/>
   Using the splunk filter bar, we search for logs related to started processes using <strong>index="index_name" EventCode=1<strong/><br/>
   
<p align="center">
  Search Running processes:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/97839aa9-4ad2-4532-8f5c-93aca594f04b" height="80%" width="80%"/>


<br />
<br />

<p align="center">
  Check images to see the processes involved and scan them using VirusTotal:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/825fa099-aa35-4509-83cf-995e1a7e1d7b" height="80%" width="80%"/>


<br />
<br />

<p align="center">
  One of the files called "Prevetivo24.02.1.exe" was flagged malicious by the vendors:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/fcaff838-b328-4ca9-923d-1fbf8582df35" height="80%" width="80%"/>


<br />
<br />
   
<b>Step 2: Analyse the malicious file</b><br/>

<p align="center">
 Investigate on VirusTotal by going to COMMUNITY. There i found "Dropbox Malware" which is likely the cloud drive used to distribute the malware:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/be905964-b00e-47de-b139-be58aef95cfc" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can also use Splunk by pivoting on the name of the malware and the FileCreate Event ID 11. As seen, there are 9 events:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/42ebee3d-defe-42d2-951c-b5fb18258935" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can now pivot to the surrounding events of the first event by clicking on the time and select +/- 5 seconds. This gives us all the events that occured before and after the time the file was written to disk:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/ed8398b1-e42a-416a-ad04-8a619df31db1" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 All events that occured 5 seconds before and after the file was created on the disk. We have 7 events in total:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/947ae6e5-cda9-4532-868d-99255c74878e" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can then use 'index="sysmonsplunklab" EventCode=22 preventio24' to see which domain the malicious file attempted to connect to:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/2e4375ed-f738-4197-b049-fa7e5c236e81" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can use EventCode ID 3 to search for the source IP address of the Malware:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/ea2309f0-2436-4005-8b48-cdb5469de3a9" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can as well see the IP address that the malicious process tried to reach out to:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/62bc24ad-6218-411d-b3ab-bbb7ac1dd50a" height="80%" width="80%"/>


<br />
<br />

<p align="center">
 We can use EventCode ID 3 'index="sysmonsplunklab" Preventivo24.02.14.exe.exe EventCode=3' to search for the source IP address of the Malware:<br/>
<img alt="image" src="https://github.com/user-attachments/assets/ea2309f0-2436-4005-8b48-cdb5469de3a9" height="80%" width="80%"/>


<br />
<br />
