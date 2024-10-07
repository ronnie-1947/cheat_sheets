# EC2

## EC2 Overview

1. **EC2** is one of the most popular AWS offerings.
   * EC2 = **Elastic Compute Cloud** = Infrastructure as a Service (IaaS).
2. EC2 provides the capability to:
   * Rent virtual machines (**EC2**).
   * Store data on virtual drives (**EBS** - Elastic Block Store).
   * Distribute load across machines (**ELB** - Elastic Load Balancing).
   * Scale services automatically using an **auto-scaling group** (**ASG**).

## EC2 User Data

1. EC2 **User Data** allows bootstrapping of instances using a script.
   * Bootstrapping means launching commands when the machine starts.
   * The script runs **only once** at the instance's first start.
2. EC2 User Data is commonly used to automate boot tasks such as:
   * Installing updates.
   * Installing software.
   * Downloading common files from the internet.
   * Performing any other custom initialization tasks.
3. The EC2 User Data script runs with **root user** privileges.

## EC2 Instance Types - Overview

1. AWS offers different types of **EC2 instances** optimized for various use cases.
   * [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
2. AWS uses the following naming convention for EC2 instances:
   * Example: **m5.2xlarge**
     * **m**: Instance class (e.g., general-purpose, memory-optimized).
     * **5**: Generation (AWS improves them over time).
     * **2xlarge**: Size within the instance class, indicating the capacity.

### EC2 Instance Types – General Purpose

1. **General Purpose** instances are ideal for a variety of workloads, such as:
   * Web servers
   * Code repositories
2. These instances offer a balance between:
   * **Compute**
   * **Memory**
   * **Networking**

## Security Groups

1. **Security Groups** are the foundation of network security in AWS.
   * They control how traffic is allowed **into** or **out of** EC2 instances.
2. Key characteristics:
   * Security groups only contain allow **rules**.
   * Rules can reference by **IP address** or by another **security group**.

### Security Groups as a Firewall for EC2 Instances

1. **Security Groups** act as a "firewall" for EC2 instances, regulating network traffic.
2. They control:
   * **Access to ports**.
   * **Authorized IP ranges** – both IPv4 and IPv6.
   * **Inbound network** traffic (from others to the instance).
   * **Outbound network** traffic (from the instance to others).

## Classic Ports to Know

1. **22** = **SSH** (Secure Shell) – log into a Linux instance.
2. **21** = **FTP** (File Transfer Protocol) – upload files to a file share.
3. **22** = **SFTP** (Secure File Transfer Protocol) – upload files using SSH.
4. **80** = **HTTP** – access unsecured websites.
5. **443** = **HTTPS** – access secured websites.
6. **3389** = **RDP** (Remote Desktop Protocol) – log into a Windows instance.

## Different type of plans

<table><thead><tr><th width="210">Purchasing Option</th><th>Description</th></tr></thead><tbody><tr><td>On Demand</td><td>Like coming and staying at a resort whenever we want, paying the full price.</td></tr><tr><td>Reserved</td><td>Planning ahead, and if staying for a long time, may get a good discount.</td></tr><tr><td>Savings Plans</td><td>Pay a certain amount per hour for a specific period and stay in any room type.</td></tr><tr><td>Spot Instances</td><td>Bid for empty rooms; highest bidder keeps the room but can be kicked out anytime.</td></tr><tr><td>Dedicated Hosts</td><td>Book an entire building of the resort for exclusive use.</td></tr><tr><td>Capacity Reservations</td><td>Book a room for a period at full price, even if you don’t stay in it.</td></tr></tbody></table>
