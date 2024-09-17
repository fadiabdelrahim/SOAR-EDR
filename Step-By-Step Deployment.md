# <p align="center"> SOAR-EDR Project <br> Step-by-Step Implementation Guide

## Introduction
This document provides a comprehensive, step-by-step guide for deploying and configuring a Security Orchestration, Automation, and Response (SOAR) system integrated with Endpoint Detection and Response (EDR) using LimaCharlie, Slack, and Tines. The purpose of this guide is to help users implement a seamless detection and automated response workflow, enabling real-time monitoring, alerting, and machine isolation during potential security incidents.

The setup focuses on deploying a virtual environment, configuring the necessary EDR tools, generating telemetry, and integrating Slack for notifications and Tines for workflow automation. Each step is carefully outlined with detailed instructions and illustrations to ensure a successful implementation.

## Table of Contents

- Part 1: Diagram (playbook workflow)
- Part 2: LimaCharlie
- Part 3: Telemetry
- Part 4: Salck & Tines
- Part 5: Automation

## Step-by-Step Implementation Guide

### Step 1: Diagram
- Create playbook workflow
  - Send a Slack message containing information about the detection
  - Send an Email containing information about the detection
  - Generate a user prompt
    - Isolate the machine? (Yes/No)
    - If Yes – Isolate
   
Create playbook workflow

<p align="center"><img src="images/SOAR EDR Project.png"></p>

### Step 2: Deploy Virtual server and setup LimaCharlie

Deploy Server
<p align="center"><img src="images/Picture1.png"></p>

Select create Azure virtual machine
<p align="center"><img src="images/Picture2.png"></p>

Create new resource group 
<p align="center"><img src="images/Picture3.png"></p>

Enter name for virtual machine
<p align="center"><img src="images/Picture4.png"></p>

Select Region
<p align="center"><img src="images/Picture5.png"></p>

Select Image 
<p align="center"><img src="images/Picture6.png"></p>

Select virtual machine size
<p align="center"><img src="images/Picture7.png"></p>

Enter a username and password
<p align="center"><img src="images/Picture8.png"></p>

Select inbound ports and review and create
<p align="center"><img src="images/Picture9.png"></p>

Select Create
<p align="center"><img src="images/Picture10.png"></p>
<p align="center"><img src="images/Picture11.png"></p>

RDP into new created vm
<p align="center"><img src="images/Picture12.png"></p>
<p align="center"><img src="images/Picture13.png"></p>

Visit limacharlie.io and create account and then select Create Organization
<p align="center"><img src="images/Picture14.png"></p>

Create name for organization and select region then select Create Organization
<p align="center"><img src="images/Picture15.png"></p>

Select Installation Keys
<p align="center"><img src="images/Picture16.png"></p>

Select Create Installation Keys
<p align="center"><img src="images/Picture17.png"></p>

Enter description name and select create
<p align="center"><img src="images/Picture18.png"></p>

Remove all other installation keys 
<p align="center"><img src="images/Picture19.png"></p>
<p align="center"><img src="images/Picture20.png"></p>

Select Windows 64 bit sensor download and paste in windows vm browser to download EDR
<p align="center"><img src="images/Picture21.png"></p>

Paste the link in URL and select enter to download
<p align="center"><img src="images/Picture22.png"></p>
<p align="center"><img src="images/Picture23.png"></p>
<p align="center"><img src="images/Picture24.png"></p>

Copy sensor key from LimaCharlie
<p align="center"><img src="images/Picture25.png"></p>

In windows vm open PowerShell as administrator
<p align="center"><img src="images/Picture26.png"></p>

Navigate to downloads directory
<p align="center"><img src="images/Picture27.png"></p>

Next, we will run the executable -i and paste our sensor key we copied and hit enter
<p align="center"><img src="images/Picture28.png"></p>
<p align="center"><img src="images/Picture29.png"></p>

