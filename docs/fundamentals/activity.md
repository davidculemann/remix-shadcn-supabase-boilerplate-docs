---
title: Activity
order: 3
---

# Activity

Activity allows you to view each individual execution of a Workflow. Each time you execute a Workflow, an activity is associated with this execution. The system keeps information on Workflow status, Workflow duration, Task status, Task duration, output properties, and logs.

## Activity snapshot

Select **View Activity** from the overflow menu on any Workflow to access the Activity page.

![Workflow dropdown](./assets/img/workflow-tile-dropdown.png)

The Activity page provides you with a snapshot breakdown with the ability to filter by:

- status
- team
- Workflow
- trigger method
- date range

![View Activity](./assets/img/workflow-view-activity.png)

Additional information for start time and duration will be displayed, along with visual markers to match the status. The **In Progress** tab will pulsate through a range of the status colors.

Click an individual activity card to view the activity execution detail.

## Activity execution detail

The activity execution detail is a read-only view of your Workflow design with a visual indication of both the status of each Task, as well as the link state.

In addition to accessing activity execution detail from the Activity page, activity execution detail is also accessed when the Workflow is run. To do so:

1. Manually execute a Workflow. Click `Run It` on the Workflow tile.

2. An Execute Workflow modal displays with two options. Click `Run and View`, which takes you through to the detailed Activity Run screen, where you can view the Workflow progress.

![Activity Overview](./assets/img/activity-run.png)

Workflow metadata is displayed in the header.

On the left, you are provided with a panel with the Workflow status and duration, as well as a breakdown of each Task that was executed.

In this Task breakdown, you can access actions depending on the Task type.

- For any Task or Custom Task you can view logs and output properties.
- For System Tasks, you will be provided with a range of specific actions such as **Action Approval**.
