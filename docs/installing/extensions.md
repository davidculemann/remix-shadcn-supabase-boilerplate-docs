---
title: Installing Extensions
order: 2
---

# Installing and Configuring Extensions

This section provides details on how to configure extensions within your system.

## Example Extension

To configure an example extension, follow the steps below.

### Preparation

Replace the placeholders in the provided configuration file, such as URLs and API tokens.

```YAML
example_extension:
  name: Sample Extension
  description: Trigger automation workflows.
  url: https://<URL_REPLACE_ME>/api/extension/trigger?token=<TOKEN_REPLACE_ME>
  commands:
    - command: /start
      description: Start a workflow
```