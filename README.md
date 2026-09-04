# NAT Gateway for Private Subnet Internet Access

**Enabling Secure Outbound-Only Internet Access for EC2 Instances in a Private Subnet**

## 📌 Project Overview

This project demonstrates the practical design and verification of a **NAT Gateway architecture within Amazon VPC**, providing a private EC2 instance with outbound-only internet access while preventing direct inbound connections.

The architecture uses public and private subnets, route tables, an Internet Gateway, and a NAT Gateway. Connectivity is validated through internet access and package installation from the private instance, while direct external SSH access is tested and rejected.

## 🎯 Objectives

* Create a custom Amazon VPC with public and private subnets.
* Configure an Internet Gateway for public subnet connectivity.
* Deploy a NAT Gateway in the public subnet.
* Provide outbound internet access to a private EC2 instance.
* Prevent direct inbound access to the private EC2 instance.
* Configure secure routing and security groups.
* Validate the architecture through connectivity and security tests.

## 🏗️ Architecture

```text
                         Internet
                             │
                             │
                    ┌────────▼────────┐
                    │ Internet Gateway│
                    └────────┬────────┘
                             │
                    ┌────────▼────────┐
                    │  Public Subnet  │
                    │   10.0.1.0/24   │
                    │                 │
                    │  NAT Gateway    │
                    │       +         │
                    │  Public EC2     │
                    └────────┬────────┘
                             │
                     NAT Translation
                             │
                    ┌────────▼────────┐
                    │ Private Subnet  │
                    │   10.0.2.0/24   │
                    │                 │
                    │  Private EC2    │
                    └─────────────────┘
```

## ☁️ AWS Services Used

* **Amazon VPC** – Creates the isolated network environment.
* **Public Subnet** – Hosts the NAT Gateway and bastion EC2 instance.
* **Private Subnet** – Hosts the EC2 instance without a public IP.
* **Internet Gateway** – Provides internet connectivity for the public subnet.
* **NAT Gateway** – Enables outbound internet access from the private subnet.
* **Elastic IP** – Provides a public IP for the NAT Gateway.
* **Route Tables** – Control traffic between subnets and gateways.
* **Amazon EC2** – Provides the public and private instances.
* **Security Groups** – Control inbound and outbound traffic.

## 🌐 Network Configuration

| Component          | Configuration                |
| ------------------ | ---------------------------- |
| VPC                | `myVPC`                      |
| VPC CIDR           | `10.0.0.0/16`                |
| Public Subnet      | `10.0.1.0/24`                |
| Private Subnet     | `10.0.2.0/24`                |
| Public EC2         | Amazon Linux 2023, t3.micro  |
| Private EC2        | Amazon Linux 2023, t3.micro  |
| NAT Gateway        | `MyNATGateway`               |
| Internet Gateway   | `myIGW`                      |
| Availability Zones | `ap-south-1a`, `ap-south-1b` |

## 🔄 Implementation Workflow

1. Create the custom VPC.
2. Create public and private subnets.
3. Create and attach the Internet Gateway.
4. Configure the public route table with a default route to the Internet Gateway.
5. Launch the NAT Gateway in the public subnet.
6. Allocate and associate an Elastic IP with the NAT Gateway.
7. Create the private route table.
8. Add `0.0.0.0/0` through the NAT Gateway.
9. Launch the private EC2 instance without a public IPv4 address.
10. Launch a public EC2 instance to act as a bastion host.
11. Configure security groups for controlled SSH access.
12. Connect to the private EC2 instance through the public EC2 instance.
13. Test outbound internet connectivity from the private instance.
14. Verify package installation and updates.
15. Test and confirm that direct external SSH access is blocked.

## 🔐 Security Configuration

* Private EC2 does not have a public IPv4 address.
* SSH access to the private instance is restricted through the public security group.
* The private security group does not allow SSH from `0.0.0.0/0`.
* NAT Gateway provides outbound connectivity without exposing the private instance to direct inbound internet traffic.
* IAM and CloudTrail can be used for additional access control and auditing.

## 🧪 Testing & Validation

### Successful Tests

* Ping external internet destinations from Private EC2.
* Run system updates using `yum`.
* Install Apache HTTP Server using `yum`.
* Access Private EC2 through the bastion host.

### Security Test

A direct SSH connection attempt from an external machine to the private EC2 instance was unsuccessful and timed out, confirming that the instance is not directly reachable from the internet.

## ✅ Results

The project successfully demonstrates that a private EC2 instance can:

* Access the internet for outbound connections.
* Download updates and packages.
* Remain without a public IP address.
* Prevent direct inbound SSH access from external networks.

## ⚠️ Limitations

* A single NAT Gateway can represent a single point of failure for the private subnet.
* NAT Gateway usage can incur AWS charges.
* The project is primarily implemented through the AWS Management Console rather than Infrastructure as Code.

## 🚀 Future Scope

* Deploy NAT Gateways across multiple Availability Zones for redundancy.
* Automate infrastructure using Terraform or AWS CloudFormation.
* Use VPC endpoints where appropriate to reduce NAT Gateway dependency.
* Implement centralized monitoring using CloudWatch and VPC Flow Logs.
* Introduce stricter IAM policies and automated security controls.

## 📂 Repository Structure

```text
nat-gateway-private-subnet/
│
├── Architecture Diagram
|   └── architecture-diagram.png
|
├── Presentation
|   └── AWS Project Presentation.pptx
|
├── Synopsis
|   └── AWS Project Synopsis.pdf
|
├── README.md
│
├── screenshots/
│   ├── vpc.png
|   ├── public-subnet.png
|   ├── private-subnet.png
|   ├── internet-gateway.png
|   ├── public-route-table.png
|   ├── private-route-table.png
|   ├── public-ip.png
│   ├── elastic-ip.png
│   ├── nat-gateway.png
│   ├── private-ec2.png
│   ├── public-ec2.png
│   ├── public-ec2-connect.png
|   ├── ping-google.png
|   ├── update-packages.png
|   ├── install-packages.png
|   ├── blocked-ssh.png
│
└── LICENSE
└── README.md
```

## 👥 Team

* **Tushit Mishra** — 2415001691
* **Prashant Sharma** — 2415001154
* **Dev Jaisawat** — 2415000495
* **Skand Singh** — 2415001577

## 📚 Conclusion

This project demonstrates a standard AWS networking pattern for providing **secure outbound internet access to private workloads without exposing them directly to the internet**. It provides practical experience with VPC networking, subnetting, routing, NAT, EC2, and security controls.
