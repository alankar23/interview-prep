### **Node Affinity in Kubernetes** 🌐

**Node Affinity** is a set of rules used to constrain which nodes your pod can be scheduled onto based on labels on the nodes. It’s a way to express **preferences** or **hard constraints** for where pods should run in a Kubernetes cluster.

There are two types of **Node Affinity**:

1. **RequiredDuringSchedulingIgnoredDuringExecution**  
2. **PreferredDuringSchedulingIgnoredDuringExecution**

---

### **1️⃣ RequiredDuringSchedulingIgnoredDuringExecution**
This is a **hard requirement** that must be satisfied for the pod to be scheduled onto a node. If no node matches the criteria, the pod **won’t be scheduled**.

- **Scheduling:** The scheduler will only place the pod on nodes that meet the affinity rules.
- **Execution:** After the pod is scheduled, Kubernetes will **ignore** the affinity rules, meaning the pod can continue running on a node even if the node no longer matches the affinity rules.

#### **Example:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-required-affinity
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "disktype"
            operator: In
            values:
            - "ssd"
  containers:
  - name: nginx
    image: nginx
```

- In this example, the pod will only be scheduled on nodes where the label `disktype=ssd` exists. If no such node is available, the pod won’t be scheduled.

---

### **2️⃣ PreferredDuringSchedulingIgnoredDuringExecution**
This is a **soft preference**. The scheduler will try to place the pod on nodes that meet the affinity rules but **will not block scheduling** if no nodes match the criteria.

- **Scheduling:** Kubernetes will try to place the pod on nodes that match the preference, but if no such nodes exist, it will schedule the pod on any available node.
- **Execution:** Once scheduled, it will not attempt to reassign the pod if the node no longer matches the affinity rule.

#### **Example:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-preferred-affinity
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
      - weight: 1
        preference:
          matchExpressions:
          - key: "region"
            operator: In
            values:
            - "us-west"
  containers:
  - name: nginx
    image: nginx
```

- This example gives a preference to scheduling the pod on nodes where the label `region=us-west` exists, but the pod can be scheduled on other nodes if no such nodes are available.

---

### **Key Components of Node Affinity:**

1. **nodeSelectorTerms**  
   This is a list of **`matchExpressions`** or **`matchFields`** that specify the rules for matching nodes.

2. **matchExpressions**  
   Each `matchExpression` contains:
   - **key:** The label key on the node.
   - **operator:** The comparison operator (e.g., `In`, `NotIn`, `Exists`, `DoesNotExist`).
   - **values:** The set of values that should match the key.

#### **Example with multiple expressions:**
```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
      - matchExpressions:
        - key: "disktype"
          operator: In
          values:
          - "ssd"
        - key: "region"
          operator: In
          values:
          - "us-west"
```

---

### **Use Cases for Node Affinity** 🤔
1. **Hardware/Resource Constraints:**  
   Schedule workloads on nodes with specific hardware capabilities (e.g., GPUs, SSDs).

2. **Geographical Scheduling:**  
   Ensure that pods are scheduled in certain geographical regions for low latency or data sovereignty reasons.

3. **Avoiding Certain Nodes:**  
   Use **`nodeAffinity`** to **avoid** scheduling certain pods on specific nodes (e.g., nodes with limited resources or nodes under maintenance).

---

### **Summary**  
- **RequiredDuringSchedulingIgnoredDuringExecution** = Hard requirement (must match for scheduling).
- **PreferredDuringSchedulingIgnoredDuringExecution** = Soft preference (preferred but not required).
- Can help in resource optimization, geographic distribution, and fine-grained control of pod placement.
