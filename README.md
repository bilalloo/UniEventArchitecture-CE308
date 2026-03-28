# UniEvent: Scalable University Event Management System
**Ghulam Ishaq Khan Institute of Engineering Sciences and Technology**
**Course:** CE 308/408 Cloud Computing - Assignment 1

## Project Overview
UniEvent is a cloud-hosted web application designed to act as a centralized platform for students to browse university events, register for activities, and view event-related media. To eliminate manual data entry, the system automatically fetches event data from an external Open API, treating them as official university events. 

The system architecture is explicitly designed to be secure, scalable, and fault-tolerant, ensuring high availability even during peak registration periods and potential instance failures.

## Repository Contents
* **Web Files & Source Code:** Python Flask application (`app.py`), HTML templates, and dependency requirements required to run the event fetching logic.
* **Architecture Evidence:** Screenshots detailing the configuration of the VPC, IAM roles, S3 buckets, EC2 instances, Session Manager terminal connections, and the live Application Load Balancer.

## Live Access
* **Application Load Balancer URL:** `UniEvent-ALB-1619398761.eu-north-1.elb.amazonaws.com`

## Architecture & Application Integration
This project utilizes a complete AWS-based architecture, seamlessly integrated with a custom Python backend.

* **Virtual Private Cloud (VPC):** A custom VPC was deployed across two Availability Zones (AZs) to provide network isolation and foundational high availability.
* **Elastic Compute Cloud (EC2):** The Python web application runs on multiple Amazon Linux 2023 EC2 instances located strictly inside private subnets. This enhances security by preventing direct internet access to the servers.
* **Python to AWS Connection:** The backend is built using **Python (Flask)**. It is deployed directly onto the EC2 instances by pulling the source code from this GitHub repository via AWS Systems Manager (Session Manager). The application runs continuously in the background on Port 80, listening for incoming web requests.
* **Elastic Load Balancing (ELB):** An internet-facing Application Load Balancer sits in the public subnets. It securely receives student traffic and routes it to the Python Flask application running on Port 80 of the healthy EC2 instances in the private subnets.
* **Identity and Access Management (IAM):** An IAM Role with `AmazonSSMManagedInstanceCore` and S3 access policies is attached directly to the EC2 instances. This allows secure, browser-based terminal access for code deployment and allows the servers to securely interact with AWS services without utilizing hardcoded credentials.

## Open API Evaluation & Connection
To fulfill the requirement of fetching external data, the **Ticketmaster Discovery API** was selected and integrated into the Python backend.

* **Evaluation:** The Ticketmaster API is a highly reliable open events API that provides structured JSON data. It comprehensively fulfills the data requirements by providing the event title, date, venue, description, and event posters.
* **API Connection Strategy:** The Python application utilizes the `requests` library to securely connect to the Ticketmaster API endpoint using a private developer key. 
* **Data Processing:** Upon receiving a web request, the Python backend dynamically fetches the live JSON payload from Ticketmaster, parses the hierarchical data to extract event titles, dates, and image URLs, and passes that structured data into an HTML template (`index.html`) to be rendered for the user.

## Step-by-Step Deployment Guide
1. **Networking:** Created a VPC with 2 public subnets (for the ALB) and 2 private subnets (for the EC2 instances).
2. **Security:** Configured an IAM Role with Systems Manager/S3 permissions and created a Security Group allowing HTTP (Port 80) traffic.
3. **Compute:** Launched two EC2 instances in separate private subnets, attaching the IAM role and Security Group.
4. **Load Balancing:** Deployed an Application Load Balancer across the public subnets, routing traffic to a Target Group containing the private EC2 instances.
5. **Code Deployment:** Used AWS Session Manager to securely connect to the private instances. Installed Git and Python, cloned the application repository, and launched the Flask app on Port 80 using `nohup` to ensure continuous background execution.
