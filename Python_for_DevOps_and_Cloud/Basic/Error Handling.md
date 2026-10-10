# 03 — Error Handling in Python

> Learn how to prevent a Python script from crashing unexpectedly and how to handle failures properly.

---

## Prerequisites

Before learning Error Handling, you should know:

- Python basics
- Variables
- `if/else`
- Functions
- Basic file handling
- Basic understanding of commands/scripts

---

# 1. What is Error Handling?

An **error** means something went wrong while our program was running.

Example:

```python
number = 10
result = number / 0
```
This gives:

ZeroDivisionError: division by zero

Normally, Python stops the program when an error occurs.

But in DevOps, this can be a problem.

For example:

Python Script
```
 |
 v
```

Connect to Server
```
 |
 V
```

Connection failed
```
 |
 V
```

Script crashes

We usually want:

Python Script
```
 |
 v
```

Connect to Server
```
 |
 V
```

Connection failed
```
 |
 v
```

Handle the error
```
 |
 v
```

Log / Retry / Exit safely

This is called Error Handling.

## Why Error Handling is Important in DevOps/Cloud?

DevOps scripts often interact with things that can fail:

Servers
Docker
Kubernetes
Cloud APIs
Files
Databases
Network
External commands
Configuration files

For example:

Python Deployment Script
```
    |
    +---- Read config
    |
    +---- Connect to server
    |
    +---- Build application
    |
    +---- Deploy application
    |
    +---- Verify deployment
```

Any step can fail.

Without error handling:

Failure → Script crashes

With error handling:

Failure
|
+--> Show useful error
|
+--> Log error
|
+--> Retry if required
|
+--> Stop safely
3. try and except

The basic syntax is:

try:
```
# code that may cause an error
```

except:
```
# code that handles the error
```

Example:

try:
```
number = 10 / 0
```

except:
```
print("Something went wrong")
```

Output:

Something went wrong

Instead of the program crashing, the error was handled.

## How try/except Works

Consider:

try:
```
number = 10 / 0
print("Hello")
```

except:
```
print("Error occurred")
```

Python executes:

try
|
v
10 / 0
|
## X error
|
v
except
|
v
"Error occurred"

Notice:

print("Hello")

is never executed because the error happened before it.

## Catching a Specific Error

It is better to catch the specific error instead of catching everything.

Example:

try:
```
number = 10 / 0
```

except ZeroDivisionError:
```
print("Cannot divide by zero")
```

Output:

Cannot divide by zero

This is better than:

except:

because we know exactly what error we are handling.

## Common Python Errors

You should know these basic errors:

Error	Meaning
ZeroDivisionError	Dividing by zero
ValueError	Wrong value
TypeError	Wrong data type
FileNotFoundError	File does not exist
KeyError	Dictionary key does not exist
IndexError	List index does not exist
PermissionError	No permission
ConnectionError	Connection problem

Example:

try:
```
number = int("hello")
```

except ValueError:
```
print("Invalid number")
```

Output:

Invalid number
## Handling Multiple Errors

You can have multiple except blocks.

try:
```
number = int(input("Enter number: "))
result = 10 / number
```

except ValueError:
```
print("Please enter a number")
```

except ZeroDivisionError:
```
print("Number cannot be zero")
```

Now different errors get different responses.

Example:

Input: hello
```
   |
   v
```

ValueError
```
   |
   v
```

Please enter a number

And:

Input: 0
```
   |
   v
```

ZeroDivisionError
```
   |
   v
```

Number cannot be zero
8. finally

finally contains code that should run whether an error happens or not.

Syntax:

try:
```
# risky code
```

except:
```
# handle error
```

finally:
```
# always runs
```

Example:

try:
```
number = 10 / 2
```

except ZeroDivisionError:
```
print("Cannot divide by zero")
```

finally:
```
print("Operation finished")
```

Output:

Operation finished

If there is an error:

try:
```
number = 10 / 0
```

except ZeroDivisionError:
```
print("Cannot divide by zero")
```

finally:
```
print("Operation finished")
```

Output:

Cannot divide by zero
Operation finished
## Why is finally Useful?

finally is useful when something must happen at the end.

