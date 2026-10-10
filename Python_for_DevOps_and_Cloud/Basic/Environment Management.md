# 04 — Environment Management in Python
## venv, pip, and requirements.txt

> Environment Management means keeping a Python project's dependencies separate, controlled, and reproducible.

---

# Prerequisites

Before learning this topic, you should know:

- Basic Python
- How to run a Python file
- Basic terminal/command-line usage
- Basic understanding of Python packages

---

# 1. What is Environment Management?

Suppose you have two Python projects:

```text
Project A
    needs requests version 2.31

Project B
    needs requests version 2.28
```
If both projects use the same Python environment, they may conflict.

For example:

```text
System Python
     |
     +---- Project A
     |
     +---- Project B
```
**Project A may upgrade a package and accidentally break Project B.**

Environment management solves this problem.

Instead:

```text
Python
  |
  +---- Project A → its own environment
  |
  +---- Project B → its own environment
```
Each project gets its own packages.

# 2. Why is This Important in DevOps?

A DevOps engineer often runs Python applications on:

```text
Local machine
CI/CD server
Docker container
Cloud server
Automation server
```

The application may depend on specific package versions.

For example:
```text
Application
    |
    +-- Python 3.x
    +-- requests 2.x
    +-- boto3 1.x
```

If the required packages are missing or have incompatible versions:

```text
Application
    ↓
Dependency problem
    ↓
Application fails
```

So we need a way to:

```text
Create environment
        ↓
Install dependencies
        ↓
Record dependencies
        ↓
Recreate same environment
```

The main tools we use here are:
```text
venv
 ↓
Creates isolated Python environment
 ↓
pip
 ↓
Installs Python packages
 ↓
requirements.txt
 ↓
Records project dependencies
```

# 3. What is venv?

venv stands for **Virtual Environment**.

It creates a separate environment for a Python project.

Example:

```text
MyProject/
│
├── app.py
│
└── .venv/
      ├── Python
      └── Installed packages
```

The .venv environment belongs to this project.

# 4. Why Do We Need venv?

Without a virtual environment:

```text
Computer
   |
   +-- Global Python
          |
          +-- requests
          +-- flask
          +-- boto3
          +-- other packages
```

Different projects share the same packages.

With venv:

```text
Computer
   |
   +-- Project A
   |      |
   |      +-- .venv
   |           +-- requests
   |
   +-- Project B
          |
          +-- .venv
               +-- flask
```
Now projects are isolated.

# 5. Creating a Virtual Environment

First create your project:

```shell
mkdir my-project
cd my-project
```
Create the virtual environment:
```python
python -m venv .venv
```
Meaning:
```text
python
  ↓
Run Python's module system
  ↓
-m venv
  ↓
Create a virtual environment
  ↓
.venv
  ↓
Name of the environment
```

After running it:
```text
my-project/
│
└── .venv/
```

# 6. Activating the Virtual Environment

Creating the environment is not enough.

We need to activate it.

Windows

PowerShell:
```shell
.venv\Scripts\Activate.ps1
```
Command Prompt:
```shell
.venv\Scripts\activate
```
Git Bash:
```shell
source .venv/Scripts/activate
Linux/macOS
source .venv/bin/activate
```

After activation, your terminal usually looks like:

**(.venv)** user@machine:~/my-project$

The:

(.venv)

shows that the virtual environment is active.

# 7. Deactivating the Environment

When finished:

**deactivate**

The:

**(.venv)**

will **disappear**.

# 8. What is pip?

pip is Python's package installer.

It is used to install and manage Python packages.

For example:

pip install requests

This installs the requests package.

After installation:
```text
.venv
   |
   +-- requests
```

# 9. Why Do We Use pip?

Python itself provides many basic features.

But applications often need additional packages.

For example:
```text
Python
  |
  +-- requests → HTTP requests
  |
  +-- boto3 → AWS
  |
  +-- PyYAML → YAML files
```

