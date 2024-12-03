## 1. You are unable to SSH into an EC2 instance in a public subnet. What could be the issue?

- **Security Group**: Ensure the inbound rule allows SSH (port 22) traffic from your IP address or a permitted range.
- **Network ACL**: Verify that the Network ACL associated with the subnet allows SSH traffic.
- **Key Pair**: Ensure you're using the correct private key (`.pem` file) associated with the EC2 instance's key pair.
- **Elastic IP**: Confirm that the EC2 instance has a public IP or an Elastic IP assigned.
- **Route Table**: Check that the route table associated with the subnet has a route to the internet gateway.
- **Instance State**: Ensure the instance is in the "running" state and not stopped or terminated.

---

## 2. Design a highly available and scalable 3-tier architecture in AWS.

1. **Presentation Layer**:
   - Use an **Elastic Load Balancer (ELB)** to distribute traffic.
   - Launch EC2 instances in an **Auto Scaling Group (ASG)** across multiple Availability Zones (AZs).

2. **Application Layer**:
   - Deploy the application on EC2 instances within a separate ASG.
   - Place the instances in private subnets across multiple AZs for high availability.

3. **Database Layer**:
   - Use Amazon **RDS** with Multi-AZ deployment for a managed database.
   - For NoSQL, use **Amazon DynamoDB** with global tables for scalability and durability.

4. **Networking**:
   - Utilize a **VPC** with public and private subnets.
   - Deploy a NAT Gateway in the public subnet for instances in private subnets.

5. **Monitoring and Backup**:
   - Use **CloudWatch** for monitoring.
   - Enable **RDS snapshots** or S3 backups for data durability.

---

## 3. How to block traffic from a particular country/region?

- Use an **AWS WAF** (Web Application Firewall):
  - Create a Geo Match rule to block traffic from the specific country or region.
  - Associate the rule with your ALB, CloudFront distribution, or API Gateway.

---

## 4. Your primary region suddenly goes down. How to move the application to the Disaster Recovery (DR) region?

1. **Database**:
   - Use **RDS with cross-region replication** or enable **DynamoDB global tables**.
2. **S3**:
   - Enable **cross-region replication** for S3 buckets.
3. **DNS**:
   - Use **Route 53** failover routing policies to direct traffic to the DR region.
4. **Infrastructure**:
   - Pre-deploy the infrastructure in the DR region using Infrastructure-as-Code tools like **CloudFormation** or **Terraform**.
5. **Testing**:
   - Periodically test the DR plan to ensure readiness.

---

## 5. Write a policy which has list access to EC2 instances and S3 buckets.

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeInstances",
        "s3:ListBucket"
      ],
      "Resource": "*"
    }
  ]
}
```

---

## 6. Lambda function is unable to access the database (any) hosted on an EC2 instance. What could be the issue?

- **VPC Configuration**:
  - Ensure the Lambda function is in the same VPC and subnet as the EC2 instance.
  - Attach proper security group rules for Lambda to communicate with the database.
- **Security Groups**:
  - Ensure the database's security group allows inbound traffic from the Lambda function's security group.
- **Network ACLs**:
  - Confirm there are no restrictive ACL rules blocking traffic.
- **IAM Role**:
  - Verify the Lambda function has the necessary IAM role to access resources.

---

## 7. Database is in a private subnet. What is the secure way to download the required package for the database?

```
