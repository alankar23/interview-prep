## Design a 3 tier infrastructure using the AWS services which should be secure and highly available
```
   +-------------------+                 +---------------------+                 +-------------------+
   |     Internet      | <-- HTTP/HTTPS ->|    ALB (Public)     | <-- HTTP/HTTPS ->| EC2 (Frontend)    |
   | (Route 53)        |                 |                     |                 | (Public Subnet)    |
   +-------------------+                 +---------------------+                 +-------------------+
                                                             |
                                          +-------------------------------+
                                          | Application Tier (Private Subnet)|
                                          |  EC2 (App Logic) or Beanstalk  |
                                          |                               |
                                          +-------------------------------+
                                                             |
                                  +-----------------------------------------------+
                                  |               Database Tier                  |
                                  |    RDS (Multi-AZ) or Aurora (Private Subnet)  |
                                  |         (Encrypted, Backed up)               |
                                  +-----------------------------------------------+
```

### **3-Tier Architecture on AWS (Secure & Highly Available)**

1. **VPC Design**:
   - **Private Subnets**: For application and database tiers.
   - **Public Subnets**: For the frontend (presentation) tier.
   - **Multiple AZs**: For high availability across the architecture.

2. **Frontend (Presentation Tier)**:
   - **Services**: EC2 or ALB in public subnets for web traffic.
   - **High Availability**: Auto-scaling EC2 behind ALB across AZs.
   - **Security**: Security Groups restrict access to the backend and DB.

3. **Application (Logic Tier)**:
   - **Services**: EC2, Beanstalk, or Fargate in private subnets.
   - **High Availability**: Auto-scaling across multiple AZs.
   - **Security**: Security Groups control traffic from the frontend.

4. **Database (Data Tier)**:
   - **Services**: RDS (Multi-AZ) or Aurora in private subnets.
   - **High Availability**: Multi-AZ for failover.
   - **Security**: Database access restricted via Security Groups.

5. **Key Services**:
   - **Elastic Load Balancer (ALB)**: Distribute traffic.
   - **Auto Scaling**: Scale EC2 based on load.
   - **Route 53**: DNS routing for availability.

6. **Security**:
   - **IAM Roles/Policies**: Least privilege access.
   - **Encryption**: KMS for data encryption.
   - **CloudWatch & CloudTrail**: Monitoring and logging.

7. **Backup & DR**:
   - **RDS Snapshots** and cross-region replication for disaster recovery.

---

**Summary**: This design ensures secure, highly available, and scalable architecture using AWS services like VPC, EC2, ALB, RDS, and Auto Scaling.