Head over to services and search for LimaCharlie to verify successful installation
<p align="center"><img src="images/Picture30.png"></p>
<p align="center"><img src="images/Picture31.png"></p>

Navigate back to LimaCharlie and select sensors list and verify our computer name is displayed
<p align="center"><img src="images/Picture32.png"></p>
<p align="center"><img src="images/Picture33.png"></p>

Select the computer name and navigate to timeline to confirm LimaCharlie is generating events
<p align="center"><img src="images/Picture34.png"></p>

### Step 3: Telemetry

Generate telemetry (Lazagne-password recovery tool). We need to disable windows security to allow us to download
<p align="center"><img src="images/Picture35.png"></p>

Select Virus & threat protection
<p align="center"><img src="images/Picture36.png"></p>

Select manage settings
<p align="center"><img src="images/Picture37.png"></p>

Turn off real-time protection
<p align="center"><img src="images/Picture38.png"></p>

Navigate to LaZagne github and download the latest release 
<p align="center"><img src="images/Picture39.png"></p>
<p align="center"><img src="images/Picture40.png"></p>

Select the file and allow download 
<p align="center"><img src="images/Picture41.png"></p>
<p align="center"><img src="images/Picture42.png"></p>

View Downloads folder to verify download was successful 
<p align="center"><img src="images/Picture43.png"></p>

Open PowerShell from the Downloads directory by pressing shift and right click and selecting open PowerShell window here
<p align="center"><img src="images/Picture44.png"></p>

Run the lazagne executable to verify it works
<p align="center"><img src="images/Picture45.png"></p>

Navigate to LimaCharlie to see that it picked up the process
<p align="center"><img src="images/Picture46.png"></p>
<p align="center"><img src="images/Picture47.png"></p>

Select the first NEW_PROCESS to open event view
<p align="center"><img src="images/Picture48.png"></p>

Next, create the detection and response rule. Select back to sensors 
<p align="center"><img src="images/Picture49.png"></p>

Navigate to Automation and select D&R Rules
<p align="center"><img src="images/Picture50.png"></p>

Select Create Custom Rule
<p align="center"><img src="images/Picture51.png"></p>

Create a detect and respond rule for lazagne
<p align="center"><img src="images/Picture52.png"></p>
<p align="center"><img src="images/Picture53.png"></p>

Save the Rule
<p align="center"><img src="images/Picture54.png"></p>
<p align="center"><img src="images/Picture55.png"></p>

Select Target Event to test the new rule
<p align="center"><img src="images/Picture56.png"></p>

Copy the entire NEW_PROCESS event for lazagne from the timeline and paste it in target event and select test event to test the new rule
<p align="center"><img src="images/Picture57.png"></p>

The new rule has successfully evaluated the 4 operations from the detect rules created
<p align="center"><img src="images/Picture58.png"></p>

Verify the detection is working. Navigate to the detection tab
<p align="center"><img src="images/Picture59.png"></p>

Select detection 
<p align="center"><img src="images/Picture60.png"></p>

Navigate back to windows vm and run lazagne all again
<p align="center"><img src="images/Picture61.png"></p>

Navigate back to LimaCharlie detection to verify (wait a few minutes and refresh the page to display)
<p align="center"><img src="images/Picture62.png"></p>
<p align="center"><img src="images/Picture63.png"></p>

### Step 4: Slack & Tines

Create a new channel in slack named alerts
<p align="center"><img src="images/Picture64.png"></p>
<p align="center"><img src="images/Picture65.png"></p>
<p align="center"><img src="images/Picture66.png"></p>
<p align="center"><img src="images/Picture67.png"></p>

Navigate to Tines and setup the link between LimaCharliie and Tines. Select Webhook and drag into story
<p align="center"><img src="images/Picture68.png"></p>

Enter name for webhook
<p align="center"><img src="images/Picture69.png"></p>

Enter description
<p align="center"><img src="images/Picture70.png"></p>

