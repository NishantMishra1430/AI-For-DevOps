# 02 — File Handling and Parsing
## Parsing JSON, YAML, and CSV

> **Goal:** Learn how DevOps/Cloud automation scripts read, understand, modify, and generate data stored in files.

---

# 1. Prerequisites

Before starting this topic, you should know:

- Basic Python syntax
- Variables
- Strings
- Lists
- Dictionaries
- `if/else`
- `for` loops
- Functions
- Basic exception handling (`try/except`)
- Basic file concepts
  - File
  - Path
  - Read
  - Write
  - Append
- Basic Linux commands such as:
  - `ls`
  - `cat`
  - `pwd`
  - `cd`

You do **not** need advanced Python.

---

# 2. What is File Handling?

File handling means:

> Using a program to read data from files or write data into files.

For example, suppose we have:

```text
config.json

{
    "app_name": "payment-service",
    "port": 5000
}

Instead of hardcoding these values in Python:

app_name = "payment-service"
port = 5000

```

we can read them from the file.

This makes our automation more flexible.

# 3. Why is File Handling Important in DevOps?

**DevOps contains a huge amount of configuration and structured data.**

Examples:

```text
config.json
config.yaml
docker-compose.yml
values.yaml
deployment.yaml
inventory.yaml
servers.csv
.env
```
Automation scripts frequently need to:

```text
Read file
   ↓
Understand data
   ↓
Extract required value
   ↓
Use value
   ↓
Possibly modify file
   ↓
Save changes
```

For example:

**A CICD Script may read:**

```text
image:
  repository: imageRegistry/myapp
  tag: v1
```
**Then Update**

```text
tag: v1.2
```

**and commit the change to Git.**


# 4. What is Parsing?

**Parsing:**Taking data stored in a particular format and converting it into a structure that a program can understand and work with.

For Example:

```python
{
    "name": "backend",
    "port": 5000
}
```
```python
print(data[name])


OutPut: backend
```

### Flow:

```text
File
 ↓
Parser
 ↓
Python object
 ↓
Program can use the data
```

# 5. Common Data Formats in DevOps
```text
 JSON
 YAML
 CSV
```
# What is JSON?

### JSON stands for

JavaScript Object Notation

It is a structured text format used for storing and exchanging data.

### Example

{
```
"name": "backend",
"port": 5000,
"environment": "production"
```
}

### JSON is extremely common in

```text
REST APIs
Cloud APIs
Configuration
Automation
CI/CD tools
AWS/Azure/GCP responses
Application configuration
```
---

## JSON Data Types

### JSON supports

String
Number
Boolean
Array
Object
Null

### Example

{
```
"name": "backend",
"port": 5000,
"production": true,
"servers": ["server1", "server2"],
"database": {
    "host": "db.example.com",
    "port": 5432
},
"backup": null
```
}
---

## Reading JSON in Python

### Python provides the built-in

json

module.

### Example

import json

### with open("config.json", "r") as file
```
data = json.load(file)
```

print(data)

### Suppose config.json contains

{
```
"app_name": "payment-service",
"port": 5000
```
}

### Then

print(data)

### gives

{
```
'app_name': 'payment-service',
'port': 5000
```
}

---

## Accessing JSON Values

Because JSON objects become Python dictionaries, we can access values using keys.

print(data["app_name"])

### Output

payment-service

### And

print(data["port"])

### Output

5000


---

## Nested JSON

### Consider

{
```
"application": {
    "name": "payment-service",
    "port": 5000
},
"database": {
    "host": "localhost",
    "port": 5432
}
```
}

### Python

import json

### with open("config.json", "r") as file
```
data = json.load(file)
```

print(data["application"]["name"])
print(data["application"]["port"])

print(data["database"]["host"])

### Output

payment-service
5000
localhost
---

## JSON Arrays

### JSON

{
```
"servers": [
    "server-1",
    "server-2",
    "server-3"
]
```
}

### Python

import json

### with open("servers.json", "r") as file
```
data = json.load(file)
```

### for server in data["servers"]
```
print(server)
```

### Output

server-1
server-2
server-3
---

## Reading JSON from a String

Sometimes JSON does not come from a file.

### It may come from

