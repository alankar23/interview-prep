You can get the **top N processes by CPU and memory usage** using the following commands:

---

### **1️⃣ Top N Processes by CPU Usage**
```sh
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -N
```
#### **Example: Get Top 5 CPU-Consuming Processes**
```sh
ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -6
```
(Note: The first row is a header.)

---

### **2️⃣ Top N Processes by Memory Usage**
```sh
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -N
```
#### **Example: Get Top 5 Memory-Consuming Processes**
```sh
ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -6
```

---

### **3️⃣ Combined: Top N by Both CPU & Memory**
```sh
echo "TOP N CPU USAGE:"; ps -eo pid,ppid,cmd,%cpu --sort=-%cpu | head -N
echo "TOP N MEMORY USAGE:"; ps -eo pid,ppid,cmd,%mem --sort=-%mem | head -N
```
Replace `N` with the number of results you want.

Would you like this wrapped in a script for easy execution? 🚀