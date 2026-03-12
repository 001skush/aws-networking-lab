# Day 2 – NAT Instance Architecture

## Architecture Diagram

![Architecture](architecture.png)

---

## Architecture Overview

This lab demonstrates how a private EC2 instance can access the internet using a NAT instance located in a public subnet.

Components used:

* VPC (10.0.0.0/16)
* Public Subnet (10.0.1.0/24)
* Private Subnet (10.0.2.0/24)
* NAT Instance
* Internet Gateway
* Route Tables
* Security Groups

---

## Route Tables

### Public Route Table

| Destination | Target           |
| ----------- | ---------------- |
| 10.0.0.0/16 | local            |
| 0.0.0.0/0   | Internet Gateway |

### Private Route Table

| Destination | Target       |
| ----------- | ------------ |
| 10.0.0.0/16 | local        |
| 0.0.0.0/0   | NAT Instance |

---

## NAT Configuration

Enable IP forwarding

sudo sysctl -w net.ipv4.ip_forward=1

Configure NAT rule

sudo iptables -t nat -A POSTROUTING -o ens5 -j MASQUERADE

---

## Connectivity Test

From the private EC2 instance

ping google.com
curl https://google.com
sudo dnf update

Result:

Private EC2 successfully accessed the internet through the NAT instance.

---

## Key Learnings

* NAT Instance allows private subnet instances to access the internet.
* Source/Destination check must be disabled.
* IP forwarding must be enabled on the NAT instance.
* Route tables control traffic flow between subnets.
