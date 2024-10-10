---
title: Administer
order: 10
---

# Administer

> **Note:** the Administer part of Flow is restricted to administrators.

## Teams

Create and manage your teams. It provides an overview table for a high level snapshot of all Teams. You can then deep dive into each Team, its Users, its Workflows, and associated Settings.

## Users

View your Users and manage their role. It provides an overview table for a high level snapshot of all Users. You can deep dive into each User to view their Workflows, Teams, associated Settings and change their role.

## Global Parameters

Global parameters are one of the scopes of the parameter layering available. The Global Parameter administration screen allows you to create, edit, and delete Global parameters. For more information, see [Getting to Know Parameters](../fundamentals/parameters).

## Global Tokens

Global Tokens is where you can create and remove global scoped API tokens that allow you to perform actions against the API endpoints on behalf of the whole platform.

To understand more about scopes, see [Security Architecture](../architecture/security).

To understand more about the APIs, see [API Architecture](../architecture/apis).

## Team Quotas

See [Getting To Know Quotas](../fundamentals/quotas) for more information.

## Settings

The centralized home to configuring _everything_. You can control everything from feature flags and extensions through to user and team defaults, and how the Tasks perform.

### Features

This is the feature flags that are available in the system and allow you to retain control and personalize the experience for the users by enabling or disabling functionality. By _default_, all features are enabled.

| Feature           | Description                                                                                                    |
| ----------------- | -------------------------------------------------------------------------------------------------------------- |
| Activity          | The ability to interact with Workflow Activity                                                                 |
| Insights          | The ability to interact with the Insights functionality                                                        |
| Workflow Quotas   | Whether to enforce quotas on Workflows                                                                         |
| Workflow Triggers | Whether to display Triggers section on the Workflows Configuration page                                        |
| Workflow Tokens   | Whether to display Tokens section on the Workflows Configuration page                                          |
| Team Parameters   | Allow access to, and create, Team Parameters                                                                   |
| Global Parameters | Allow access to, and create, Global Parameters                                                                 |
| Team Management   | Allow management and editing of teams. If disabled, the Teams page will still be visible but in read only mode |
| User Management   | Allow management and editing of users. If disabled, the Users page will still be visible but in read only mode |
| Team Tasks        | Allow the creation and management of Team tasks                                                                |

### Extensions

You can enter information as to how Extensions work

| Setting             | Description                                                                                                                          |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Slack Auth Token    | The default auth token for authenticating as a Slack app. Only used if installing into a single workspace and not a distributed app. |
| Slack App ID        | The App ID from your Slack apps credentials                                                                                          |
| Slack Client ID     | The Client ID from your Slack apps credentials                                                                                       |
| Slack Client Secret | The Client Secret from your Slack apps credentials                                                                                   |

### Task Configuration

Ability to modify the underlying task execution configuration.

| Config                             | Description                                                                                                |
| ---------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Debug                              | Will enable debug information in the log of executing Tasks                                                |
| Default Image Path                 | If a Task doesn't define the image it will default to what is defined here                                 |
| Deletion Policy                    | Defines the completion state that will lead to worker removal. Options are: never, on success, and always. |
| Enable Verified Tasks to be Edited | By default, the Tasks that are shipped with Flow are considered 'verfied' and are not editable.            |
| Task Timeout Configuration         | Number of minutes before a Task times out.                                                                 |

### Team Defaults

Default quotas to apply to all Teams or to restore to when managing a Teams quotas.

### User Defaults

Default quotas to apply to all Users.

### Workspace Configuration - Activity Storage

Define how the system utilizes and interacts with storage per execution

You can set the default Storage Size, Class Name, Access Mode (Read Write Once, Read Write Many, and Read Only Many), and Maximum Storage Size

### Workspace Configuration - Workflow Storage

Define how the system utilizes and interacts with storage per Workflow and persisted across executions.

You can set the default Storage Size, Class Name, Access Mode (Read Write Once, Read Write Many, and Read Only Many), and Maximum Storage Size

## Task Manager

The Task Manager displays and allows you to manage all currently defined tasks, except for System Tasks.

![Task Manager Panel](./assets/img/administer-task-manager.png)

### Viewing tasks

The left panel displays the currently defined tasks by category. Expand each category to view its related tasks.

### Task header

The top of each task definition presents task version control. The name of the task, along with the date the task was last updated is displayed.

The **Version** control in the task header allows you to cycle through past versions of the task. When viewing the current version, **Reset changes** can be used to reset any editing you have done. When viewing a previous version, the parameters from that version can be copied into a new version using **Copy to new version**.

It also allows you to switch from Overview to YAML view. The YAML view supports the [Tekton Task definition](https://tekton.dev/docs/pipelines/tasks/).

### Task definition

Task definition is split into: **Basics**, **Definition fields**, and **Result Parameters**

#### Basics

The following fields are defined as **Basics**.

| Field                  | Description                                                                                                |
| ---------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Name**               | The identifier for task, used throughout the platform.                                                     |
| **Category**           | The group in the left task panel that is identified with the task.                                         |
| **Icon**               | The icon associated with that task, used throughout the platform.                                          |
| **Description**        | A short narrative of the task.                                                                             |
| **Image**              | The container image to use                                                                                 |
| **Command**            | The command to run when starting the container                                                             |
| **Arguments**          | The list of values passed into the task parameters.                                                        |
| **Script**             | The body of a script to run. If the script field is present, the task cannot also contain a command field. |
| **Working Directory**  | The directory inside the container to execute inside of                                                    |
| **Envs**               | Environment key values to define and pass through to the container                                         |
| **Contribution level** | Verified or Community                                                                                      |

#### Definition fields

Definition fields vary according to the task. See [Parameters](/boomerang-cicd/fundamentals/parameters).
The displayed fields can be dragged and dropped to change their order.

> **Note**: Click **Preview** in the top right corner of the page to view what the user sees for that task, when editing a task on the platform.

#### Result Parameters

Define the result parameters we should expect on completion of the Task. These are defined in advanced so Users know what variables they can utilize in following Tasks.

### Editing tasks

To edit a task, simply edit any of the parameters in the **Basics** and **Definition fields** and click **Save** in the top right corner of the page.

> **Note**: Verified tasks are not editable, unless the platform Administrator has enabled editing in the Global settings.

### Adding a task

Click **Add a new task** at the top of the left panel. A dialog requesting value for each of the **Basics** parameters is displayed.

Enter the desired values, then click **Create**.

![Add Task](./assets/img/task-mgr-add-task.png)

## System Workflows

The System Workflows screen allows you to define and execute System Workflows that do not abide to any Quotas and are essentially considered System wide workflows.

You also administer the Workflow Templates from this screen. Any workflow defined as a Template will appear in the Templates model on the Workflows screen.
