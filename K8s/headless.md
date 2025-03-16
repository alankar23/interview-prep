# Headless Service

[Stackoverflow Defination](https://stackoverflow.com/a/67160106/9819856)


### **Headless Service vs. Normal Service in Kubernetes**  

| Feature          | **Headless Service** 🏗️ | **Normal Service** 🌐 |
|-----------------|-----------------|-----------------|
| **Cluster IP** | `None` (No virtual IP assigned). | Allocates a Cluster IP. |
| **DNS Resolution** | Returns pod IPs directly. | Returns a single Cluster IP. |
| **Load Balancing** | No built-in load balancing. Clients must handle it. | Kubernetes load balances traffic across pods. |
| **Use Case** | Used for databases (Cassandra, Elasticsearch) or applications needing direct pod access. | Ideal for stateless services (web apps, APIs) where clients don't need to know individual pod IPs. |
| **Pod Communication** | Clients discover and connect to individual pods directly. | Clients connect via the Cluster IP, which routes traffic to healthy pods. |

---

### **Example YAML for Each**  

#### **Headless Service Example (For Databases)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-headless-service
spec:
  clusterIP: None
  selector:
    app: my-db
  ports:
    - port: 5432
      targetPort: 5432
```
🔹 **Behavior:** Pods are directly discoverable via DNS queries like `my-headless-service.default.svc.cluster.local`.  

---

#### **Normal Service Example (For Web Apps)**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: ClusterIP
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 80
```
🔹 **Behavior:** Clients connect using `my-service.default.svc.cluster.local`, and traffic is load-balanced.  

---

### **When to Use What?**  
✅ **Use a Headless Service when:**  
- Your application **requires direct pod access** (e.g., databases, StatefulSets).  
- You need **custom load balancing** (e.g., client-side discovery).  

✅ **Use a Normal Service when:**  
- You want **automatic load balancing** for stateless applications.  
- Clients should access the service **via a single Cluster IP**.  

Would you like an example of how DNS works differently in these cases? 🚀
