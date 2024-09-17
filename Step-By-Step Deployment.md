# <p align="center"> SOAR-EDR Project <br> Step-by-Step Implementation Guide

## Introduction
This document provides a comprehensive, step-by-step guide for deploying and configuring a Security Orchestration, Automation, and Response (SOAR) system integrated with Endpoint Detection and Response (EDR) using LimaCharlie, Slack, and Tines. The purpose of this guide is to help users implement a seamless detection and automated response workflow, enabling real-time monitoring, alerting, and machine isolation during potential security incidents.

The setup focuses on deploying a virtual environment, configuring the necessary EDR tools, generating telemetry, and integrating Slack for notifications and Tines for workflow automation. Each step is carefully outlined with detailed instructions and illustrations to ensure a successful implementation.

## Table of Contents

- [Part 1: Diagram (playbook workflow)](#Part-1-Diagram)
- [Part 2: Deploy Virtual server and setup LimaCharlie](#Part-2-Deploy-Virtual-server-and-setup-LimaCharlie)
- [Part 3: Telemetry](#Part-3-Telemetry)
- [Part 4: Salck & Tines](#Part-4-Salck-&-Tines)
- [Part 5: Automation](#Part-5-Automation)

## Step-by-Step Implementation Guide

### Part 1: Diagram
- Create playbook workflow
  - Send a Slack message containing information about the detection
  - Send an Email containing information about the detection
  - Generate a user prompt
    - Isolate the machine? (Yes/No)
    - If Yes – Isolate
   
<p align="center"> Create playbook workflow

<p align="center"><img src="images/SOAR EDR Project.png"></p>

### Part 2: Deploy Virtual server and setup LimaCharlie
- Deploy Virtual Server
- Install and setup LimaCharlie
- Confirm Events

<p align="center"> Deploy Server
<p align="center"><img src="images/Picture1.png"></p>

<p align="center"> Select create Azure virtual machine
<p align="center"><img src="images/Picture2.png"></p>

<p align="center"> Create new resource group 
<p align="center"><img src="images/Picture3.png"></p>

<p align="center"> Enter name for virtual machine
<p align="center"><img src="images/Picture4.png"></p>

<p align="center"> Select Region
<p align="center"><img src="images/Picture5.png"></p>

<p align="center"> Select Image 
<p align="center"><img src="images/Picture6.png"></p>

<p align="center"> Select virtual machine size
<p align="center"><img src="images/Picture7.png"></p>

<p align="center"> Enter a username and password
<p align="center"><img src="images/Picture8.png"></p>

<p align="center"> Select inbound ports and review and create
<p align="center"><img src="images/Picture9.png"></p>

<p align="center"> Select Create
<p align="center"><img src="images/Picture10.png"></p>
<p align="center"><img src="images/Picture11.png"></p>

<p align="center"> RDP into new created vm
<p align="center"><img src="images/Picture12.png"></p>
<p align="center"><img src="images/Picture13.png"></p>

<p align="center"> Visit limacharlie.io and create account and then select Create Organization
<p align="center"><img src="images/Picture14.png"></p>

<p align="center"> Create name for organization and select region then select Create Organization
<p align="center"><img src="images/Picture15.png"></p>

<p align="center"> Select Installation Keys
<p align="center"><img src="images/Picture16.png"></p>

<p align="center"> Select Create Installation Keys
<p align="center"><img src="images/Picture17.png"></p>

<p align="center"> Enter description name and select create
<p align="center"><img src="images/Picture18.png"></p>

<p align="center"> Remove all other installation keys 
<p align="center"><img src="images/Picture19.png"></p>
<p align="center"><img src="images/Picture20.png"></p>

<p align="center"> Select Windows 64 bit sensor download and paste in windows vm browser to download EDR
<p align="center"><img src="images/Picture21.png"></p>

<p align="center"> Paste the link in URL and select enter to download
<p align="center"><img src="images/Picture22.png"></p>
<p align="center"><img src="images/Picture23.png"></p>
<p align="center"><img src="images/Picture24.png"></p>

<p align="center"> Copy sensor key from LimaCharlie
<p align="center"><img src="images/Picture25.png"></p>

<p align="center"> In windows vm open PowerShell as administrator
<p align="center"><img src="images/Picture26.png"></p>

<p align="center"> Navigate to downloads directory
<p align="center"><img src="images/Picture27.png"></p>

<p align="center"> Run the executable -i and paste our sensor key we copied and hit enter
<p align="center"><img src="images/Picture28.png"></p>
<p align="center"><img src="images/Picture29.png"></p>

<p align="center"> Navigate to services and search for LimaCharlie to verify successful installation
<p align="center"><img src="images/Picture30.png"></p>
<p align="center"><img src="images/Picture31.png"></p>

<p align="center"> Navigate back to LimaCharlie and select sensors list and verify our computer name is displayed
<p align="center"><img src="images/Picture32.png"></p>
<p align="center"><img src="images/Picture33.png"></p>

<p align="center"> Select the computer name and navigate to timeline to confirm LimaCharlie is generating events
<p align="center"><img src="images/Picture34.png"></p>

### Part 3: Telemetry
- Generate telemetry (Lazagne)
- Create detection and response rule

<p align="center"> Generate telemetry (Lazagne-password recovery tool). Disable windows security to allow download
<p align="center"><img src="images/Picture35.png"></p>

<p align="center"> Select Virus & threat protection
<p align="center"><img src="images/Picture36.png"></p>

<p align="center"> Select manage settings
<p align="center"><img src="images/Picture37.png"></p>

<p align="center"> Turn off real-time protection
<p align="center"><img src="images/Picture38.png"></p>

<p align="center"> Navigate to LaZagne github and download the latest release 
<p align="center"><img src="images/Picture39.png"></p>
<p align="center"><img src="images/Picture40.png"></p>

<p align="center"> Select the file and allow download 
<p align="center"><img src="images/Picture41.png"></p>
<p align="center"><img src="images/Picture42.png"></p>

<p align="center"> View Downloads folder to verify download was successful 
<p align="center"><img src="images/Picture43.png"></p>

<p align="center"> Open PowerShell from the Downloads directory by pressing shift and right click and selecting open PowerShell window here
<p align="center"><img src="images/Picture44.png"></p>

<p align="center"> Run the lazagne executable to verify it works
<p align="center"><img src="images/Picture45.png"></p>

<p align="center"> Navigate to LimaCharlie to see that it picked up the process
<p align="center"><img src="images/Picture46.png"></p>
<p align="center"><img src="images/Picture47.png"></p>

<p align="center"> Select the first NEW_PROCESS to open event view
<p align="center"><img src="images/Picture48.png"></p>

<p align="center"> Next, create the detection and response rule. Select back to sensors 
<p align="center"><img src="images/Picture49.png"></p>

<p align="center"> Navigate to Automation and select D&R Rules
<p align="center"><img src="images/Picture50.png"></p>

<p align="center"> Select Create Custom Rule
<p align="center"><img src="images/Picture51.png"></p>

<p align="center"> Create a detect and respond rule for lazagne
<p align="center"><img src="images/Picture52.png"></p>
<p align="center"><img src="images/Picture53.png"></p>

<p align="center"> Save the Rule
<p align="center"><img src="images/Picture54.png"></p>
<p align="center"><img src="images/Picture55.png"></p>

<p align="center"> Select Target Event to test the new rule
<p align="center"><img src="images/Picture56.png"></p>

<p align="center"> Copy the entire NEW_PROCESS event for lazagne from the timeline and paste it in target event and select test event to test the new rule
<p align="center"><img src="images/Picture57.png"></p>

<p align="center"> The new rule has successfully evaluated the 4 operations from the detect rules created
<p align="center"><img src="images/Picture58.png"></p>

<p align="center"> Verify the detection is working. Navigate to the detection tab
<p align="center"><img src="images/Picture59.png"></p>

<p align="center"> Select detection 
<p align="center"><img src="images/Picture60.png"></p>

<p align="center"> Navigate back to windows vm and run lazagne all again
<p align="center"><img src="images/Picture61.png"></p>

<p align="center"> Navigate back to LimaCharlie detection to verify (wait a few minutes and refresh the page to display)
<p align="center"><img src="images/Picture62.png"></p>
<p align="center"><img src="images/Picture63.png"></p>

### Part 4: Slack & Tines
- Setup Slack and Tines
- Test Connection (LimaCharlie & Tines)

<p align="center"> Create a new channel in slack named alerts
<p align="center"><img src="images/Picture64.png"></p>
<p align="center"><img src="images/Picture65.png"></p>
<p align="center"><img src="images/Picture66.png"></p>
<p align="center"><img src="images/Picture67.png"></p>

<p align="center"> Navigate to Tines and setup the link between LimaCharliie and Tines. Select Webhook and drag into story
<p align="center"><img src="images/Picture68.png"></p>

<p align="center"> Enter name for webhook
<p align="center"><img src="images/Picture69.png"></p>

<p align="center"> Enter description
<p align="center"><img src="images/Picture70.png"></p>

<p align="center"> Copy the webhook URL 
<p align="center"><img src="images/Picture71.png"></p>

<p align="center"> Navigate to LimaCharlie and select outputs
<p align="center"><img src="images/Picture72.png"></p>

<p align="center"> Select add output
<p align="center"><img src="images/Picture73.png"></p>

<p align="center"> Select detections
<p align="center"><img src="images/Picture74.png"></p>

<p align="center"> Select Tines
<p align="center"><img src="images/Picture75.png"></p>

<p align="center"> Enter name
<p align="center"><img src="images/Picture76.png"></p>

<p align="center"> Enter the copied webhook URL from Tines in the Destination Host 
<p align="center"><img src="images/Picture77.png"></p>

<p align="center"> Select save output
<p align="center"><img src="images/Picture78.png"></p>
<p align="center"><img src="images/Picture79.png"></p>

<p align="center"> Navigate to windows vm and execute lazagne to verify the SOAR-EDR configuration is working
<p align="center"><img src="images/Picture80.png"></p>

<p align="center"> Navigate to LimaCharlie and select Refresh Samples to verify the detection has generated
<p align="center"><img src="images/Picture81.png"></p>
<p align="center"><img src="images/Picture82.png"></p>

<p align="center"> Navigate to Tines and select Events from the webhook
<p align="center"><img src="images/Picture83.png"></p>

<p align="center"> Select the most recent detection
<p align="center"><img src="images/Picture84.png"></p>

<p align="center"> Expand retrieve_detection
<p align="center"><img src="images/Picture85.png"></p>

<p align="center"> Expand body to verify detection is displayed in Tines
<p align="center"><img src="images/Picture86.png"></p>
<p align="center"><img src="images/Picture87.png"></p>

### Part 5: Automation
- Create automation for playbook workflow
- Test the automation response for network isolation

<p align="center"> Navigate to Slack to create a link to Tines
<p align="center"><img src="images/Picture88.png"></p>

<p align="center"> Select More 
<p align="center"><img src="images/Picture89.png"></p>

<p align="center"> Select Automations
<p align="center"><img src="images/Picture90.png"></p>

<p align="center"> Select Apps
<p align="center"><img src="images/Picture91.png"></p>

<p align="center"> Search for Tines
<p align="center"><img src="images/Picture92.png"></p>

<p align="center"> Select Add
<p align="center"><img src="images/Picture93.png"></p>

<p align="center"> Select Add to Slack
<p align="center"><img src="images/Picture94.png"></p>

<p align="center"> Navigate to tines and select Dashboard
<p align="center"><img src="images/Picture95.png"></p>

<p align="center"> Select team
<p align="center"><img src="images/Picture96.png"></p>

<p align="center"> Select Credentials
<p align="center"><img src="images/Picture97.png"></p>

<p align="center"> Select New
<p align="center"><img src="images/Picture98.png"></p>

<p align="center"> Select Slack
<p align="center"><img src="images/Picture99.png"></p>

<p align="center"> Select use Tines app for Slack
<p align="center"><img src="images/Picture100.png"></p>

<p align="center"> Select Allow
<p align="center"><img src="images/Picture101.png"></p>
<p align="center"><img src="images/Picture102.png"></p>

<p align="center"> Navigate back to story
<p align="center"><img src="images/Picture103.png"></p>

<p align="center"> Select story
<p align="center"><img src="images/Picture104.png"></p>

<p align="center"> Select Templates
<p align="center"><img src="images/Picture105.png"></p>

<p align="center"> Select Slack
<p align="center"><img src="images/Picture106.png"></p>

<p align="center"> Search for message
<p align="center"><img src="images/Picture107.png"></p>

<p align="center"> Select Send a message
<p align="center"><img src="images/Picture108.png"></p>

<p align="center"> Select Your first story and change the default name of story
<p align="center"><img src="images/Picture109.png"></p>
<p align="center"><img src="images/Picture110.png"></p>

<p align="center"> Select Dashboard 
<p align="center"><img src="images/Picture111.png"></p>

<p align="center"> Select show story actions
<p align="center"><img src="images/Picture112.png"></p>

<p align="center"> Select Move
<p align="center"><img src="images/Picture113.png"></p>

<p align="center"> Select team
<p align="center"><img src="images/Picture114.png"></p>

<p align="center"> Select Move
<p align="center"><img src="images/Picture115.png"></p>

<p align="center"> Select Done
<p align="center"><img src="images/Picture116.png"></p>

<p align="center"> Select team
<p align="center"><img src="images/Picture117.png"></p>

<p align="center"> Select SOAR-EDR
<p align="center"><img src="images/Picture118.png"></p>

<p align="center"> Select Slack
<p align="center"><img src="images/Picture119.png"></p>

<p align="center"> Navigate to Slack to retrieve the Alerts channel ID
<p align="center"><img src="images/Picture120.png"></p>

<p align="center"> Right click on alerts and select view channel details
<p align="center"><img src="images/Picture121.png"></p>

<p align="center"> Copy the Channel ID
<p align="center"><img src="images/Picture122.png"></p>

<p align="center"> Navigate to Tines and paste Channel ID in Channel/User ID
<p align="center"><img src="images/Picture123.png"></p>

<p align="center"> Enter message to display
<p align="center"><img src="images/Picture124.png"></p>

<p align="center"> Select Connect to Slack
<p align="center"><img src="images/Picture125.png"></p>

<p align="center"> Select Slack
<p align="center"><img src="images/Picture126.png"></p>

<p align="center"> Make a connection from the webhook to Slack
<p align="center"><img src="images/Picture127.png"></p>

<p align="center"> Select Slack and select run to test
<p align="center"><img src="images/Picture128.png"></p>

<p align="center"> Navigate to Slack and check alerts channel to verify connection is successful 
<p align="center"><img src="images/Picture129.png"></p>

<p align="center"> Navigate to Tines and select Send Email and insert in story
<p align="center"><img src="images/Picture130.png"></p>

<p align="center"> Connect Webhook to Send Email
<p align="center"><img src="images/Picture131.png"></p>

<p align="center"> Select Send Email
<p align="center"><img src="images/Picture132.png"></p>

<p align="center"> Enter Description
<p align="center"><img src="images/Picture133.png"></p>

<p align="center"> Enter Email address
<p align="center"><img src="images/Picture134.png"></p>

<p align="center"> Enter Sender Name
<p align="center"><img src="images/Picture135.png"></p>

<p align="center"> Enter a Subject
<p align="center"><img src="images/Picture136.png"></p>

<p align="center"> Select Test
<p align="center"><img src="images/Picture137.png"></p>

<p align="center"> Select the most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture138.png"></p>

<p align="center"> Navigate to email inbox and verify email was sent 
<p align="center"><img src="images/Picture139.png"></p>
<p align="center"><img src="images/Picture140.png"></p>

<p align="center"> Navigate to Tines and select tools and insert page into story
<p align="center"><img src="images/Picture141.png"></p>

<p align="center"> Enter Name
<p align="center"><img src="images/Picture142.png"></p>

<p align="center"> Enter Description
<p align="center"><img src="images/Picture143.png"></p>

<p align="center"> Enter Success message
<p align="center"><img src="images/Picture144.png"></p>

<p align="center"> Link the Webhook to the User Prompt
<p align="center"><img src="images/Picture145.png"></p>

<p align="center"> Select Edit page
<p align="center"><img src="images/Picture146.png"></p>

<p align="center"> Select My new page 
<p align="center"><img src="images/Picture147.png"></p>

<p align="center"> Enter name in contents
<p align="center"><img src="images/Picture148.png"></p>

<p align="center"> Select the message to edit
<p align="center"><img src="images/Picture149.png"></p>

<p align="center"> Enter message 
<p align="center"><img src="images/Picture150.png"></p>

<p align="center"> Select Boolean and insert into User Prompt
<p align="center"><img src="images/Picture151.png"></p>

<p align="center"> Select Slack
<p align="center"><img src="images/Picture152.png"></p>

<p align="center"> Edit Message to include the details from our playbook diagram
<p align="center"><img src="images/Picture153.png"></p>
<p align="center"><img src="images/Picture154.png"></p>

<p align="center"> Select Test
<p align="center"><img src="images/Picture155.png"></p>

<p align="center"> Select most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture156.png"></p>

<p align="center"> Navigate to Slack alerts channel to verify the message has been received
<p align="center"><img src="images/Picture157.png"></p>

<p align="center"> Navigate to Tines and select Send Email
<p align="center"><img src="images/Picture158.png"></p>

<p align="center"> Edit Body to include the details from our playbook diagram 
<p align="center"><img src="images/Picture159.png"></p>
<p align="center"><img src="images/Picture160.png"></p>

<p align="center"> Select Test
<p align="center"><img src="images/Picture161.png"></p>

<p align="center"> Select most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture162.png"></p>

<p align="center"> Navigate to email inbox to verify email was received
<p align="center"><img src="images/Picture163.png"></p>
<p align="center"><img src="images/Picture164.png"></p>

<p align="center"> Select User Prompt 
<p align="center"><img src="images/Picture165.png"></p>

<p align="center"> Select Edit
<p align="center"><img src="images/Picture166.png"></p>

<p align="center"> Select the message and edit to include the details from our playbook diagram
<p align="center"><img src="images/Picture167.png"></p>
<p align="center"><img src="images/Picture168.png"></p>
<p align="center"><img src="images/Picture169.png"></p>

<p align="center"> Select Boolean and edit 
<p align="center"><img src="images/Picture170.png"></p>
<p align="center"><img src="images/Picture171.png"></p>

<p align="center"> Select back
<p align="center"><img src="images/Picture172.png"></p>

<p align="center"> Select Visit page to test
<p align="center"><img src="images/Picture173.png"></p>

<p align="center"> Select most recent event
<p align="center"><img src="images/Picture174.png"></p>
<p align="center"><img src="images/Picture175.png"></p>

<p align="center"> Select No and select submit
<p align="center"><img src="images/Picture176.png"></p>

<p align="center"> The submission was successful. Close this window 
<p align="center"><img src="images/Picture177.png"></p>

<p align="center"> Select Trigger and insert into story
<p align="center"><img src="images/Picture178.png"></p>

<p align="center"> Edit name
<p align="center"><img src="images/Picture179.png"></p>

<p align="center"> Edit Description
<p align="center"><img src="images/Picture180.png"></p>

<p align="center"> Link the User Prompt to Trigger NO
<p align="center"><img src="images/Picture181.png"></p>

<p align="center"> Select rules to edit
<p align="center"><img src="images/Picture182.png"></p>

<p align="center"> Delete the current rule
<p align="center"><img src="images/Picture183.png"></p>

<p align="center"> Select user_prompt
<p align="center"><img src="images/Picture184.png"></p>

<p align="center"> Select body
<p align="center"><img src="images/Picture185.png"></p>

<p align="center"> Select isolate
<p align="center"><img src="images/Picture186.png"></p>

<p align="center"> Edit is equal to false
<p align="center"><img src="images/Picture187.png"></p>

<p align="center"> Copy the Slack message 
<p align="center"><img src="images/Picture188.png"></p>

<p align="center"> Paste in story using ctrl-v
<p align="center"><img src="images/Picture189.png"></p>

<p align="center"> Select new Slack message and edit the description
<p align="center"><img src="images/Picture190.png"></p>
<p align="center"><img src="images/Picture191.png"></p>

<p align="center"> Link the NO trigger to Slack
<p align="center"><img src="images/Picture192.png"></p>

<p align="center"> Test the NO trigger. Select Webhook
<p align="center"><img src="images/Picture193.png"></p>

<p align="center"> Select Events
<p align="center"><img src="images/Picture194.png"></p>

<p align="center"> Select most recent event and select Re-emit
<p align="center"><img src="images/Picture195.png"></p>

<p align="center"> Select User Prompt 
<p align="center"><img src="images/Picture196.png"></p>

<p align="center"> Select visit page and select most recent event 
<p align="center"><img src="images/Picture197.png"></p>

<p align="center"> Select No and select submit
<p align="center"><img src="images/Picture198.png"></p>

<p align="center"> Close the window
<p align="center"><img src="images/Picture199.png"></p>

<p align="center"> Navigate to Slack and verify the response 
<p align="center"><img src="images/Picture200.png"></p>

<p align="center"> Navigate to Tines and copy the No Trigger
<p align="center"><img src="images/Picture201.png"></p>

<p align="center"> Paste in story using ctrl-v
<p align="center"><img src="images/Picture202.png"></p>

<p align="center"> Select Webhook 
<p align="center"><img src="images/Picture203.png"></p>

<p align="center"> Select Events
<p align="center"><img src="images/Picture204.png"></p>

<p align="center"> Select the most recent event and select Re-emit
<p align="center"><img src="images/Picture205.png"></p>

<p align="center"> Select User Prompt
<p align="center"><img src="images/Picture206.png"></p>

<p align="center"> Select visit page and select the most recent event
<p align="center"><img src="images/Picture207.png"></p>

<p align="center"> Select Yes and Submit
<p align="center"><img src="images/Picture208.png"></p>

<p align="center"> Close the window
<p align="center"><img src="images/Picture209.png"></p>

<p align="center"> Select the copied No trigger
<p align="center"><img src="images/Picture210.png"></p>

<p align="center"> Edit the Name
<p align="center"><img src="images/Picture211.png"></p>

<p align="center"> Edit the Rules to is equal to true
<p align="center"><img src="images/Picture212.png"></p>

<p align="center"> Link the Yes Trigger to the User Prompt
<p align="center"><img src="images/Picture213.png"></p>

<p align="center"> Select Templates and search LimaCharlie
<p align="center"><img src="images/Picture214.png"></p>

<p align="center"> Select LimaCharlie and Inset into the story
<p align="center"><img src="images/Picture215.png"></p>

<p align="center"> Search isolate
<p align="center"><img src="images/Picture216.png"></p>

<p align="center"> Select Isolate Sensor 
<p align="center"><img src="images/Picture217.png"></p>

<p align="center"> Edit the URL to include the sid 
<p align="center"><img src="images/Picture218.png"></p>
<p align="center"><img src="images/Picture219.png"></p>

<p align="center"> Link the YES Trigger to the HTTP Request Isolate Sensor
<p align="center"><img src="images/Picture220.png"></p>

<p align="center"> Navigate to LimaCharlie and select Access Management and select REST API
<p align="center"><img src="images/Picture221.png"></p>

<p align="center"> Copy the Org JWT
<p align="center"><img src="images/Picture222.png"></p>

<p align="center"> Navigate to Tines and select dashboard
<p align="center"><img src="images/Picture223.png"></p>

<p align="center"> Select Credentials
<p align="center"><img src="images/Picture224.png"></p>

<p align="center"> Select New and select Text
<p align="center"><img src="images/Picture225.png"></p>

<p align="center"> Enter name 
<p align="center"><img src="images/Picture226.png"></p>

<p align="center"> Enter Description
<p align="center"><img src="images/Picture227.png"></p>

<p align="center"> Paste the Org JWT from LimaCharlie in Value
<p align="center"><img src="images/Picture228.png"></p>

<p align="center"> Enter URLs and Domains 
<p align="center"><img src="images/Picture229.png"></p>

<p align="center"> Select Save
<p align="center"><img src="images/Picture230.png"></p>

<p align="center"> Navigate back to story
<p align="center"><img src="images/Picture231.png"></p>

<p align="center"> Select Connect and select LimaCharlie
<p align="center"><img src="images/Picture232.png"></p>

<p align="center"> Select Templates and search LimaCharlie
<p align="center"><img src="images/Picture233.png"></p>

<p align="center"> Select LimaCharlie and insert into story
<p align="center"><img src="images/Picture234.png"></p>

<p align="center"> Select Get Isolation Status
<p align="center"><img src="images/Picture235.png"></p>

<p align="center"> Link the HTTP Request Isolate Sensor to HTTP Request Get Isolation Status
<p align="center"><img src="images/Picture236.png"></p>

<p align="center"> Edit the URL for the HTTP Request for the Get Isolation Status to match our playbook
<p align="center"><img src="images/Picture237.png"></p>
<p align="center"><img src="images/Picture238.png"></p>

<p align="center"> Select Connect and select LimaCharlie 
<p align="center"><img src="images/Picture239.png"></p>

<p align="center"> Select Slack and copy the send a message
<p align="center"><img src="images/Picture240.png"></p>

<p align="center"> Paste in story using ctrl-v
<p align="center"><img src="images/Picture241.png"></p>

<p align="center"> Link the HTTP Request Get Isolation message to the new Slack send a message
<p align="center"><img src="images/Picture242.png"></p>

<p align="center"> Select Slack send a message and edit the message
<p align="center"><img src="images/Picture243.png"></p>
<p align="center"><img src="images/Picture244.png"></p>

<p align="center"> Navigate to LimaCharlie and verify Network Access is Allowed
<p align="center"><img src="images/Picture245.png"></p>

<p align="center"> Navigate to Tines to test automated isolation. Select Webhook Retrieve Detection 
<p align="center"><img src="images/Picture246.png"></p>

<p align="center"> Select Events 
<p align="center"><img src="images/Picture247.png"></p>

<p align="center"> Select the most recent Event and select Re-emit
<p align="center"><img src="images/Picture248.png"></p>

<p align="center"> Navigate to Slack to verify message has been received
<p align="center"><img src="images/Picture249.png"></p>

<p align="center"> Select User Prompt
<p align="center"><img src="images/Picture250.png"></p>

<p align="center"> Select Visit Page
<p align="center"><img src="images/Picture251.png"></p>

<p align="center"> Select the most recent event
<p align="center"><img src="images/Picture252.png"></p>

<p align="center"> Select Yes and Submit
<p align="center"><img src="images/Picture253.png"></p>

<p align="center"> Close the window
<p align="center"><img src="images/Picture254.png"></p>

<p align="center"> Navigate to Navigate to LimaCharlie to verify the network has been isolated
<p align="center"><img src="images/Picture255.png"></p>

<p align="center"> Navigate to Slack and verify the status message was received
<p align="center"><img src="images/Picture256.png"></p>

## Conclusion

By following this step-by-step implementation guide, this project successfully deployed and configured a robust SOAR-EDR system using LimaCharlie, Slack, and Tines. This system is now capable of detecting potential security threats in real-time, alerting relevant personnel, and automating critical response actions such as network isolation.

The integration of Slack for notifications and Tines for workflow automation allows for seamless communication and rapid response to incidents, reducing the need for manual intervention and improving overall efficiency. Additionally, the use of LimaCharlie’s detection and response rules ensures that suspicious activities, like those simulated with the LaZagne tool, are quickly identified and mitigated.
