---
title: Workflows
order: 5
---

# Workflows

Learn how to create and manage workflows.

## Workflow Editor

Use the Workflow Editor to design and manage workflows.

### Features

- **Drag-and-drop**: Easily arrange tasks.
- **Dynamic parameter resolution**: Automatically resolve parameters.
- **Conditional switches**: Add conditional logic.
- **Triggers**: Set up triggers for workflow execution.
- **Schedules**: Schedule workflows to run at specific times.
- **Workspaces**: Organize workflows into workspaces.
- **Labels**: Tag workflows for easy identification.
- **Parallelism**: Execute tasks in parallel.
- **Notes**: Add notes to workflows.
- **Change log**: Track changes to workflows.

## Workflow Table

| Feature            | Description                          |
|--------------------|--------------------------------------|
| Drag-and-drop      | Arrange tasks visually               |
| Dynamic parameters | Resolve parameters automatically     |
| Conditional logic  | Add if-else conditions               |
| Triggers           | Set up execution triggers            |
| Schedules          | Schedule workflows                   |
| Workspaces         | Organize workflows                   |
| Labels             | Tag workflows                        |
| Parallel execution | Run tasks in parallel                |
| Notes              | Add workflow notes                   |
| Change log         | Track workflow changes               |

## Example Workflow

```yaml
name: Example Workflow
tasks:
  - name: Task 1
    type: manual
  - name: Task 2
    type: automated
    dependsOn: Task 1