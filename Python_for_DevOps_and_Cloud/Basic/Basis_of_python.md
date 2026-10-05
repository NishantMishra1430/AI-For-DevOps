# 01. Python Syntax & Data Structures (Lists, Dicts, Sets, Tuples)

**Pre-requisites:** 
Before starting this topic, you should only know:
* How to install Python on your machine.
* What a basic variable is (e.g., `server_name = "web-server-1"`).
* How to run a simple Python script (`python script.py`).

---

## Why Do Data Structures Matter in DevOps & Cloud?
As a Cloud Engineer, you rarely write Python to build websites. You write Python to **automate infrastructure**. When you ask AWS, "Give me all my servers," AWS doesn't reply with English text. It replies with a massive block of structured data (usually JSON). 

To parse that data, find the broken server, and restart it automatically, you must master Python's four core data structures: **Lists, Dictionaries, Sets, and Tuples**.

---

## 1. Lists (The Ordered Queue)
A List is exactly what it sounds like: a collection of items in a specific order. You can add to it, remove from it, and change it.

* **Syntax:** Created using square brackets `[]`.
* **DevOps Use Case:** Storing a list of IP addresses, a queue of servers to be rebooted, or fetching all Security Group IDs attached to an EC2 instance.

### Code Example:
```python
# 1. Creating a list of server IP addresses
server_ips = ["192.168.1.10", "10.0.0.5", "172.16.0.1"]

# 2. Accessing data (Python starts counting at 0)
print(server_ips[0]) 
# Output: 192.168.1.10

# 3. Adding a new IP to the end of the list (e.g., auto-scaling spun up a new server)
server_ips.append("10.0.0.6")

# 4. Looping through the list to do an action on every server
for ip in server_ips:
    print(f"Deploying new code to: {ip}")

```

# 2. Dictionaries (The Key-Value Store)

**A Dictionary (often called a "Dict") stores data in Key-Value pairs. You look up a "Key" (like looking up a word in a real dictionary), and it gives you the "Value" (the definition).**

* **Syntax**: Created using curly braces {} with colons : separating keys and values.
* **DevOps Use Case**: This is the most important data structure in DevOps. Every API response from AWS, GitHub, or Kubernetes is formatted exactly like a Python dictionary (JSON). You use dicts to manage AWS Tags, server configurations, or API payloads.


## Code Example:

```python
# 1. Creating a dictionary for a single AWS EC2 instance configuration
ec2_instance = {
    "instance_id": "i-1234567890abcdef0",
    "instance_type": "t3.micro",
    "state": "running",
    "tags": ["prod", "frontend"] # You can put a List inside a Dict!
}

# 2. Reading a specific value (Looking up the instance type)
print(ec2_instance["instance_type"]) 
# Output: t3.micro

# 3. Changing a value (e.g., the server crashed)
ec2_instance["state"] = "stopped"

# 4. Adding a new Key-Value pair (Adding an IP address later)
ec2_instance["public_ip"] = "203.0.113.50"

```

# 3. Sets (The Unique Filter)

**A Set is a collection of items, but with one absolute rule: No duplicates allowed. It also has no specific order.**

* **Syntax**: Created using curly braces {}, but without colons.
* **DevOps Use Case**: Deduplication. Imagine you are scanning 10,000 VPC flow logs to see which IP addresses are attacking your network. The same hacker IP might appear 5,000 times. If you throw all 10,000 logs into a Set, Python automatically deletes all duplicates in a microsecond, leaving you with just the unique hacker IPs.

## Code Example:

```python
# 1. A log file gave us these IPs, notice the duplicates
raw_logs = ["10.0.0.1", "10.0.0.5", "10.0.0.1", "10.0.0.5", "192.168.1.1"]

# 2. Convert the list into a Set to instantly remove duplicates
unique_ips = set(raw_logs)

print(unique_ips)
# Output: {'192.168.1.1', '10.0.0.1', '10.0.0.5'}
# Notice the duplicates are gone, and the order might change!

# 3. Checking if a specific IP is in the set (Sets do this EXTREMELY fast)
if "10.0.0.5" in unique_ips:
    print("Block this IP in the AWS WAF!")
```

# 4. Tuples (The Immutable Lock)

**A Tuple is exactly like a List, but with one critical difference: it is Immutable (it cannot be changed after it is created). You cannot add, remove, or modify items inside a Tuple.**

* **Syntax**: Created using parentheses ().
* **DevOps Use Case**: Hardcoded configurations. Use Tuples for data that your script relies on and should never accidentally change during execution. Examples include database connection coordinates (Host, Port), or a fixed list of allowed AWS Regions.

## Code Example:

```python
# 1. Creating a Tuple for database connection details (Host, Port)
db_coordinates = ("rds.amazon.com", 5432)

# 2. Reading data works exactly like a List
print(db_coordinates[1])
# Output: 5432

# 3. Trying to change it will cause a crash (This is a GOOD thing for security)
# db_coordinates[1] = 3306  <-- This will throw an error! Python protects the data.

# 4. Defining a fixed list of allowed production regions
ALLOWED_REGIONS = ("us-east-1", "eu-central-1", "ap-south-1")

region_to_deploy = "us-west-2"

if region_to_deploy not in ALLOWED_REGIONS:
    print("Deployment Blocked: You cannot deploy to this region.")
```