For example:

Open resource
```
 |
 v
```

Perform operation
```
 |
 v
```

Error or Success
```
 |
 v
```

Cleanup

Example:

file = None

try:
```
file = open("config.txt", "r")
print(file.read())
```

except FileNotFoundError:
```
print("Config file not found")
```

finally:
```
if file:
    file.close()
```

The important idea:

try      → perform operation
except   → handle failure
finally  → cleanup
10. else

Python also provides else with try.

Syntax:

try:
```
# risky operation
```

except:
```
# error
```

else:
```
# runs when there is NO error
```

finally:
```
# always runs
```

Example:

try:
```
number = int("10")
```

except ValueError:
```
print("Invalid number")
```

else:
```
print("Number is valid")
```

finally:
```
print("Finished")
```

Output:

Number is valid
Finished

The flow is:

try
|
+---- Error ----> except
|
+---- No Error -> else
|
v
finally

For basic DevOps scripts, remember:

try     = try the operation
except  = handle failure
else    = run if successful
finally = always run
## Getting the Error Message

You can store the error in a variable using as.

try:
```
number = 10 / 0
```

except ZeroDivisionError as error:
```
print(error)
```

Output:

division by zero

This is useful for debugging and logging.

Example:

try:
```
open("config.yaml")
```

except FileNotFoundError as error:
```
print(f"Error: {error}")
```

## Error Handling in DevOps Scripts

Imagine a script that checks whether a server is reachable.

try:
```
# connect to server
print("Connecting to server...")
```

except ConnectionError:
```
print("Server connection failed")
```

Instead of allowing the script to fail without explanation, we give a useful message.

Real DevOps scripts may perform:

Read configuration
```
   |
   v
```

Connect to server
```
   |
   v
```

Run command
```
   |
   v
```

Check result
```
   |
   v
```

Deploy

Every step can potentially fail.

Error handling allows us to handle those failures.

## Example: File Handling in DevOps

Suppose our deployment script needs a configuration file.

try:
```
with open("config.yaml", "r") as file:
    config = file.read()
```

except FileNotFoundError:
```
print("config.yaml was not found")
```

Without error handling:

File missing
```
|
v
```

Script crashes

With error handling:

File missing
```
|
v
```

FileNotFoundError
```
|
v
```

Useful error message
## Example: Configuration File

A DevOps script may need:

config.yaml

If the file doesn't exist:

try:
```
with open("config.yaml", "r") as file:
    data = file.read()
```

except FileNotFoundError:
```
print("ERROR: config.yaml is missing")
```

This makes the problem clear.

## Example: Running a Command

Python can run system commands.

A command may fail.

Example:

import subprocess

try:
```
subprocess.run(
    ["docker", "ps"],
    check=True
)
```

except subprocess.CalledProcessError:
```
print("Docker command failed")
```

Here:

check=True

means Python should treat a failed command as an error.

DevOps use:

Python
|
v
docker ps
|
+---- Success → Continue
|
+---- Failure → except
## Important Rule: Don't Hide Errors

Avoid doing this:

try:
```
deploy_application()
```

except:
```
pass
```

This is bad.

Why?

Because the error is completely ignored.

You may think:

Deployment successful

while actually:

Deployment FAILED

Better:

try:
```
deploy_application()
```

except Exception as error:
```
print(f"Deployment failed: {error}")
```

Now the failure is visible.

17. raise

Sometimes we want to create an error ourselves.

Use:

raise

Example:

age = -5

if age < 0:
```
raise ValueError("Age cannot be negative")
```

Output:

ValueError: Age cannot be negative

In DevOps, this can be useful when configuration is invalid.

Example:

environment = "testing"

if environment not in ["dev", "prod"]:
```
raise ValueError("Invalid environment")
```

## Simple DevOps Example

Suppose a deployment script requires an environment.

environment = "prod"

try:
```
if environment not in ["dev", "prod"]:
    raise ValueError("Invalid environment")
```

```
print(f"Deploying to {environment}")
```

except ValueError as error:
```
print(f"Deployment stopped: {error}")
```

Output:

Deploying to prod

If we use:

environment = "test"

Output:

Deployment stopped: Invalid environment

