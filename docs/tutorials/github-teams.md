---
title: GitHub Teams and Organizations
order: 2
---

# Using GitHub Tasks to Create Teams and Add Members

This guide describes how to create a team in a GitHub organization, invite a user to join the organization and the newly created team, and then remove the team.

> You will need a GitHub personal access token with the correct privileges (admin:org).

## Create Your Flow

1. Navigate to the Workflows page and click **Create new Workflow**. Choose **Start from scratch**, fill in the details, and select **Create**.

2. In the Workflow Editor, select the **Parameters** tab and define four _Required_ parameters:
   - **url**: The GitHub API endpoint
   - **token**: Personal access token with the correct privileges (admin:org)
   - **emailAddress**: The email address of the member to invite
   - **org**: The name of the GitHub organization

3. In the left panel, expand the GitHub category and drag the following Tasks onto the canvas:
   - **Create Team in Organization**
   - **Invite User to Organization**
   - **Invite Member to Team**
   - **Delete Team from Organization**

4. Expand the Utilities category and drag the following Tasks onto the canvas:
   - **Sleep**
   - **Json Path To Property**

5. Chain the Tasks as needed.

Edit each Task with the appropriate parameters using the pencil icon at the top of the Task.