Copy the webhook URL 
<p align="center"><img src="images/Picture71.png"></p>

Navigate to LimaCharlie and select outputs
<p align="center"><img src="images/Picture72.png"></p>

Select add output
<p align="center"><img src="images/Picture73.png"></p>

Select detections
<p align="center"><img src="images/Picture74.png"></p>

Select Tines
<p align="center"><img src="images/Picture75.png"></p>

Enter name
<p align="center"><img src="images/Picture76.png"></p>

Enter the copied webhook URL from Tines in the Destination Host 
<p align="center"><img src="images/Picture77.png"></p>

Select save output
<p align="center"><img src="images/Picture78.png"></p>
<p align="center"><img src="images/Picture79.png"></p>

Navigate to windows vm and execute lazagne to verify the SOAR-EDR configuration is working
<p align="center"><img src="images/Picture80.png"></p>

Navigate to LimaCharlie and select Refresh Samples to verify the detection has generated
<p align="center"><img src="images/Picture81.png"></p>
<p align="center"><img src="images/Picture82.png"></p>

Navigate to Tines and select Events from the webhook
<p align="center"><img src="images/Picture83.png"></p>

Select the most recent detection
<p align="center"><img src="images/Picture84.png"></p>

Expand retrieve_detection
<p align="center"><img src="images/Picture85.png"></p>

Expand body to verify detection is displayed in Tines
<p align="center"><img src="images/Picture86.png"></p>
<p align="center"><img src="images/Picture87.png"></p>

### Step 5: Automation

Navigate to Slack to create a link to Tines
<p align="center"><img src="images/Picture88.png"></p>

Select More 
<p align="center"><img src="images/Picture89.png"></p>

Select Automations
<p align="center"><img src="images/Picture90.png"></p>

Select Apps
<p align="center"><img src="images/Picture91.png"></p>

Search for Tines
<p align="center"><img src="images/Picture92.png"></p>

Select Add
<p align="center"><img src="images/Picture93.png"></p>

Select Add to Slack
<p align="center"><img src="images/Picture94.png"></p>

Navigate to tines and select Dashboard
<p align="center"><img src="images/Picture95.png"></p>

Select team
<p align="center"><img src="images/Picture96.png"></p>

Select Credentials
<p align="center"><img src="images/Picture97.png"></p>

Select New
<p align="center"><img src="images/Picture98.png"></p>

Select Slack
<p align="center"><img src="images/Picture99.png"></p>

Select use Tines app for Slack
<p align="center"><img src="images/Picture100.png"></p>

Select Allow
<p align="center"><img src="images/Picture101.png"></p>
<p align="center"><img src="images/Picture102.png"></p>

Navigate back to story
<p align="center"><img src="images/Picture103.png"></p>

Select story
<p align="center"><img src="images/Picture104.png"></p>

Select Templates
<p align="center"><img src="images/Picture105.png"></p>

Select Slack
<p align="center"><img src="images/Picture106.png"></p>

Search for message
<p align="center"><img src="images/Picture107.png"></p>

Select Send a message
<p align="center"><img src="images/Picture108.png"></p>

Select Your first story and change the default name of story
<p align="center"><img src="images/Picture109.png"></p>
<p align="center"><img src="images/Picture110.png"></p>

Select Dashboard 
<p align="center"><img src="images/Picture111.png"></p>

Select show story actions
<p align="center"><img src="images/Picture112.png"></p>

Select Move
<p align="center"><img src="images/Picture113.png"></p>

Select team
<p align="center"><img src="images/Picture114.png"></p>

Select Move
<p align="center"><img src="images/Picture115.png"></p>

Select Done
<p align="center"><img src="images/Picture116.png"></p>

Select team
<p align="center"><img src="images/Picture117.png"></p>

Select SOAR-EDR
<p align="center"><img src="images/Picture118.png"></p>

Select Slack
<p align="center"><img src="images/Picture119.png"></p>