This prevents a script from continuing with bad configuration.

## Error Handling Flow

Remember this simple flow:

## Start
```
              |
              v
            try
              |
      +-------+-------+
      |               |
   Success           Error
      |               |
      v               v
    else            except
      |               |
      +-------+-------+
              |
              v
           finally
              |
              v
             END
```

The important part:

try     → Try something
except  → Handle error
else    → Run if successful
finally → Always run
## DevOps/Cloud Use Cases

Error handling is commonly useful for:

## Configuration Files
Read config
↓
File missing?
↓
Handle error
## Cloud API Calls
Python script
```
 ↓
```

Cloud API
```
 ↓
```

Success / Failure
```
 ↓
```

Handle failure
## Server Connections
Connect to server
```
 ↓
```

Connection failed
```
 ↓
```

Handle error
## Docker Commands
docker command
```
 ↓
```

Command fails
```
 ↓
```

Python handles error
## Kubernetes Commands
kubectl command
```
 ↓
```

Command fails
```
 ↓
```

Show useful error
## Deployment Scripts
Build
↓
Test
↓
Deploy
↓
Failure?
↓
Handle safely
## File Operations
Read
Write
Delete
Move

These operations can fail because of:

Missing file
Wrong path
Permission problem
## Simple Best Practices
## Catch specific errors

Prefer:

except FileNotFoundError:

instead of:

except:
## Don't ignore errors

Bad:

except:
```
pass
```

Better:

except Exception as error:
```
print(error)
```

## Give useful error messages

Bad:

Error

Better:

ERROR: config.yaml was not found
## Use finally for cleanup

For example:

Open resource
```
 ↓
```

Use resource
```
 ↓
```

Error or success
```
 ↓
```

Cleanup
## Stop when an important configuration is invalid

Example:

if not config:
```
raise ValueError("Configuration is missing")
```

Don't continue a deployment with invalid configuration.

## Mini Real-World Example

A simple deployment-style script:

import subprocess

try:
```
print("Checking Docker...")
```

```
subprocess.run(
    ["docker", "ps"],
    check=True
)
```

```
print("Docker is working")
```

except FileNotFoundError:
```
print("Docker is not installed or not in PATH")
```

except subprocess.CalledProcessError:
```
print("Docker command failed")
```

finally:
```
print("Check completed")
```

What happens?

If Docker works:

Checking Docker...
Docker is working
Check completed

If Docker is not installed:

Checking Docker...
Docker is not installed or not in PATH
Check completed

If the Docker command fails:

Checking Docker...
Docker command failed
Check completed

The important thing is that the script gives a useful result instead of blindly crashing.

## Quick Cheat Sheet
Keyword	Purpose
try	Code that may fail
except	Handle an error
else	Runs when there is no error
finally	Always runs
raise	Manually create an error
as	Store the error in a variable

Basic pattern:

try:
```
risky_operation()
```

except SomeError as error:
```
print(error)
```

else:
```
print("Success")
```

finally:
```
print("Finished")
```

## Interview Points
Q1. What is exception handling?

Exception handling is a way to handle runtime errors without allowing the program to crash unexpectedly.

Q2. What is try?

try contains code that may produce an error.

Q3. What is except?

except handles an error produced inside the try block.

Q4. What is finally?

finally contains code that runs whether an error occurs or not.

Q5. Why is error handling important in DevOps?

DevOps scripts interact with servers, files, commands, APIs, containers, and cloud services. These operations can fail, so error handling allows scripts to fail safely and provide useful information.

## Final Mental Model

Don't memorize complicated definitions.

Just remember:

try
↓
"Try to do this"

except
↓
"Something went wrong, handle it"

else
↓
"Everything worked, continue"

finally
↓
"This must happen at the end"

For DevOps:

```
            Python Script
                 |
                 v
               try
                 |
      +----------+----------+
      |                     |
   Success                 Error
      |                     |
      v                     v
   Continue              except
      |                     |
      +----------+----------+
                 |
                 v
              finally
                 |
                 v
                END
```

The main goal of Error Handling
Don't let failures become
unexpected crashes.

Instead:

Detect → Handle → Explain → Finish safely