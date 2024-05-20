# Cross VPC connection

This project demonstrates how to establish a connection between two Amazon EC2 instances residing in separate VPCs. The setup involves configuring an Nginx server on one instance in a private subnet, and then enabling communication from another EC2 instance in a different VPC's private subnet.

## Setup

1. **Create Two VPCs:**
   - Launch two VPCs, each with its own CIDR block:
     - VPC-A: 10.1.0.0/16
     - VPC-B: 10.2.0.0/16

2. **Create Subnets:**
   - In each VPC, create a public subnet and a private subnet:
     - VPC-A:
       - Public Subnet: `PUBLIC-SUBNET TORIQ-A`
       - Private Subnet: `PRIVATE-SUBNET TORIQ-A`
     - VPC-B:
       - Private Subnet: `PRIVATE-SUBNET-TORIQ-B`

3. **Launch EC2 Instances:**
   - Launch two EC2 instances:
     - Instance A: In the public subnet of VPC-A, launch an instance that will act as a bastion server.
     - Instance B: In the private subnet of VPC-A, launch an instance that will host an Nginx server.
     - Instance C: In the private subnet of VPC-B, launch an instance from which we will try to access the Nginx server.

4. **Configure Security Groups:**
   - Create security groups and configure rules to allow the necessary traffic:
     - **Bastion Server Security Group:**
       - Allow SSH access from your local machine.
     - **Nginx Server Security Group:**
       - Allow inbound traffic on port 80 (HTTP) from the private subnet of VPC-B.
     - **Private Server Security Group:**
       - Allow outbound traffic on port 80 (HTTP) to the private subnet of VPC-A.

5. **Enable VPC Peering:**
   - Establish VPC peering between VPC-A and VPC-B. This allows communication between instances in the two VPCs.

6. **Install and Configure Nginx:**
   - On Instance B (the Nginx server), install and configure Nginx. Ensure the Nginx server is listening on port 80 and can serve a simple web page.

7. **Test Connectivity:**
   - From Instance C (the private server in VPC-B), use `curl` to access the Nginx server on Instance B. You should be able to successfully retrieve the web page served by the Nginx server.

## Diagram

The diagram depicts the network topology:

<figure > 
<p align="center">
  <img src="./System_design.png" alt="project architecture" />
</p>
</figure>


## Notes

- This setup demonstrates a basic cross-VPC connection. You may need to modify security groups, firewall rules, and other configurations depending on your specific requirements.
- Consider using a dedicated load balancer in front of the Nginx server for high availability and scalability.
- Ensure that you adhere to best practices for security and network management when working with VPC peering and cross-VPC communication.
