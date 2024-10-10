---
title: Tasks
order: 7
---

# Tasks and Task Manager

When in the Workflow editor, notice the collapsible task palette on the left side of the Editor page.

Tasks are split into different expendable categories. Individual Tasks can be dragged and dropped on the Editor's graphical interface. From there, you connect Tasks together in a left-to-right flow. Click the pencil icon on any Task to access configuration parameters for a Task.

## Task Manager

Task management involves the following:

- Verified or Community Tasks
- Task scope and management
- Task versioning
- System Tasks
- Template Tasks

## Verified or Community Tasks

Tasks can be managed via Task Manager and can be labeled as `verified` or `community contribution`. _We may refer to Tasks as Template Tasks._

Tasks provided out-of-the-box with an installation are labeled as `verified`, with a blue ribbon icon located to the right of the Task in the palette.

Users are welcome to create their own Tasks, that will be labeled as a `community contribution`. The Boomerang community provides support to verified Tasks. These Tasks are described in more detail below.

## Task scope and management

Task Manager is available at the Team and Global levels, depending on the user role. It is the centralized place to define and manage the Tasks available to Workflows and are based on the Tekton<sup>®</sup> Task model, however abstracts the experience so you don't need to be working in YAML directly in Kubernetes<sup>®</sup>.

Tasks follow the Tekton Task model, along with Kubernetes standards that allow you to define what you want to happen at the execution of the Task, as well as parameters that are needed.

### Team Task Manager

The Team Task Manager can be found under Manage > Team Tasks and allows users in the team to create and manage Tasks scoped to the team and only available to that team. For more information, see [How to Manage Team Tasks](../fundamentals/manage).

### Global Task Manager

All of the shared Tasks across the instance of Boomerang Flow are Global Tasks that can be used by all teams.

### YAML Definition

The YAML specification has three important sections to be aware of: metadata, params, and steps. Its important to note that the full Tekton Task specification is not yet fully supported. We cannot run multi-step Tasks, nor do we allow resources to be specified. For more information, see [Known Issues and Limitations](../introduction/known-issues-&-limitations).

**Metadata**

This includes the standard name and labels metadata matching all Kubernetes object. In addition, this is the area where we store a series of annotations related to the end user experience. This is where we store the choices made in the UI that don't directly relate to the Task definition itself needed for the execution.

| Annotations         | Description                                                                                                                                                                            |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| boomerang.io/icon   | This is the icon for the Task as seen in the Workflow Editor.                                                                                                                          |
| boomerang.io/params | This is the metadata around a Task parameter to allow it to be rendered in the Workflow Editor. These map to the parameters in the `spec` block and will be defaulted if not provided. |

```yaml
metadata:
  annotations:
    boomerang.io/icon: Add
    boomerang.io/params:
      - required: true
        placeholder: ""
        defaultValue: ""
        readOnly: false
        key: enterpriseId
        label: Authentication - Enterprise ID
        type: text
        helperText: Box enterprise id
```

**Params**

As part of the `spec` block, this is where you define parameters that are needed for the execution of the Task.

```yaml
spec:
  params:
    - name: enterpriseId
      type: string
      description: ""
      default: ""
    - name: clientId
      type: string
      description: ""
      default: ""
    - name: clientSecret
      type: string
      description: ""
      default: ""
```

**Steps**

This is the definition that defines what the Task does.

```yaml
spec:
  steps:
    - name: Add Box Folder
      image: boomerangio/box-service:0.0.10
      command: null
      args:
        - -props
        - box
        - add
        - folderName
```

## Task Versioning

We may publish a new version of a Task if we want to add additional parameters or functionality. In doing so, these may introduce breaking changes. So Workflows won't be automatically updated to use the latest version of the Task.

There will be a caution icon displayed on your Workflow tile to let you know that there is a more recent version available of a Task. You can view the comparison of the previous version and what the new version looks like. You can continue to run your Workflow with the previous version, but we suggest you upgrade to the latest version.

## System Tasks