pip helps install these packages.

# 10. Important pip Commands
Install a package
```shell
pip install requests
```
Install a specific version
```shell
pip install requests==2.31.0
```
This installs exactly version:

2.31.0
Upgrade a package
```shell
pip install --upgrade requests
```
Uninstall a package
```shell
pip uninstall requests
```

See installed packages
```shell
pip list
```

Example:
```text
Package    Version
---------- -------
requests  2.31.0
Check package information
pip show requests
```
This gives information such as:

Version
Installation location
Dependencies

# 11. What is requirements.txt?

requirements.txt is a text file containing the **Python packages** required by a project.

Example:
```text
requests==2.31.0
PyYAML==6.0.1
boto3==1.34.0
```
It tells another machine:

"Install these packages so this project can run."

# 12. Why is requirements.txt Important?

Imagine your project works perfectly on your laptop.

Your project uses:
```text
requests
PyYAML
boto3
```
You send the code to another developer.

They only receive:

**app.py**

They don't automatically know which packages your application needs.

But if you provide:

**requirements.txt**

they can install everything:
```shell
**pip install -r requirements.txt**
```
The -r means:

**Read** requirements from this file.

# 13. Creating requirements.txt

**If your virtual environment already contains the required packages**:
```shell
pip freeze > requirements.txt
```
Example:
```python
requests==2.31.0
PyYAML==6.0.1
```
Now your project can look like:
```text
my-project/
│
├── app.py
├── requirements.txt
└── .venv/
```

# 14. Installing from requirements.txt

On another machine:
```python
pip install -r requirements.txt
```
Flow:
```text
requirements.txt
       |
       v
pip install -r
       |
       v
Install required packages
       |
       v
Application can run
```
This is extremely common in CI/CD and deployment.

# 15. Complete Example

Suppose we create:
```text
python-devops-tool/
│
├── app.py
├── requirements.txt
└── .venv/
```
Step 1 — Create project
```shell 
mkdir python-devops-tool
cd python-devops-tool
```
Step 2 — Create environment
```python
python -m venv .venv
```
Step 3 — Activate it

Linux/macOS:
```python
source .venv/bin/activate
```
Windows:
```python
.venv\Scripts\Activate.ps1
```
Step 4 — Install package
```python
pip install requests
```
Step 5 — Create requirements file
```python
pip freeze > requirements.txt
```

Now:
```text
python-devops-tool/
│
├── app.py
├── requirements.txt
└── .venv/
```

# 16. Recreating the Project on Another Machine

Suppose another developer gets:
```text
app.py
requirements.txt
```
They can do:
```python
python -m venv .venv
```
Activate it:
```python
source .venv/bin/activate
```
Then:
```python
pip install -r requirements.txt
```
Now the required packages are installed.

# 17. Very Important: Don't Upload .venv to GitHub

You normally should not commit .venv to Git.

**Why?**

Because .venv can contain many files and packages.

Instead, commit:
```text
app.py
requirements.txt
```
and let another machine create its own:

**.venv**

# 18. Use .gitignore

Create:
```text
.gitignore
```
Add:
```text
.venv/
```
Example project:
```text
python-devops-tool/
│
├── app.py
├── requirements.txt
├── .gitignore
└── .venv/
```
Git will ignore .venv.

# 19. Version Pinning

Consider:

requests

This does not specify a **version**.

The package may **change** over time.

Better:
```text
requests==2.31.0
```
Now the project expects:
```text
requests 2.31.0
```
This helps make environments more predictable.

# 20. Why Dependency Versions Matter in DevOps

Imagine:
```text
Developer Machine
requests 2.31
      |
      v
Application works
```
But CI server has:
```text
requests 3.x
      |
      v
Application behaves differently
```
This can cause:
```text
Works on my machine
        ↓
CI fails
```
Using a **requirements** file helps **control** the **dependencies**.

