# How-To: Package and Share an Application

One of the most valuable contributions you can make to the Contextually ecosystem is to package a useful workflow into a shareable Application. This guide will walk you through the process of creating a valid application package, ready for private distribution or for contribution to the public registry.

**Goal:** Turn a collection of local assets into a distributable `.tar.gz` archive.

---

### 1. Organize Your Assets

First, create a single root directory for your application. Inside this directory, organize all your assets into the standard `~/.cx/` subfolder names.

**Example Structure:**

```
my-awesome-app/
├── flows/
│ └── do-the-main-thing.flow.yaml
├── queries/
│ └── get-the-data.sql
└── README.md
```

### 2. Create the `app.cx.yaml` Manifest

At the root of your application directory (`my-awesome-app/`), create an `app.cx.yaml` file. This is the manifest that tells the `cx` installer what your application is and what it needs.

```yaml
# my-awesome-app/app.cx.yaml
name: my-awesome-app
namespace: community
version: 1.0.0
description: "This application does one awesome thing."
author: "Your Name"

# List all the blueprint versions your flows/queries depend on.
dependencies:
  blueprints:
    - "system/mssql@v0.1.1"

# Define the connections the user will be prompted to create.
required_connections:
  - id: "database" # The logical ID used in your flows
    description: "A connection to the source database."
    blueprint: "system/mssql@v0.1.1" # The suggested blueprint
```

### 3. Write a README.md

Create a README.md file in your application's root. This is crucial! The content of this file will be displayed in the user's terminal after they successfully install your application.
Your README.md should briefly explain:
What the application does.
What flows are now available.
How to run the main flow, including an example.

### 4. Package Your Application

Once your directory is structured and your manifest is complete, you can use the cx shell to create the distributable archive.
Navigate your terminal outside your application directory, and run the package command:
code
Bash

This command will read `my-awesome-app/app.cx.yaml` and create `my-awesome-app-v1.0.0.tar.gz` in your current directory.

```
cx app package ./my-awesome-app
```

!!! success "Package Created!"
You now have a .tar.gz file that can be installed by any cx user via cx app install <path/to/your/archive.tar.gz>. 5. (Optional) Contribute to the Public Registry
If you'd like to make your application available to the entire community, you can submit it to the flowcontextually/applications repository by opening a Pull Request.
