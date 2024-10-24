---
title: Email with attachments
order: 1
---

# Sending email with attachments hosted on Box

The following use cases describe how you can send emails with attachments hosted on Box<sup>®</sup>, using a no-code workflow tool. These are useful in business workflows that require sending emails with attachments, either generated within the workflow or hosted on an artifact storage system like Box.

> You will _need_ a publicly accessible file hosted on Box for the task to download.

## Create your flow

First, create the new workflow by navigating to the Workflows page and clicking **Create new Workflow**. Choose to **Start from scratch**, fill in the details, and select **Create**.

You are taken to the Workflow Editor for this particular workflow. In the tabbed menu, select **Parameters** and define two _Required_ parameters, named **fileName** and **downloadURL**.

In the left panel of the Workflows tabbed page, expand the Utilities category and drag onto the canvas the **Execute Shell** task. After that, expand the Communication category and drag onto the canvas the **Send Email with Sendgrid** task. These are the two tasks that we are going to use in this tutorial.

Edit each task with the following parameters, using the pencil icon at the top of the task.

For the **Execute Shell** task, enter the following specifics:

- Directory Path: The folder where the files will get downloaded `/Workflow`
- Shell Interpreter: `bash`