Navigate to Slack to retrieve the Alerts channel ID
<p align="center"><img src="images/Picture120.png"></p>

Right click on alerts and select view channel details
<p align="center"><img src="images/Picture121.png"></p>

Copy the Channel ID
<p align="center"><img src="images/Picture122.png"></p>

Navigate to Tines and paste Channel ID in Channel/User ID
<p align="center"><img src="images/Picture123.png"></p>

Enter message to display
<p align="center"><img src="images/Picture124.png"></p>

Select Connect to Slack
<p align="center"><img src="images/Picture125.png"></p>

Select Slack
<p align="center"><img src="images/Picture126.png"></p>

Make a connection from the webhook to Slack
<p align="center"><img src="images/Picture127.png"></p>

Select Slack and select run to test
<p align="center"><img src="images/Picture128.png"></p>

Navigate to Slack and check alerts channel to verify connection is successful 
<p align="center"><img src="images/Picture129.png"></p>

Navigate to Tines and select Send Email and insert in story
<p align="center"><img src="images/Picture130.png"></p>

Connect Webhook to Send Email
<p align="center"><img src="images/Picture131.png"></p>

Select Send Email
<p align="center"><img src="images/Picture132.png"></p>

Enter Description
<p align="center"><img src="images/Picture133.png"></p>

Enter Email address
<p align="center"><img src="images/Picture134.png"></p>

Enter Sender Name
<p align="center"><img src="images/Picture135.png"></p>

Enter a Subject
<p align="center"><img src="images/Picture136.png"></p>

Select Test
<p align="center"><img src="images/Picture137.png"></p>

Select the most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture138.png"></p>

Navigate to email inbox and verify email was sent 
<p align="center"><img src="images/Picture139.png"></p>
<p align="center"><img src="images/Picture140.png"></p>

Navigate to Tines and select tools and insert page into story
<p align="center"><img src="images/Picture141.png"></p>

Enter Name
<p align="center"><img src="images/Picture142.png"></p>

Enter Description
<p align="center"><img src="images/Picture143.png"></p>

Enter Success message
<p align="center"><img src="images/Picture144.png"></p>

Link the Webhook to the User Prompt
<p align="center"><img src="images/Picture145.png"></p>

Select Edit page
<p align="center"><img src="images/Picture146.png"></p>

Select My new page 
<p align="center"><img src="images/Picture147.png"></p>

Enter name in contents
<p align="center"><img src="images/Picture148.png"></p>

Select the message to edit
<p align="center"><img src="images/Picture149.png"></p>

Enter message 
<p align="center"><img src="images/Picture150.png"></p>

Select Boolean and insert into User Prompt
<p align="center"><img src="images/Picture151.png"></p>

Select Slack
<p align="center"><img src="images/Picture152.png"></p>

Edit Message to include the details from our playbook diagram
<p align="center"><img src="images/Picture153.png"></p>
<p align="center"><img src="images/Picture154.png"></p>

Select Test
<p align="center"><img src="images/Picture155.png"></p>

Select most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture156.png"></p>

Navigate to Slack alerts channel to verify the message has been received
<p align="center"><img src="images/Picture157.png"></p>

Navigate to Tines and select Send Email
<p align="center"><img src="images/Picture158.png"></p>

Edit Body to include the details from our playbook diagram 
<p align="center"><img src="images/Picture159.png"></p>
<p align="center"><img src="images/Picture160.png"></p>

Select Test
<p align="center"><img src="images/Picture161.png"></p>

Select most recent Retrieve Detection and select test
<p align="center"><img src="images/Picture162.png"></p>

Navigate to email inbox to verify email was received
<p align="center"><img src="images/Picture163.png"></p>
<p align="center"><img src="images/Picture164.png"></p>

Select User Prompt 
<p align="center"><img src="images/Picture165.png"></p>

Select Edit
<p align="center"><img src="images/Picture166.png"></p>