System Tasks are special Tasks that affect the logic of the Workflow and do not execute inside a container, but instead affect the processing of the DAG. System Tasks are identified by their system label, that appears to the top left of a Task when dragged onto the editor.

| Task Name                      | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Result Parameters                                                                                                                                                                                                                                                                                   |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Acquire Lock**               | Attempts to obtain a lock with the specified name. If no other Workflow in the team is using a lock with that name, a lock with that name and is created. If another Workflow in the team is using a lock with the specified name, the current Workflow waits until the lock is available.                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                                                     |
| **Manual Approval**            | This Task creates a pause in Workflow execution. An approver can mark their approval from the execution screen. Once an approver marks 'approve' or 'deny', then the Workflow will resume Task execution.                                                                                                                                                                                                                                                                                                                                                                       | <ul><li>`approvalStatus` - `approved` or `rejected`</li><li>`approvalDate` - Date and time of the action</li><li>`approvalUserName` - Username of the approver</li><li>`approvalUserEmail` - Email of the approver</li><li>`approvalComments` - Additional comments added by the approver</li></ul> |
| **Manual Task**                | A step with a series of instructions in Markdown that can be followed by an end user. Can be utilized for manual steps that may occur in a Workflow. It can then be marked as successfully completed or not and still follows the link states.                                                                                                                                                                                                                                                                                                                                  |                                                                                                                                                                                                                                                                                                     |
| **Release Lock**               | This Task releases a lock with the specified name.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |                                                                                                                                                                                                                                                                                                     |
| **Run Custom Task**            | A special type of System Task. With the Custom Task, you can bring your own container and run that as a Task. Custom Tasks also have a specialized ability to dynamically create result parameters, see [Getting To Know Parameters](../fundamentals/parameters).                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                     |
| **Run Scheduled Workflow**     | Schedules the selected Workflow to be run one time in the future. You define the Interval, Period (i.e. Days, Weeks, Months), the Time, which Workflow, and any Workflow Parameters that are needed to execture. This will then be viewable in the list of Schedules.                                                                                                                                                                                                                                                                                                           |                                                                                                                                                                                                                                                                                                     |
| **Run Workflow**               | Runs the selected Workflow separately. It only triggers the Workflow, it does not wait for completion. Additionally, you can pass in parameters to the Workflow if they have been defined.                                                                                                                                                                                                                                                                                                                                                                                      |                                                                                                                                                                                                                                                                                                     |
| **Set Result Parameter**       | Create a parameter that is set on the result of the Workflow. This Task accepts a name and value.                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |                                                                                                                                                                                                                                                                                                     |
| **Set Workflow Result Status** | Ability to override and set the Workflow's execution status. This allows you to mark the Workflow as failed but continue processing, for example sending an email or raising a ticket after a failure.                                                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                     |
| **Switch**                     | A Switch is a subtype of a system Task which allows for simple branch-based decisions. <br>This Task acts as a logical gate for Workflows. The Switch Task takes a value. This could likely be a result parameter from a previous Task or an input parameter to the Workflow. <br>You have the ability to conditionally control the path of execution, based on the value supplied to the switch. If a link coming off the switch has a set label that does not match the value of what was supplied to the switch, then those subsequent connected Tasks will not be executed. |                                                                                                                                                                                                                                                                                                     |
| **Wait For Event**             | Wait for an event from an external system to resume Workflow execution. The event can be a webhook or a Cloud Event. Visit the [eventing architecture](../architecture/eventing) for more information.                                                                                                                                                                                                                                                                                                                                                                          |                                                                                                                                                                                                                                                                                                     |

## Template Tasks

The following Tasks are available as verified Tasks. Their result parameters that are available for use are identified, refer to [Getting To Know Parameters](../fundamentals/parameters) for more information on how to use result parameters or provide input to Tasks with parameters.

### Artifactory

| Task Name         | Description                                                                                                                                                                                                              | Result Parameters |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- |
| **File Download** | Supplies login credentials to Artifactory (via username/password or username/API Token) and the direct URL route to the desired file. The file will be downloaded and named based upon the `Destination Path` specified. |                   |
| **File Upload**   | Similar to the above plugin, specifies the URL to include the path in Artifactory, and specifies the name of the file at the end of the path that is displayed in Artifactory.                                           |                   |