API response
Cloud CLI
Kubernetes command
Environment variable
Another program

### Example

import json

json_data = '{"name": "backend", "port": 5000}'

data = json.loads(json_data)

print(data["name"])

### Output

backend
13. json.load() vs json.loads()

This is important.

json.load()

Used when JSON comes from a file.

json.load(file)

### Think

load → file
json.loads()

Used when JSON comes from a string.

json.loads(string)

### Think

loads → string
---

## Writing JSON

Python can also create JSON files.

import json

data = {
```
"app": "backend",
"port": 5000,
"environment": "production"
```
}

### with open("config.json", "w") as file
```
json.dump(data, file, indent=4)
```

### This creates

{
```
"app": "backend",
"port": 5000,
"environment": "production"
```
}
15. Why indent=4?

### Without indentation

{"app":"backend","port":5000,"environment":"production"}

### With

indent=4

### it becomes

{
```
"app": "backend",
"port": 5000,
"environment": "production"
```
}

The data is the same.

Indentation only makes it easier for humans to read.

---

## Modifying JSON

### Suppose

{
```
"app": "backend",
"version": "1.0"
```
}

### Python

import json

### with open("config.json", "r") as file
```
data = json.load(file)
```

data["version"] = "1.1"

### with open("config.json", "w") as file
```
json.dump(data, file, indent=4)
```

### The file becomes

{
```
"app": "backend",
"version": "1.1"
```
}
---

## DevOps Use Case — Updating Docker Image Tag

### Suppose a deployment configuration contains

{
```
"image": {
    "repository": "myapp",
    "tag": "1.0"
}
```
}

### Python

import json

### with open("config.json", "r") as file
```
config = json.load(file)
```

config["image"]["tag"] = "1.1"

### with open("config.json", "w") as file
```
json.dump(config, file, indent=4)
```

This kind of operation is common in automation.

---

## YAML
What is YAML?

### YAML stands for

YAML Ain't Markup Language

YAML is a human-readable data/configuration format.

It is extremely important in DevOps.

### You will see YAML in

Kubernetes
Docker Compose
GitHub Actions
GitLab CI
Ansible
Helm
Argo CD
Cloud configuration
Infrastructure automation

### Example

application:
name: backend
port: 5000
environment: production
---

## Why DevOps Uses YAML So Much

YAML is designed to be easy for humans to read.

### Compare JSON

{
```
"application": {
    "name": "backend",
    "port": 5000
}
```
}

### YAML

application:
name: backend
port: 5000

YAML is generally easier to write and read.

---

## YAML Indentation

YAML uses indentation to represent hierarchy.

### Correct

application:
name: backend
port: 5000

### Here

application
├── name
└── port

Incorrect indentation can completely change the meaning or make the YAML invalid.

### Example

application:
name: backend
port: 5000

This is not equivalent.

---

## YAML Lists

### Example

### servers
- server-1
- server-2
- server-3

### Python will understand this approximately as

{
```
"servers": [
    "server-1",
    "server-2",
    "server-3"
]
```
}
---

## YAML Objects

### Example

database:
host: localhost
port: 5432
username: admin

### Python

{
```
"database": {
    "host": "localhost",
    "port": 5432,
    "username": "admin"
}
```
}
---

## Reading YAML in Python

Unlike JSON, YAML is not handled by Python's standard library.

### A commonly used library is

PyYAML

### Install

pip install pyyaml

### Then

import yaml

### with open("config.yaml", "r") as file
```
data = yaml.safe_load(file)
```

print(data)
24. Why safe_load()?

### Use

yaml.safe_load()

when reading normal YAML data.

It safely converts YAML into normal Python data structures.

### For DevOps automation, prefer

yaml.safe_load()

rather than unsafe loading methods.

---

## Accessing YAML Values

### Suppose

application:
name: backend
port: 5000

### Python

import yaml

### with open("config.yaml", "r") as file
```
data = yaml.safe_load(file)
```

print(data["application"]["name"])
print(data["application"]["port"])

### Output

backend
5000
---

## Reading Kubernetes YAML

This is extremely important for DevOps.

### Example Kubernetes deployment

| Field | Value |
| --- | --- |
| apiVersion | apps/v1 |
| kind | Deployment |

