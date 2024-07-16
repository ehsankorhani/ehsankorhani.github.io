---
layout: post
title: Create a Salesforce DX project
date: 2024-07-14
author: ehsan
tags: [Salesforce, CLI, SFDX]
cover-img:
thumbnail-img: /assets/img/salesforce-cli.png
share-img:
comments: true
---

#### 1. Create a New Playground

There's no local compiler to build Salesforce projects, so the first thing you need is an Org.

Log in to your [Trailhead](https://trailhead.salesforce.com/ "Trailhead") account, open the profile menu, and select the **Hands-On Orgs** option.

From there, you have the option to create a Playground.

#### 2. Set Up Your Credentials

From the Playground Starter app, select **Get Your Login Credentials**. Click the **Reset My Password** button.

You will receive an email with a link to reset your password. Click the link and follow the steps to set a new password.

#### 3. Create a DX project

I assume you already have Salesforce CLI and VS Code installed on your local machine.

In the VS Code terminal, navigate to the parent folder of your project and run the following command:

```bash
sf project generate --name MyProject
```

Alternatively, you can press F1 and select the appropriate SFDX command.

#### 4. Authorize an Org

Run the following command:

```bash
sf org login web --alias my-org
```

Or select the appropriate command from the SFDX commands list. You will be asked to enter your username and password and allow access to the CLI. Do that.

That's it. You're good to go! :)