Select the message and edit to include the details from our playbook diagram
<p align="center"><img src="images/Picture167.png"></p>
<p align="center"><img src="images/Picture168.png"></p>
<p align="center"><img src="images/Picture169.png"></p>

Select Boolean and edit 
<p align="center"><img src="images/Picture170.png"></p>
<p align="center"><img src="images/Picture171.png"></p>

Select back
<p align="center"><img src="images/Picture172.png"></p>

Select Visit page to test
<p align="center"><img src="images/Picture173.png"></p>

Select most recent event
<p align="center"><img src="images/Picture174.png"></p>
<p align="center"><img src="images/Picture175.png"></p>

Select No and select submit
<p align="center"><img src="images/Picture176.png"></p>

The submission was successful. Close this window 
<p align="center"><img src="images/Picture177.png"></p>

Select Trigger and insert into story
<p align="center"><img src="images/Picture178.png"></p>

Edit name
<p align="center"><img src="images/Picture179.png"></p>

Edit Description
<p align="center"><img src="images/Picture180.png"></p>

Link the User Prompt to Trigger NO
<p align="center"><img src="images/Picture181.png"></p>

Select rules to edit
<p align="center"><img src="images/Picture182.png"></p>

Delete the current rule
<p align="center"><img src="images/Picture183.png"></p>

Select user_prompt
<p align="center"><img src="images/Picture184.png"></p>

Select body
<p align="center"><img src="images/Picture185.png"></p>

Select isolate
<p align="center"><img src="images/Picture186.png"></p>

Edit is equal to false
<p align="center"><img src="images/Picture187.png"></p>

Copy the Slack message 
<p align="center"><img src="images/Picture188.png"></p>

Paste in story using ctrl-v
<p align="center"><img src="images/Picture189.png"></p>

Select new Slack message and edit the description
<p align="center"><img src="images/Picture190.png"></p>
<p align="center"><img src="images/Picture191.png"></p>

Link the NO trigger to Slack
<p align="center"><img src="images/Picture192.png"></p>

Test the NO trigger. Select Webhook
<p align="center"><img src="images/Picture193.png"></p>

Select Events
<p align="center"><img src="images/Picture194.png"></p>

Select most recent event and select Re-emit
<p align="center"><img src="images/Picture195.png"></p>

Select User Prompt 
<p align="center"><img src="images/Picture196.png"></p>

Select visit page and select most recent event 
<p align="center"><img src="images/Picture197.png"></p>

Select No and select submit
<p align="center"><img src="images/Picture198.png"></p>

Close the window
<p align="center"><img src="images/Picture199.png"></p>

Navigate to Slack and verify the response 
<p align="center"><img src="images/Picture200.png"></p>

Navigate to Tines and copy the No Trigger
<p align="center"><img src="images/Picture201.png"></p>

Paste in story using ctrl-v
<p align="center"><img src="images/Picture202.png"></p>

Select Webhook 
<p align="center"><img src="images/Picture203.png"></p>

Select Events
<p align="center"><img src="images/Picture204.png"></p>

Select the most recent event and select Re-emit
<p align="center"><img src="images/Picture205.png"></p>

Select User Prompt
<p align="center"><img src="images/Picture206.png"></p>

Select visit page and select the most recent event
<p align="center"><img src="images/Picture207.png"></p>

Select Yes and Submit
<p align="center"><img src="images/Picture208.png"></p>

Close the window
<p align="center"><img src="images/Picture209.png"></p>

Select the copied No trigger
<p align="center"><img src="images/Picture210.png"></p>

Edit the Name
<p align="center"><img src="images/Picture211.png"></p>

Edit the Rules to is equal to true
<p align="center"><img src="images/Picture212.png"></p>

Link the Yes Trigger to the User Prompt
<p align="center"><img src="images/Picture213.png"></p>

Select Templates and search LimaCharlie
<p align="center"><img src="images/Picture214.png"></p>

