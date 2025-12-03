---
title: "Week 2 Worklog"
date: "2025-11-14"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---

### Week 2 Objectives:

* Understand the basic about AWS Virtual Private Cloud 
* Learn about VPC Security and Multi-VPC Features
* Understand the basic of the VPC, DirectConnect, LoadBalancer, ExtraResources

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Study the knowledge related to Virtual Private Cloud <br> - Firewall in VPC <br> - **Practice:** <br>&emsp; + Create VPC, Subnet, Internet Gateway<br>&emsp; + Create Route Table, Security Group <br>&emsp; + Enable VPC Flow Logs <br> - Deploy Amazon EC2 Instances                                                                                        | 09/15/2025 | 09/15/2025      | <https://000003.awsstudygroup.com/1-introduce/>
| 3   | - Set up Hybrid DNS with Route 53 Resolver <br> - **Practice:** <br>&emsp; + Generate Key Pair <br>&emsp; + Intialize CloudFormation Template <br>&emsp;  + Configuring Security Group <br>&emsp; + Connecting to RDGW                                              | 09/16/2025 | 09/16/2025      | <https://000010.awsstudygroup.com/3-connecttordgw/> |
| 4   | - Learn about VPC Peerings and How to set up <br> - **Practice:** <br>&emsp; + Initialize CloudFormation Template <br>&emsp; + Create Security Group <br> &emsp; + Create EC2 Instance <br> &emsp; + Update Network ACL <br> &emsp; + Configuring route tables to enable communication between peered VPCs <br> &emsp; + Enabling and testing Cross-Peer DNS | 09/17/2025 | 09/17/2025      | <https://000019.awsstudygroup.com/6-crosspeerdns/> |
| 5   | - Learn basic AWS Transit Gateway <br>  - **Practice:** <br>&emsp; + Create Transit Gateway <br>&emsp; + Create Transit Gateway Attachments <br>&emsp; + Create Transit Gateway Route Tables <br>&emsp; + Add Transit Gateway Routes to VPC Route Tables                           | 08/18/2025 | 08/18/2025      | <https://000020.awsstudygroup.com/2-prerequiste/> |
| 6   | - Synthesize knowledge on Networking fundamentals (VPC, Subnets, Gateways, Routing) <br> - Do some exercises on VPC Peering and connecting VPCs using the Transit Gateway                                                                                     | 08/19/2025 | 08/19/2025      |  |


### Week 2 Achievements:

* **VPC Foundation Mastery:** Successfully created and configured core Virtual Private Cloud (VPC) components:
    * Subnets, Internet Gateway, Route Tables, and Security Groups.
* **Core Deployment & Monitoring:** Gained practical experience deploying **Amazon EC2 Instances** within the custom VPC environment.
* **Networking Security & Monitoring:** Applied security principles by configuring **Firewall mechanisms (Security Groups and Network ACLs)** and successfully enabled **VPC Flow Logs** for traffic inspection.
* **Multi-VPC Connectivity:** Mastered the setup and communication for complex networking scenarios:
    * **VPC Peering:** Configured route tables and tested **Cross-Peer DNS** between peered VPCs.
    * **Transit Gateway (TGW):** Learned and implemented TGW, managing attachments and route tables for scalable VPC interconnectivity.
* **Hybrid DNS & Access:** Practiced setting up **Hybrid DNS** using **Route 53 Resolver** and configured secure connection methods (Key Pair generation, RDGW access) via CloudFormation templates.
* **Knowledge Synthesis:** Consolidated all weekly networking knowledge by completing integrated exercises on complex multi-VPC routing and security.