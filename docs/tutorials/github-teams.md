---
title: Github teams and organizations
order: 2
---

# Using GitHub Tasks to create teams and add members

The following use cases describes how you can create a team in a GitHub<sup>®</sup> organization, invite an user to join the organization and the newly created team. For the last step in the Workflow, we will remove the team, leaving the organization as it was initially.

> You will _need_ a GitHub personal access token with the correct privileges (admin:org).

> You will _also need_ access to Boomerang Flow.

## Create your flow

First, lets create the new Workflow by navigating to the Workflows page and clicking **Create new Workflow**. At the modal, choose to **Start from scratch**, fill in the details, and select **Create**.

You are taken to the Workflow Editor for this particular Workflow. In the tabbed menu, select the **Parameters** and define four _Required_ parameters, named **url** - _the GitHub<sup>®</sup> API endpoint_, **token** - _personal access token with the correct privileges (admin:org)_, **emailAddress** - _the email address of the member that will get invited to the team_ and **org** - _the name of the GitHub organization where the team is created_. Provide the values specific to your case.

![Workflow Parameters](./assets/github-define-parameters.png)

In the left panel of the Workflows tabbed page, expand the GitHub category and drag the following Tasks onto the canvas: **Create Team in Organization**, **Invite User to Organization**, **Invite Member to Team**, and **Delete Team from Organization**. After that, expand the Utilities category and drag the following two Tasks onto the canvas: **Sleep** and **Json Path To Property**. You can chain the Tasks, as shown in the following screenshot.

![Workflow Design](./assets/github-workflow-design.png)

Edit each Task with the following parameters, using the pencil icon at the top of the Task.

For the **Create Team in Organization** Task, enter the following specifics.

- Task Name: The name of this Task. It is going to be referenced in the following Tasks `Create Team`
- URL: Reference to the flow parameter holding the GitHub API endpoint `$(params.url)`
- Token: Reference to the flow parameter holding the personal access token `$(params.token)`
- Organization Name: Reference to the Flow parameter holding the name of the organization where the team is created `$(params.org)`
- Team Name: The name of the team to be created `Team name testing`
- Level of privacy: Scroll down and select the option `closed`

For the **Json Path To Property** Task, enter the following specifics.

- Task Name: The name of this Task that will be referenced in the following Tasks `Get team name`
- Json: Reference to the previous Task output result `$(Task.Create Team.results.team)`
- Query Expression: JSON path expression pointing to the name of the newly created team `$.name`
- Output Property Key: The output property name under which we will store the newly created team name `createdTeamName`

For the **Invite User to Organization** Task, enter the following specifics.

- Task Name: The name of this Task `Invite User to Organization`
- URL, Token and Organization Name: can be filled in as in the previous Tasks
- User Email Address: Reference to the flow parameter holding the email address of the member that will get invited to the organization `$(Workflow.params.emailAddress)`

For the **Invite Member to Team** Task, enter the following specifics.

- Task Name: The name of this Task `Invite Member to Team`
- URL, Token and Organization Name: can be filled in as in the previous Tasks
- Team Name: Reference to the output parameter defined in the **Json Path To Property** Task `$(Task.Get team name.results.createdTeamName)`

For the **Sleep** Task, enter the following specifics.

- We'll make the Workflow stop for 30 seconds enough time to check the team created in the GitHub UI `30000`.

For the **Delete Team from Organization** Task, enter the following specifics.

- Task Name: The name of this Task `Delete Team`
- URL, Token and Organization Name: can be filled in as in the previous Tasks
- Team Name: Reference to the output parameter defined in the **Json Path To Property** Task `$(Task.Get team name.results.createdTeamName)`

Next, wire these Tasks together by dragging a line from **Start** to **End**, going through the configured Tasks. You need to change the status checks of the links to `logic-success`.

To learn more about this, navigate to _Getting to Know_ | _Editor_ | _Links_ in the Flow documentation.

The last step is to save the Workflow by selecting **Create new version** from the top-right corner.

## Testing your Flow

To test your Workflow, you can run this manually from the Workflows page. Identify the newly created Workflow and select **Run it**. In the Workflow Parameters window, fill in the parameter values and select **Run** or **Run and View**.

![Workflow Parameters](./assets/github-teams-run.png)

Once the Workflow starts, you are redirected to the activity details, depicting the progress of the execution.

![Workflow Running Details](./assets/github-teams-running.png)

When the Workflow diagram shows the **Sleep** Task as running, let's open the Organization Teams GitHub UI page: `https://github.ibm.com/orgs/test-Workflow/teams`. If you defined a different value for the **org** parameter, make sure you replace it in the URL when accessing the GitHub UI.

![GitHub UI Teams page](./assets/github-teams-success-created.png)

Coming back to the Flow Activity UI, the Workflow execution should have reach its end. Checking back the Teams GitHub UI page, you should not see the team in the organization.

![GitHub UI Teams page](./assets/github-teams-completed.png)
