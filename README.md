# 🌍 Multi-Region AWS VPC Connectivity Using Transit Gateway Peering

![AWS](https://img.shields.io/badge/AWS-Networking-orange)
![Project](https://img.shields.io/badge/Architecture-Multi--Region-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)

---

## ❗ Problem Statement
Organizations operating in multiple regions require **secure, scalable, and low-latency connectivity** between VPCs.  

Traditional VPC peering becomes complex and difficult to manage as the network grows.

---

## 💡 Solution
This project demonstrates how to use **AWS Transit Gateway Peering** to create a **centralized, scalable, and efficient multi-region network architecture**.

---

## 🏗️ Architecture Overview

![Architecture Diagram]
<p align="center">
  <img src="architecture-diagram.png\architecture-diagram.png" width="700"/>
</p>

The architecture connects VPCs deployed in two AWS regions:
- Region 1: us-west-1 (N. California)
- Region 2: us-east-2 (Ohio)
  
A Transit Gateway in each region acts as a hub to enable scalable communication between VPC networks.

---

## 🧠 Architecture Design

 Architecture Overview

 | Region    | Resource          |
| --------- | ----------------- |
| us-west-1 | NASENI VPC        |
| us-west-1 | ELDI VPC          |
| us-west-1 | Transit Gateway A |
| us-east-2 | SEDDI VPC         |
| us-east-2 | Transit Gateway B |


                                                            Connectivity Model
Region: us-west-1

-----------------------------

NASENI-VPC (10.1.0.0/16)

|

|

TGW-A

|

|

ELDI-VPC (10.10.0.0/16)

|

|

==== TGW Peering Attachment ====

|

|

TGW-B

|

|

SEDDI-VPC (10.16.0.0/16)

-----------------------------

Region: us-east-2

Traffic between the regions flows through the Transit Gateway Peering connection.



Network Design
| VPC    | Region    | CIDR         |
| ------ | --------- | ------------ |
| NASENI | us-west-1 | 10.1.0.0/16  |
| ELDI   | us-west-1 | 10.10.0.0/16 |
| SEDDI  | us-east-2 | 10.16.0.0/16 |

 
Each VPC contains:

- 1 Availability Zone

- 1 Public Subnet

- Network ACL

- Security Group

---

## ⚙️ Implementation Steps

1 Create VPC Infrastructure
Three VPCs were created using Amazon VPC:
| VPC        | Region    | CIDR         |
| ---------- | --------- | ------------ |
| NASENI-vpc | us-west-1 | 10.1.0.0/16  |
| ELDI-vpc   | us-west-1 | 10.10.0.0/16 |
| SEDDI-vpc  | us-east-2 | 10.16.0.0/16 |

 
Each VPC includes:

- Public subnet

- Route table

- Network ACL

- Security Group

2 Configure Network Security

Network ACL Rules

Inbound rules allowed:
- HTTP
- SSH
- ICMP
- TCP
  
Outbound rules mirror the inbound configuration to allow return traffic.

Security Group Rules
| Protocol | Port | Source   |
| -------- | ---- | -------- |
| HTTP     | 80   | Anywhere |
| SSH      | 22   | My IP    |
| ICMP     | All  | Anywhere |

 
3 Deploy Transit Gateways
Two Transit Gateways were deployed:
| Transit Gateway | Region    |
| --------------- | --------- |
| TGW-A           | us-west-1 |
| TGW-B           | us-east-2 |


4 Create Transit Gateway Attachments
Transit Gateway attachments were created for each VPC.
| Attachment | Resource   |
| ---------- | ---------- |
| Att-NASENI | NASENI VPC |
| Att-ELDI   | ELDI VPC   |
| Att-SEDDI  | SEDDI VPC  |

 
5 Establish Transit Gateway Peering

A Transit Gateway Peering Attachment was created between the two regions.
TGW-A (us-west-1)
|
| Peering Connection
|
TGW-B (us-east-2)

The peering attachment was then accepted in the destination region.

6 Configure Route Tables
Route tables were updated in each VPC to enable traffic routing across regions.
Example Route
| Destination  | Target          |
| ------------ | --------------- |
| 10.10.0.0/16 | Transit Gateway |
| 10.16.0.0/16 | Transit Gateway |

 Transit Gateway route tables were also configured with static routes.
 
7 Connectivity Testing
Instances created:
| Instance   | VPC    |
| ---------- | ------ |
| Dan-EC2    | NASENI |
| Dav-EC2    | ELDI   |
| Judith-EC2 | SEDDI  |

To validate the architecture, EC2 instances were deployed in each VPC using Amazon EC2. Instances created:
  
Ping tests confirmed successful connectivity using private IP addresses

-Connectivity Results
| Test            | Result     |
| --------------- | ---------- |
| Dan → Dav       | Successful |
| Dan → Judith    | Successful |
| Dav → Dan       | Successful |
| Judith → NASENI | Successful |

 
Example output:

PING 10.10.11.95

64 bytes from 10.10.11.95: icmp_seq=1 ttl=63 time=1.3 ms

This confirms inter-VPC and cross-region connectivity.
---

## 📸 Screenshots

### 🔹 Transit Gateway Setup
![TGW](screenshots/tgw-creation.png)

### 🔹 VPC Attachment
![Attachment](screenshots/vpc-attachment.png)

### 🔹 Route Table Configuration
![Routes](screenshots/route-table.png)

---

### 🔹 AWS Services Used
This project leverages several core AWS networking services:

- Amazon VPC – Virtual private networks

- AWS Transit Gateway – Centralized network hub

- Amazon EC2 – Instances used to test connectivity

- AWS Identity and Access Management – Secure access management

- Network ACLs and Security Groups – Network security controls

---

## 🔐 Security Considerations

- Controlled traffic using Security Groups  
- Route table segmentation  
- No unnecessary public exposure  

---

## 📈 Key Learnings

Transit Gateway simplifies complex network topologies  

Route table design is critical for connectivity  

Multi-region architectures require careful planning  

---

## 🌍 Real-World Use Case

This architecture is ideal for:

- Banks and financial systems  
- Multi-region SaaS platforms  
- Disaster recovery architectures  

---

## 🚀 Future Enhancements

Infrastructure as Code (Terraform)  

Monitoring with CloudWatch  

Failover and high availability improvements  

---

## 👨‍💻 Author
Cloud Networking Project by:
[Augustine Ebere Ohuabunwa]
Solution Architect | DBA | AWS Certified | Cost Optimization, Automation & Security | Enterprise Systems

---

## ⭐ Support

If you found this project useful, consider giving it a ⭐ and connecting with me on LinkedIn.