metadata:
name: backend

spec:
replicas: 3

selector:
```
matchLabels:
  app: backend
```

template:
```
metadata:
  labels:
    app: backend

spec:
  containers:
    - name: backend
      image: myapp/backend:1.0
      ports:
        - containerPort: 5000
```

Python can read this file.

import yaml

### with open("deployment.yaml", "r") as file
```
data = yaml.safe_load(file)
```

print(data["metadata"]["name"])
print(data["spec"]["replicas"])

### Output

backend
3
---

## DevOps Use Case — Change Kubernetes Replicas

### Python

import yaml

### with open("deployment.yaml", "r") as file
```
data = yaml.safe_load(file)
```

data["spec"]["replicas"] = 5

### with open("deployment.yaml", "w") as file
```
yaml.safe_dump(data, file, sort_keys=False)
```

### Now

spec:
replicas: 5

### This demonstrates a real automation pattern

Read Kubernetes YAML
```
    ↓
```
Find configuration
```
    ↓
```
Change configuration
```
    ↓
```
Write YAML
```
    ↓
```
Git commit
```
    ↓
```
---

# CI/CD
```
    ↓
```
Deployment
---

## YAML and Kubernetes

### A very important distinction

YAML itself does not deploy anything.

### For example

replicas: 3

is just data.

Kubernetes reads that data and interprets it according to the Kubernetes API.

### So

YAML
↓
kubectl / Helm / Argo CD
↓
Kubernetes API
↓
Kubernetes resources
---

## YAML and Helm

### Helm commonly uses

values.yaml

### Example

image:
repository: myapp
tag: "1.0"

replicaCount: 3

### An automation script can read or modify

image:
tag: "1.1"

This is a very common GitOps automation pattern.

---

## CSV
What is CSV?

### CSV stands for

Comma-Separated Values

It stores tabular data.

### Example

name,ip,environment
server-1,10.0.0.10,production
server-2,10.0.0.11,staging
server-3,10.0.0.12,development

Think of CSV as a simple spreadsheet in text form.

---

## CSV Structure

### Example

name,ip,environment
server-1,10.0.0.10,production
server-2,10.0.0.11,staging

### First line

name,ip,environment

is the header.

The remaining lines are records.

---

## Reading CSV in Python

### Python has a built-in

csv

module.

### Example

import csv

### with open("servers.csv", "r") as file
```
reader = csv.DictReader(file)

for row in reader:
    print(row)
```

### Output

{'name': 'server-1', 'ip': '10.0.0.10', 'environment': 'production'}
{'name': 'server-2', 'ip': '10.0.0.11', 'environment': 'staging'}
---

## Accessing CSV Columns
import csv

### with open("servers.csv", "r") as file
```
reader = csv.DictReader(file)

for row in reader:
    print(row["name"])
    print(row["ip"])
    print(row["environment"])
```

### Output

server-1
10.0.0.10
production

server-2
10.0.0.11
staging
---

## CSV Use Case — Server Inventory

### Suppose your company maintains

hostname,ip,environment,status
web-01,10.0.1.10,production,running
web-02,10.0.1.11,production,running
web-03,10.0.1.12,staging,stopped

### Python can find production servers

import csv

### with open("servers.csv", "r") as file
```
reader = csv.DictReader(file)

for server in reader:
    if server["environment"] == "production":
        print(server["hostname"])
```

### Output

web-01
web-02
---

## Writing CSV

### Python

import csv

servers = [
```
["web-01", "10.0.1.10", "production"],
["web-02", "10.0.1.11", "staging"]
```
]

### with open("servers.csv", "w", newline="") as file
```
writer = csv.writer(file)

writer.writerow(["hostname", "ip", "environment"])
writer.writerows(servers)
```

### Generated file

hostname,ip,environment
web-01,10.0.1.10,production
web-02,10.0.1.11,staging
---

## File Modes

### When working with files, you will commonly see

| Mode | Meaning |
| --- | --- |
| r | Read |
| w | Write |
| a | Append |
| r+ | Read + Write |

### Example

open("config.txt", "r")

means read the file.

open("config.txt", "w")

means write to the file.

### Important

w can overwrite existing content.