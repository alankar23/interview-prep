
# VPC

## SUBNET
### Create Subnet
	- Public Subnet
	- Private Subnet


#### Public subnet
	- create `route table`
	- associte `route table` with subnet
	- create a public route for internet access `0.0.0.0/0`

#### Private Subet (For Internet Access)
	- Create `route table` in `Private Subet`
    - Create `NAT Gateway` in `Public subnet`
    - Assign `Elastic IP` to `NAT GATEWAY`
	- Create a route in route table with Destination `0.0.0.0` and Target as `NAT GATEWAY`


## Difference between Security Groups and Network ACLs (NACLs)

### **Security Groups**  
- **Level**: Operates at the instance level.
- **Stateful**: Responses to allowed inbound traffic are automatically allowed outbound, and vice versa.
- **Rules**: 
  - Only allows traffic; no deny rules.
  - Defines rules based on protocols, ports, and IP ranges.
- **Application**: Attach directly to instances (ENIs).
- **Evaluation**: Evaluates all rules before deciding whether traffic is allowed.
- **Typical Use**: Instance-specific protection (like a firewall for individual servers).

### **Network ACLs (NACLs)**
- **Level**: Operates at the subnet level.
- **Stateless**: Explicit rules are required for both inbound and outbound traffic.
- **Rules**: 
  - Can allow or deny traffic.
  - Rules are evaluated in numbered order (lowest first).
- **Application**: Automatically applies to all instances in the associated subnet.
- **Evaluation**: Processes rules in order, and the first matching rule is applied.
- **Typical Use**: Subnet-wide traffic control (like a border firewall for subnets).

---

### **Key Differences**
| Feature              | Security Group           | NACL                     |
|----------------------|--------------------------|--------------------------|
| **Level**            | Instance-level          | Subnet-level            |
| **Statefulness**     | Stateful                | Stateless               |
| **Rules**            | Only allow (no deny)    | Allow and deny          |
| **Evaluation**       | All rules applied       | Rules processed in order|
| **Scope**            | Specific to instance    | Entire subnet           |
| **Use Case**         | Fine-grained control    | Broad traffic filtering |

### **When to Use**
- **Security Groups**: For controlling access to specific instances.
- **NACLs**: For controlling access to entire subnets or for an additional layer of security.

Let me know if you'd like deeper examples or scenarios!