# 21. **venv** + **pip** + **requirements.txt**

These three work together.
```text
                Python Project
                     |
                     v
                  venv
                     |
              Isolated environment
                     |
                     v
                    pip
                     |
              Install packages
                     |
                     v
             requirements.txt
                     |
              Record dependencies
```
Simple meaning:

**venv**
→ Where packages are installed

**pip**
→ Tool that installs packages

**requirements.txt**

→ List of packages required by the project

# 22. DevOps/Cloud Use Cases
Use Case 1 — CI/CD

A CI pipeline can create an environment and install dependencies:
```python
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app.py
```
Flow:
```text
Git Push
   ↓
CI Pipeline
   ↓
Create environment
   ↓
Install requirements
   ↓
Run tests
   ↓
Build / Deploy
```

### Use Case 2 — Cloud Server

Suppose a Python automation application is deployed on a server.

The server can do:
```python
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
Now the application has its required dependencies.

### Use Case 3 — DevOps Automation

Suppose you create a Python tool that interacts with AWS.

Your project may require:
```text
boto3
```
requirements.txt:
```text
boto3==1.34.0
```
A CI/CD server can **install** it **automatically**.

### Use Case 4 — Team Development

Every developer can create:
```text
.venv
```
and install:
```text
requirements.txt
```
This gives everyone the same project dependencies.

# 23. Common Mistakes
**Mistake 1** — Installing packages globally

Example:
```python
pip install requests
```
**without** using a virtual environment.

This may **install** the package into the **global Python environment**.

Better:
```text
python -m venv .venv
```

activate it, then:
```text
pip install requests
```

**Mistake 2** — Forgetting to activate the environment

You may think:

I installed the package.

But it may have been **installed** into **another Python environment**.

Check:
```python
pip list
```
after **activating** .venv.

**Mistake 3** — Committing .venv

Don't normally commit:
```text
.venv/
```
Use:
```text
.gitignore
```

**Mistake 4** — Not creating requirements.txt

If the project depends on **external packages**, keep a **dependency file**.
```python
pip freeze > requirements.txt
```

# 24. Simple Project Structure

A clean beginner Python project can look like:
```text
my-python-project/
│
├── app.py
├── requirements.txt
├── .gitignore
└── .venv/
```

**.gitignore:**
```python
.venv/
```
**requirements.txt:**
```python
requests==2.31.0
```
# 25. Important Commands Cheat Sheet
Create environment
```python
python -m venv .venv
```
Activate — Windows PowerShell
```python
.venv\Scripts\Activate.ps1
```
Activate — Linux/macOS
```python
source .venv/bin/activate
```
Deactivate
```shell
deactivate
```
Install package
```shell 
pip install requests
```
Install specific version
```shell
pip install requests==2.31.0
```
Remove package
```shell
pip uninstall requests
```
Show installed packages
```shell
pip list
```
Create requirements file
```shell 
pip freeze > requirements.txt
```
Install requirements
```shell 
pip install -r requirements.txt
```
# 26. Final Mental Model

Remember only this:
```text
              Python Project
                    |
                    v
              Create venv
                    |
                    v
          Activate .venv
                    |
                    v
             Use pip install
                    |
                    v
        Install required packages
                    |
                    v
        pip freeze > requirements.txt
                    |
                    v
       Commit code + requirements.txt
                    |
                    v
         CI/Server creates venv
                    |
                    v
      pip install -r requirements.txt
                    |
                    v
             Run application
```
The three most **important** things:

**venv**
→ Creates an **isolated** Python environment.

**pip**
→ **Installs** and **manages** Python packages.

**requirements.txt**
→ Records the packages and versions needed by the project.
# 27. One-Line Interview Answer

Python **environment management** means **creating** an **isolated environment** for a project using **venv**, **installing dependencies using pip**, and **storing those dependencies in requirements.txt** so the **same** application environment can be **recreated on another machine** or in **CI/CD**.