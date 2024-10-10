---
title: Security
order: 3
---

# Security Architecture

Boomerang Flow has a number of security protocols in place. The following architecture information provides details on how these are applicable.

## Authentication

Boomerang Flow relies on the Boomerang `OAuth Proxy` (a fork of `oauth2_proxy`) to provide user authentication in the form of basic authentication or integrated with a provider such as GitHub<sup>®</sup> or Google.

There are also Tokens for securing API endpoints and Workflows by either Global, Team, or Workflow tokens.

### Tokens

Tokens are used to secure the API endpoints as well as Workflow triggers. There are two formats of Tokens currently implemented in Boomerang Flow;

- The legacy token format is currently used by Workflow Triggers and the _deprecated_ System Tokens
- The new token format used by our API Tokens which have identifiable prefixes of `bfg_` for Global and `bft_` for Team tokens.

> You can learn more about the new Token format by reading this [GitHub Blog](https://github.blog/2021-04-05-behind-githubs-new-authentication-token-formats/) or by [understanding our scoping implementation](https://github.com/boomerang-io/flow.service.workflow).

#### API Tokens

API Tokens can be of two different scopes, easily identified by their Token prefix.

| Scope  | Token Prefix | Access                                                                                     |
| ------ | ------------ | ------------------------------------------------------------------------------------------ |
| Global | `bfg_`       | Allows you to retrieve information or perform an action on any team, user, or workflow.    |
| Team   | `bft_`       | Allows you to retrieve information perform an action of a specific team or team workflows. |

#### Workflow Tokens

Workflow tokens are used by the eventing framework to trigger workflows or to respond to certain actions.

> In time these will migrate to be a scoped token called via the APIs and be prefixed with `bfw_` for Workflow tokens.

#### System Tokens [Deprecated]

These are used to protect internally available endpoints, that have no available ingress, for integration with other applications that are internal to the Kubernetes cluster.

## Authorization

Authorization is split into Global roles and also Team Entitlements.

**Roles**

| Level  | Role     | Access                                                                       |
| ------ | -------- | ---------------------------------------------------------------------------- |
| global | admin    | Full access to Flow: Ability to access and manage all teams and system       |
| global | operator | Access to Flow across teams: Ability to access and manage all teams          |
| global | user     | No access unless in a team with an entitlement                               |
| team   | user     | Default role within a team: Can do most functions other than Flow management |

## Access control

| Interface                | Administrator (Global) | Operator (Global) | User (Global) | User (Team)    |
| ------------------------ | ---------------------- | ----------------- | ------------- | -------------- |
| Workflows                | View & Execute         | View & Execute    | -             | View & Execute |
| Actions                  | View & Execute         | View & Execute    | -             | View & Execute |
| Activity                 | View                   | View              | View          | View           |
| Insights                 | View                   | View              | View          | View           |
| Manage - Team Parameters | Edit                   | Edit              | -             | -              |
| Manage - Team Tasks      | Edit                   | Edit              | -             | -              |
| Manage - Team Tokens     | Edit                   | Edit              | -             | -              |
| Admin - Teams            | Edit                   | Edit              | -             | -              |
| Admin - Users            | Edit                   | Edit              | -             | -              |
| Admin - Parameters       | Edit                   | Edit              | -             | -              |
| Admin - Tokens           | Edit                   | Edit              | -             | -              |
| Admin - Quotas           | Edit                   | Edit              | -             | -              |
| Admin - Settings         | Edit                   | Edit              | -             | -              |
| Admin - Task Manaers     | Edit                   | Edit              | -             | -              |
| Admin - System Workflows | Edit                   | Edit              | -             | -              |

## Audit

Only available when integrated to IBM Consulting Essentials.

## SSL certificates

Expects an SSL certificate for HTTPS ingress. This can be configured as per your Kubernetes<sup>®</sup> configuration.

## Data

Data is stored in the following locations:

- Activity Logs - Kubernetes ephemeral storage and optionally ingested by a chosen logging implementation such as Elastic<sup>®</sup> or Loki
- Audit Logs - MongoDB<sup>®</sup>
- Application Data - MongoDB
- Workflow Cache - Kubernetes Persistent Volumes (PV)

The following table provides an overview of the data management profile:

| Data Entity      | Storage        | PII | Customer Data | Source Code | Encrypted at Rest | Encrypted in Flight |
| ---------------- | -------------- | --- | ------------- | ----------- | ----------------- | ------------------- |
| Activity Logs    | File           | N   | N             | Y           | Y (\*\*)          | Y                   |
| Activity Logs    | Elastic \ Loki | N   | N             | Y           | Y (\*)            | Y                   |
| Audit Logs       | MongoDB        | Y   | N             | N           | Y (\*)            | Y                   |
| Application Data | MongoDB        | Y   | Y             | N           | Y (\*)            | Y (\*)              |
| Workflow Cache   | Kubernetes PV  | N   | N             | Y           | N                 | N                   |

(\*) App-level encryption (\*\*) Disk-level encryption

### Retention

Data is backed up based on a chosen mechanism. We recommend Velero<sup>®</sup> and Restic.

It is retained based on the configurations of the various tools.

## Encryption

Encryption can be implemented at two distinct layers and depends on what is configured at installation time and the underlying Kubernetes.

- Network or in-flight traffic can be encrypted through configuration of the Kubernetes networking layer and the exchange of SSL certificates.
- Data or at-rest components can be encrypted through configuration of the persistence volume layer and a passphrase.

## Kubernetes policies

There are a number of policy types in Kubernetes that help ensure security. We leverage a number of these.

### Pod security policy

Pod Security Policies (PSP) / Security Context Constraints (SCC) enable fine-grained authorization and define a set of conditions that a pod must run with in order to be accepted into the system. From host access to Linux<sup>®</sup> capabilities and privileges. See [Kubernetes Pod Security Policies](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) for more information.

By default all pods run under `privileged` policy. You can also define a default policy at the namespace level. Flow takes care of creating the Kubernetes Service Accounts and role bindings to the elevated policy at install time.

### Image Policies

We recommended leveraging cluster-wide image policies with a list of approved containers that can be accessed.

### Roles

The controller is the Kubernetes orchestrator. It needs elevated permissions to be able to communicate with the Kubernetes API. To achieve this, we define a Custom role with the appropriate Service account bound to this role. The controller microservice runs as this Service account.

See the Helm<sup>®</sup> chart for further in-depth detail as to what Resource Groups and Verbs this role utilizes.

### Task

The task is the Tekton TaskRuns that runs and executes the container. It needs some elevated permissions to be created and bound to the `configmap` and Persistent Volume Claim (PVC). To achieve this, we have a Service account and a binding to a predefined role with only the required abilities.

Every Task is a self-contained short-living execution runner. All artifacts and secure values are only pulled into the Task as it is executing. The Task is then removed upon completion or during scheduled cleanups, based on installed properties.
