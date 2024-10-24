---
title: GitHub Issues Bot
order: 0
---

# GitHub Issues Bot

The following use cases describe how you can set up a GitHub<sup>®</sup> Issues bot using a no-code workflow tool. These bots are useful in finding, labeling, and closing stale issues after a period of inactivity.

> You will _need_ a GitHub Personal Access token for a user that has access to the repository.

## Create your flow

First, create your new Workflow by navigating to the Workflows page and clicking **Create new Workflow**. At the modal, choose to **Start from scratch**, fill in the details, and select **Create**.

You are taken to the Workflow Editor for this particular Workflow. In the left panel of the Workflows page, expand the GitHub category. Identify the Tasks that we will use today: **Find Issues and Label** and **Find Issues and Remove**.

Drag these Tasks onto the screen. Edit each Task with the following parameters, using the pencil icon at the top of the Task.

For the **Find Issues and Label** Task, enter the following specifics:

- Endpoint: can be left with the default `https://api.github.com`
- Token: Your personal access token
- Owner: the GitHub organization that your repository is in
- Repository: the name of the repository
- Days Since Activity: `14 days`
- Label: `stale`
- Ignore Label: `bot ignore`