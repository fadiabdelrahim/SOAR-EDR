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

Step 1: Diagram
- Create playbook workflow
  - Send a Slack message containing information about the detection
  - Send an Email containing information about the detection
  - Generate a user prompt
    - Isolate the machine? (Yes/No)
    - If Yes – Isolate
   
Create playbook workflow
