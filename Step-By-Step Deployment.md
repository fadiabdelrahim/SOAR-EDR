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








