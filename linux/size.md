To get directory details and sort them by size in Linux, use the following command:

```sh
du -ah --max-depth=1 /path/to/directory | sort -rh
```

### **Explanation:**
- `du -ah` → Show disk usage (`du`), including hidden files (`-a`), in human-readable format (`-h`).
- `--max-depth=1` → Limits the scan to the first level inside the directory.
- `sort -rh` → Sorts the results **numerically** (`-r` for reverse, largest first, `-h` for human-readable sizes).

### **Example:**
To check the **current directory** (`.`):
```sh
du -ah --max-depth=1 . | sort -rh
```

### **Find the Top 10 Largest Directories & Files**
```sh
du -ah /path/to/directory | sort -rh | head -10
```

Would you like a more detailed breakdown or a script for automation? 🚀