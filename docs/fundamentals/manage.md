---
title: Manage
order: 10
---

# Manage

## Team Parameters

Team parameters are one of the scopes of the parameter layering available. The Parameter management screen allows you to create, edit, and delete Team parameters. For more information, see [Getting to Know Parameters](../fundamentals/parameters).

![Team Tasks in Workflow Editor](./assets/img/manage-team-params.png)

## Team Approvers

Manage groups of users to easily set as approvers. Groups can only be formed with users from the same Team.

## Team Tasks

Team Tasks are scoped to a specific team, and only visible by that team in the Task palette as part of the Workflow Editor. For more information, see [Getting to Know Tasks](/docs/boomrang-flow/fundamentals/tasks).

> **Note:** You will need to be careful that when you export a Workflow that references a Team Task, it will only be able to be imported to a team that has the same Task.

### Workflow Editors

In the Workflow Editor, Team Tasks are still found in the Task palette and are denoted by the words: 'Team Task' underneath the name of the Task.

![Team Tasks in Workflow Editor](./assets/img/manage-tasks-workfloweditor.png)

## Team tokens

Team tokens is where you can create and remove team scoped API tokens that allow you to perform actions against the API endpoints on behalf of a specific team.

To understand more about scopes, see [Security Architecture](../architecture/security).

To understand more about the APIs, see [API Architecture](../architecture/apis).