Select LimaCharlie and Inset into the story
<p align="center"><img src="images/Picture215.png"></p>

Search isolate
<p align="center"><img src="images/Picture216.png"></p>

Select Isolate Sensor 
<p align="center"><img src="images/Picture217.png"></p>

Edit the URL to include the sid 
<p align="center"><img src="images/Picture218.png"></p>
<p align="center"><img src="images/Picture219.png"></p>

Link the YES Trigger to the HTTP Request Isolate Sensor
<p align="center"><img src="images/Picture220.png"></p>

Navigate to LimaCharlie and select Access Management and select REST API
<p align="center"><img src="images/Picture221.png"></p>

Copy the Org JWT
<p align="center"><img src="images/Picture222.png"></p>

Navigate to Tines and select dashboard
<p align="center"><img src="images/Picture223.png"></p>

Select Credentials
<p align="center"><img src="images/Picture224.png"></p>

Select New and select Text
<p align="center"><img src="images/Picture225.png"></p>

Enter name 
<p align="center"><img src="images/Picture226.png"></p>

Enter Description
<p align="center"><img src="images/Picture227.png"></p>

Paste the Org JWT from LimaCharlie in Value
<p align="center"><img src="images/Picture228.png"></p>

Enter URLs and Domains 
<p align="center"><img src="images/Picture229.png"></p>

Select Save
<p align="center"><img src="images/Picture230.png"></p>

Navigate back to story
<p align="center"><img src="images/Picture231.png"></p>

Select Connect and select LimaCharlie
<p align="center"><img src="images/Picture232.png"></p>

Select Templates and search LimaCharlie
<p align="center"><img src="images/Picture233.png"></p>

Select LimaCharlie and insert into story
<p align="center"><img src="images/Picture234.png"></p>

Select Get Isolation Status
<p align="center"><img src="images/Picture235.png"></p>

Link the HTTP Request Isolate Sensor to HTTP Request Get Isolation Status
<p align="center"><img src="images/Picture236.png"></p>

Edit the URL for the HTTP Request for the Get Isolation Status to match our playbook
<p align="center"><img src="images/Picture237.png"></p>
<p align="center"><img src="images/Picture238.png"></p>

Select Connect and select LimaCharlie 
<p align="center"><img src="images/Picture239.png"></p>

Select Slack and copy the send a message
<p align="center"><img src="images/Picture240.png"></p>

Paste in story using ctrl-v
<p align="center"><img src="images/Picture241.png"></p>

Link the HTTP Request Get Isolation message to the new Slack send a message
<p align="center"><img src="images/Picture242.png"></p>

Select Slack send a message and edit the message
<p align="center"><img src="images/Picture243.png"></p>
<p align="center"><img src="images/Picture244.png"></p>

Navigate to LimaCharlie and verify Network Access is Allowed
<p align="center"><img src="images/Picture245.png"></p>

Navigate to Tines to test automated isolation. Select Webhook Retrieve Detection 
<p align="center"><img src="images/Picture246.png"></p>

Select Events 
<p align="center"><img src="images/Picture247.png"></p>

Select the most recent Event and select Re-emit
<p align="center"><img src="images/Picture248.png"></p>

Navigate to Slack to verify message has been received
<p align="center"><img src="images/Picture249.png"></p>

Select User Prompt
<p align="center"><img src="images/Picture250.png"></p>

Select Visit Page
<p align="center"><img src="images/Picture251.png"></p>

Select the most recent event
<p align="center"><img src="images/Picture252.png"></p>

Select Yes and Submit
<p align="center"><img src="images/Picture253.png"></p>

Close the window
<p align="center"><img src="images/Picture254.png"></p>

Navigate to Navigate to LimaCharlie to verify the network has been isolated
<p align="center"><img src="images/Picture255.png"></p>

Navigate to Slack and verify the status message was received
<p align="center"><img src="images/Picture256.png"></p>















