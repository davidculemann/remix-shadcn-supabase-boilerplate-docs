---
title: Manual Approval Output Details
order: 3
---

# Manual Approval Task Details

This tutorial describes how to create a Manual Approval Task and use its details like status, username, email, comments, and date. We will show how to send an email using the information retrieved from the Manual Approval Task.

## Create your flow

1. Create a new Workflow.
2. Define a required parameter named `sendgridKey` for the SendGrid API Key.

In the Workflow Editor, add a **Manual Approval** Task and a **Send Email with Sendgrid** Task to the canvas. Chain the Tasks as needed.

Edit each Task with the following parameters:

For the **Manual Approval** Task:
- Task Name: `Approve action`