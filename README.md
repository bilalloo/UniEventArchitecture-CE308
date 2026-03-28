# UniEvent Cloud Architecture
CE308 Cloud Computing 
[cite_start]**Ghulam Ishaq Khan Institute of Engineering Sciences and Technology** [cite: 1, 2]
[cite_start]**Course:** CE 308/408 Cloud Computing - Assignment 1 [cite: 2, 3]

## Project Overview
UniEvent is a cloud-hosted web application designed to act as a centralized platform for students to browse university events, register for activities, and view event-related media[cite: 5]. [cite_start]To eliminate manual data entry, the system automatically fetches event data from an external Open API, treating them as official university events[cite: 8, 9, 11]. 

The system architecture is explicitly designed to be secure, scalable, and fault-tolerant, ensuring high availability even during peak registration periods and potential instance failures[cite: 6, 7, 23].

## Repository Contents
* **Web Files & Source Code:** All application source code and web files required to run the event fetching and display logic are uploaded to this repository.
* **Architecture Evidence:** Screenshots detailing the configuration of the VPC, IAM roles, S3 buckets, EC2 instances, and the Application Load Balancer are included in the repository.
* **Screenshots:** Evidence screenshots of the VPC, IAM, S3, EC2, and Load Balancer are attached in this repository.

## Live Access
* **Application Load Balancer URL:** `UniEvent-ALB-1619398761.eu-north-1.elb.amazonaws.com`

## Architecture & Service Justification
[cite_start]This project utilizes a complete AWS-based architecture leveraging IAM, VPC, EC2, S3, and Elastic Load Balancing[cite: 15].

* **Virtual Private Cloud (VPC):** A custom VPC was deployed across two Availability Zones (AZs) to provide network isolation and foundational high availability.
* [cite_start]**Elastic Compute Cloud (EC2):** The web application runs on multiple EC2 instances located strictly inside private subnets[cite: 18]. This enhances security by preventing direct internet access to the servers. [cite_start]Distributing these instances across two AZs ensures the system must continue operating even if one EC2 instance fails[cite: 23].
* **Elastic Load Balancing (ELB):** An internet-facing Application Load Balancer sits in the public subnets, securely receiving student traffic and distributing it across the healthy EC2 instances in the private subnets.
* [cite_start]**Simple Storage Service (S3):** Event posters or related images must be stored securely in S3[cite: 21]. A dedicated, private S3 bucket is used to persist these media files securely.
* **Identity and Access Management (IAM):** To maintain cloud security best practices, an IAM Role with least-privilege permissions is attached directly to the EC2 instances. This allows the servers to securely upload fetched images to S3 without utilizing hardcoded AWS credentials.

## Open API Evaluation & Integration
[cite_start]To fulfill the requirement of fetching external data, the **Ticketmaster Discovery API** was selected[cite: 9, 12].

* [cite_start]**Evaluation:** The Ticketmaster API is a highly reliable, legitimate open events API that provides structured JSON data[cite: 12]. [cite_start]It comprehensively fulfills the data requirements by providing the event title, date, venue, description, and optionally event images[cite: 14].
* [cite_start]**Integration Strategy:** The application periodically fetches event data from the selected Open API[cite: 19]. [cite_start]The JSON payload is processed, text data is displayed to the users as "University Events," and the associated event images are extracted and downloaded directly into the secure S3 bucket[cite: 21, 22].

## Step-by-Step Deployment Guide
1. **Networking:** Created a VPC with 2 public subnets (for the ALB/NAT) and 2 private subnets (for the EC2 instances).
2. **Security:** Configured an IAM Role with S3 access policies and created a Security Group allowing HTTP (Port 80) traffic.
3. **Storage:** Provisioned a secure, private S3 bucket for media storage.
4. **Compute:** Launched two Amazon Linux 2023 EC2 instances in separate private subnets, attaching the IAM role and Security Group.
5. **Load Balancing:** Deployed an Application Load Balancer across the public subnets, routing traffic to a Target Group containing the private EC2 instances.
6. **Application Deployment:** Deployed the web application source code to the EC2 instances to fetch and serve the API data.
