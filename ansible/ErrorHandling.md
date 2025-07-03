
# 🛠️ Ansible Error Handling Cheat Sheet

---

## ✅ 1. `ignore_errors: yes`
**Purpose:** Continue playbook execution even if a task fails.

```yaml
- name: This might fail
  command: /bin/false
  ignore_errors: yes
````

🧠 Useful for non-critical tasks.

---

## ✅ 2. `failed_when`

**Purpose:** Manually define when a task is considered failed.

```yaml
- name: Fail only if exit code is not 0 or 1
  command: ./my_script.sh
  register: result
  failed_when: result.rc not in [0, 1]
```

🧠 Great for scripts or commands with custom logic.

---

## ✅ 3. `changed_when`

**Purpose:** Manually control when a task reports “changed”.

```yaml
- name: Check service status
  command: systemctl status nginx
  register: result
  changed_when: false
```

🧠 Avoid false positives in reporting.

---

## ✅ 4. `rescue` and `always` (Block Error Handling)

**Purpose:** Run fallback or cleanup steps if something fails.

```yaml
- name: Use block to handle errors
  block:
    - name: Try to install package
      yum:
        name: fakepkg
        state: present

  rescue:
    - name: Log failure
      debug:
        msg: "Failed to install package."

  always:
    - name: Always run this
      debug:
        msg: "This runs no matter what."
```

🧠 Like try-catch-finally in programming.

---

## ✅ 5. `when` + `register`

**Purpose:** Conditional logic based on previous task result.

```yaml
- name: Check if file exists
  stat:
    path: /tmp/somefile
  register: file_status

- name: Do something if file is missing
  command: touch /tmp/somefile
  when: not file_status.stat.exists
```

🧠 Use `register` + `when` to handle branching behavior.

---

## ✅ 6. `any_errors_fatal: true`

**Purpose:** If any host fails, fail the whole playbook (useful for parallel runs).

```yaml
- hosts: all
  any_errors_fatal: true
  tasks:
    - name: Critical operation
      command: /bin/false
```

🧠 Stops all hosts if one fails — great for consistency.

---

## ✅ 7. `assert` Module

**Purpose:** Custom failure messages for validation.

```yaml
- name: Ensure variable is defined
  assert:
    that: my_var is defined
    fail_msg: "my_var is not set!"
```

🧠 Validate inputs and fail early with helpful messages.

---

## ✅ Bonus: Debug on Failure

```yaml
- name: Run command with debug on failure
  command: /bin/false
  register: result
  ignore_errors: yes

- debug:
    var: result
```

🧠 Helps troubleshoot failures interactively.

---

# ✅ General Tip:

Always use `register` + `debug` when testing error handling. It helps you understand what Ansible sees internally.

```

---

Would you like a printable version of this or want me to turn it into a slide or visual cheat sheet?
```