### Communication

| Task Name                             | Description                                                                                                                          | Result Parameters                                                                                      |
| ------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------ |
| **Send Email with Postmark Template** | Using Postmark, send an email with template using a template ID and variables. You can optionally specify tags and a message stream. | <ul><li>`To`</li><li>`SubmittedAt`</li><li>`MessageID`</li><li>`ErrorCode`</li><li>`Message`</li></ul> |
| **Send Email with SendGrid**          | Using your SendGrid API Key, send a basic email.                                                                                     |                                                                                                        |
| **Send Email with SendGrid Template** | Using your SendGrid<sup>®</sup> API Key, send a Dynamic Template email using a template ID and dynamic data.                         |                                                                                                        |
| **Send Twilio SMS**                   | Reference the [Twilio API](https://www.twilio.com/docs/sms/api)                                                                      |                                                                                                        |

### Communication with Slack

The following Slack<sup>®</sup> Tasks are to be used in conjunction with a Slack application. To integrate them with Slack API, first we'll need to [Create a New Slack App](https://api.slack.com/apps?new_app=1), and add `Bots` feature. Next we need to add at least one scope to the `Bot Token Scopes` based on the Slack Task-specific requirements. Next we will `Install to Workspace` the bot and allow the requesting access permission.

Once the newly created application is installed in your workspace, the UI will show us the `Bot User OAuth Token` which needs to be used in the Slack Tasks as `token` field.

| Task Name                                 | Description                                                                                                                                                                                                                                                                                                             | Result Parameters                                                                                           |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| **Create Slack Channel**                  | Create a Slack channel using a Slack application. Relies on the Slack underlying API [conversations.create](https://api.slack.com/methods/conversations.create). The Slack bot/user's required scopes are `channels:manage`, `groups:write`, `im:write`, `mpim:write`.                                                  | Result called `response`, containing: <ul><li>`ok`</li><li>`channel`</li></ul>                              |
| **Delete Slack Channel**                  | Archives a Slack conversation using a Slack app. Not all types of conversations can be archived. Relies on the Slack underlying API [conversations.archive](https://api.slack.com/methods/conversations.archive). The Slack bot/user's required scopes are `channels:manage`, `groups:write`, `im:write`, `mpim:write`. | Result called `response`, containing: <ul><li>`ok`</li></ul>                                                |
| **Find Slack Member By Email**            | Find a Slack user with an email address using a Slack app. Relies on the Slack underlying API [users.lookupByEmail](https://api.slack.com/methods/users.lookupByEmail). The Slack bot/user's required scope is `users:read.email`.                                                                                      | <ul><li>`slackUserId`</li></ul>                                                                             |
| **Find Slack Member By Id**               | Find a Slack user based on the Slack user Id using a Slack app. Relies on the Slack<sup>®</sup> underlying API [users.info](https://api.slack.com/methods/users.info). The Slack bot/user's required scope is `users:read.email`.                                                                                       | Result called `response`, containing: <ul><li>`ok`</li><li>`user`</li></ul>                                 |
| **Get Slack Channel Info**                | Retrieves the Slack channel information using a Slack app. For further reading on the underlying API [conversations.info](https://api.slack.com/methods/conversations.info). The Slack bot/user's required scopes are `channels:read`, `groups:read`, `im:read`, `mpim:read`.                                           | Result called `response`, containing: <ul><li>`ok`</li><li>`channel`</li></ul>                              |
| **Get Slack Channel Members**             | Retrieve the Slack channel members using a Slack app. Relies on the Slack underlying API [conversations.members](https://api.slack.com/methods/conversations.members). The Slack bot/user's required scopes are `channels:read`, `groups:read`, `im:read`, `mpim:read`.                                                 | Result called `response`, containing: <ul><li>`ok`</li><li>`members`</li><li>`response_metadata`</li></ul>  |
| **Get Slack Channels**                    | Retrieves the Slack channels using a Slack app. For further reading on the underlying API [conversations.list](https://api.slack.com/methods/conversations.list). The Slack bot/user's required scopes are `channels:read`, `groups:read`, `im:read`, `mpim:read`.                                                      | Result called `response`, containing: <ul><li>`ok`</li><li>`channels`</li><li>`response_metadata`</li></ul> |
| **Invite Members to Channel**             | Invite Slack users to a conversation using a Slack app. Relies on the Slack underlying API [conversations.invite](https://api.slack.com/methods/conversations.invite). The Slack bot/user's required scopes are `channels:manage`, `groups:write`, `im:write`, `mpim:write`.                                            | Result called `response`, containing: <ul><li>`ok`</li><li>`channel`</li></ul>                              |
| **Remove Member from Channel**            | Removes a user from Slack conversation using a Slack app. Relies on the Slack underlying API [conversations.kick](https://api.slack.com/methods/conversations.kick). The Slack bot/user's required scopes are `channels:manage`, `groups:write`, `im:write`, `mpim:write`.                                              | Result called `response`, containing: <ul><li>`ok`</li></ul>                                                |
| **Send Custom Slack Message**             | Allows for more configuration and creates a custom JSON payload if the simple Slack message does not meet your needs. Refer to the Slack API.                                                                                                                                                                           |                                                                                                             |
| **Send Simple Slack Message**             | Uses the wrapper plugin around the [Slack Messaging API](https://api.slack.com/messaging/sending).                                                                                                                                                                                                                      |                                                                                                             |
| **Send Slack Message with File Contents** | Similar to the simple Slack message, this plugin allows you to attach contents of a file.                                                                                                                                                                                                                               |                                                                                                             |
| **Upload Slack File with Message**        | Uploads a file to Slack.                                                                                                                                                                                                                                                                                                |                                                                                                             |
| **Slack User Look Up**                    | Derives a Slack username from an email, using the Slack API. Usually used as a step before sending a Slack message to a user.                                                                                                                                                                                           |                                                                                                             |

### File Utilities

| Task Name                       | Description                                                                                                                                                                                                | Result Parameters |
| ------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| **Check File Contains String**  | Specifies a direct path that includes the file name. The plugin will search to find a specified regex pattern for the file.                                                                                |                   |
| **Check File or Folder Exists** | Searches for a file or folder. You can have the Task fail if the folder/file is not found.                                                                                                                 |                   |
| **Create File**                 | Specifies a file location and its contents.                                                                                                                                                                |                   |
| **Read File to Parameter**      | Specifies a file and the desired result parameter name. The file gets stored as a result parameter string and can be referenced as a value in later Tasks.                                                 | `content`         |
| **Replace String in a File**    | Within a specified file, replaces the first instance of a string with the value supplied by the user during configuration.                                                                                 |                   |
| **Replace Tokens in a File**    | Within a specified file or files, replace all occurrences of tokens between a start and end pattern that all available as Parameters, so `@parmaeter@` would be replaced with the value of that parameter. | `files`           |

### GitHub

| Task Name                            | Description                                                                                                                                                                                                                                  | Result Parameters                                                  |
| ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Add Issue To Project**             | Adds an issue to GitHub<sup>®</sup> Issues Project.                                                                                                                                                                                          | <ul><li>`result`</li></ul>                                         |
| **Create Team in Organization**      | Creates a GitHub<sup>®</sup> team in the organization. The authenticated user must be a member or owner of the organization where the team is created.                                                                                       | <ul><li>`team`</li></ul>                                           |
| **Delete Team from Organization**    | Deletes a GitHub team from the organization. To delete a team, the authenticated user must be an organization owner or team maintainer. If you are an organization owner, deleting a parent team will delete all of its child teams as well. |                                                                    |
| **Find Issues and Label**            | Finds issues in a repository based on time since last activity, and adds a label and a comment. This is useful for marking issues inactive or reminding users to update an issue.                                                            |                                                                    |
| **Find Issues and Remove**           | Finds GitHub issues and removes them with optional days since activity and labels.                                                                                                                                                           |                                                                    |
| **Find Repositories in Org**         | Finds GitHub repositories in a particular organization filtered by the desired visibility.                                                                                                                                                   | <ul><li>`repositories`</li><li>`repositoriesPrettyPrint`</li></ul> |
| **Get All Organizations**            | Lists all GitHub organizations, in the order that they were created on GitHub. The Task offers a pagination capability powered by the `First organization ID` and `Number of organizations` parameters.                                      | <ul><li>`organizations`</li></ul>                                  |
| **Get All Teams In Organization**    | Lists all teams in an organization that are visible to the authenticated user's token. The Task offers a pagination capability powered by the `Page Number` and `Number of teams per page` parameters.                                       | <ul><li>`teams`</li></ul>                                          |
| **Get Organization Info**            | Retrieves a GitHub organization details based on its name.                                                                                                                                                                                   | <ul><li>`organization`</li></ul>                                   |
| **Get Github Repository**            | Get the GitHub repository details based on the GitHub URL.                                                                                                                                                                                   | <ul><li>`result`</li></ul>                                         |
| **Get Team In Organization**         | Retrieves a GitHub team details based on its name.                                                                                                                                                                                           | <ul><li>`team`</li></ul>                                           |
| **Invite Collaborator to Project**   | Invites a GitHub user identified via email to collaborate to a repository.                                                                                                                                                                   |                                                                    |
| **Invite Member to Team**            | Adds an organization member to a GitHub team. If the user is already a member of the team, the Task will update the role of the team member's role.                                                                                          | <ul><li>`result` of the invitation</li></ul>                       |
| **Invite User to Organization**      | Adds a member to a GitHub organization.                                                                                                                                                                                                      | <ul><li>`result` of the invitation</li></ul>                       |
| **Make Repositories Private**        | Loops through and changes repositories to private visibility.                                                                                                                                                                                |                                                                    |
| **Remove Collaborator From Project** | Removes a GitHub Collaborator from a repository.                                                                                                                                                                                             |                                                                    |
| **Remove Member from Team**          | Removes a team membership between a user and a GitHub team, within an organization.                                                                                                                                                          |                                                                    |

### IBM Consulting Essentials

| Task Name                        | Description                                                                                                                        | Result Parameters |
| -------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| **Send Platform Email**          | Configures an email to be sent from the platform.                                                                                  |                   |
| **Send Platform Notification**   | Sends a platform-wide notification. For more information on platform notifications, see the IBM Services Essentials documentation. |                   |
| **Create Support Center Ticket** | Raise a support center ticket as `issue` or `query`. Should be used when you got a problem, found a bug or just need some help.    |                   |

### ServiceNow

| Task Name            | Description                                                     | Result Parameters |
| -------------------- | --------------------------------------------------------------- | ----------------- |
| **Get Incidents**    | Gets incidents from ServiceNow<sup>®</sup> with optional state. |                   |
| **Update Incidents** | Updates incidents from ServiceNow.                              |                   |

### Utilities

| Task Name                      | Description                                                                                                                                                                  | Result Parameters                                                                                         |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| **Execute Advanced HTTP Call** | Makes an HTTP request specifying an endpoint and retrial, success and failure scenarios.                                                                                     | <ul><li>`response` with the content of the HTTP request</li><li>`statusCode` HTTP response code</li></ul> |
| **Execute Basic HTTP Call**    | Makes an HTTP request, and specifies an endpoint.                                                                                                                            | <ul><li>`response` with the content of the HTTP request</li><li>`statusCode` HTTP response code</li></ul> |
| **Execute Shell Script**       | Supply your own script. This is a special type of Task that also allows dynamic creation of result parameters, see [Getting To Know Parameters](../fundamentals/parameters). |                                                                                                           |
| **File JSONPath To Parameter** | Takes the first value returned by a valid JSONPath expression from a File content and sets as an output parameter.                                                           | `evaluation` - The value based on the applied expression                                                  |
| **JSONPath To Parameter**      | Takes the first value returned by a valid JSONPath expression and sets as an output parameter.                                                                               | `evaluation` - The value based on the applied expression                                                  |
| **MongoDB Query Execution**    | Takes a query and runs it against a MongoDB endpoint identified through the definition fields. The output of the query is saved in an external file.                         |                                                                                                           |
| **Run Python Script**          | Execute a Python script. The returned result of the script is returned as the result value of the Task.                                                                      | `output` - Python script standard output standard and error content.                                      |
| **Sleep**                      | Sleeps for a specified duration, in milliseconds.                                                                                                                            |                                                                                                           |

### Box

| Task Name                       | Description                                                                                                               | Result Parameters                                              |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| **Add Box Folder**              | Adds a Box<sup>®</sup> folder with the specified name. If no parent folder is provided, then the Box root folder is used. | <ul><li>`status` - `OK` or `Error`</li><li>`id`</li></ul>      |
| **Add User To Box Folder**      | Adds an user to the specified Box folder.                                                                                 | <ul><li>`status` - `OK` or `Error`</li></ul>                   |
| **Download File From Box**      | Downloads a file from Box to the provided path.                                                                           | <ul><li>`status` - `OK` or `Error`</li></ul>                   |
| **Get Box Folder Info**         | Gets the details of a Box folder.                                                                                         | <ul><li>`status` - `OK` or `Error`</li><li>`folder`</li></ul>  |
| **List Box Folders**            | Retrieves and lists an user's Box folders.                                                                                | <ul><li>`status` - `OK` or `Error`</li><li>`folders`</li></ul> |
| **Remove Box Folder**           | Removes a Box folder.                                                                                                     | <ul><li>`status` - `OK` or `Error`</li></ul>                   |
| **Remove User From Box Folder** | Removes an user from a Box folder.                                                                                        | <ul><li>`status` - `OK` or `Error`</li></ul>                   |
| **Upload File To Box**          | Uploads the file at the specified path to the desired Box folder.                                                         | <ul><li>`status` - `OK` or `Error`</li><li>`id`</li></ul>      |

### Google Sheets

In order to integrate the following tasks with the Google<sup>®</sup> Sheets API, we'll need to:

> _Note:_ You need a Google project to enable the APIS.

- [Create a project](https://developers.google.com/workspace/guides/create-project)
- [Create a Service Account](https://developers.google.com/workspace/guides/create-credentials#create_a_service_account)
- [Obtain service account credentials](https://developers.google.com/workspace/guides/create-credentials#obtain_service_account_credentials)
- Share the spreadsheets we want to use with the service account.

For more information, please visit [Google for Developers Guide](https://developers.google.com/workspace/guides/getstarted-overview).

For information regarding the supported cell ranges and different ID's, please see [Google Sheets Concepts](https://developers.google.com/sheets/api/guides/concepts).

| Task Name                             | Description                                                             | Result Parameters                                                                                                                    |
| ------------------------------------- | ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **Add Sheet**                         | Adds a new sheet to an existing Google spreadsheet.                     |                                                                                                                                      |
| **Append Data**                       | Appends data to an existing Google spreadsheet.                         | `response` - [the API response](https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values/append#response-body) |
| **Clear Data**                        | Clears the cell data in the specified ranges from a Google spreadsheet. |                                                                                                                                      |
| **Copy Sheet To Another Spreadsheet** | Copies a sheet from a Google spreadsheet to another.                    |                                                                                                                                      |
| **Create Spreadsheet**                | Creates a new Google spreadsheet with the provided name.                | `spreadsheet` - [the API response](https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets#resource:-spreadsheet)    |
| **Delete Sheet**                      | Deletes a sheet from an existing Google spreadsheet.                    |                                                                                                                                      |
| **Get Data**                          | Gets the cell data in the specified ranges from a Google spreadsheet.   | `rows` - the list of row cell data from the provided ranges                                                                          |
| **Update Data**                       | Updates the cell data from a Google spreadsheet.                        | `response` - [the API response](https://developers.google.com/sheets/api/reference/rest/v4/UpdateValuesResponse)                     |
