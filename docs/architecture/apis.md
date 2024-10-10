---
title: APIs
order: 4
---

# API Architecture

Boomerang Flow exposes a series of RESTFul APIs that allows the query and management of teams, users, and workflows as well as drive extensibility.

These include for example:

- User Management (List, Create, update and delete users)
- Team Management (List, Create, update and delete teams)
- Workflow Management (List Boomerang Flow team workflows and system workflows)
- Activity Management (Submit requests to execute workflows and provide the ability to search and retrieve workflow activities)

Available APIs can be viewed in the online Boomerang Flow API viewer at (https://try.useboomerang.io/flow/apis/docs/swagger-ui/index.html?url=/flow/apis/docs/api).

## Security

All Flow APIs calls are required by authenticated with an appropriately scoped x-access-token in the HTTP header of each request.

Please refer to [security architecture](../architecture/security#tokens) for further guidance and API documentation for the required token scope for each API call.

## OpenAPI specification

The APIs are documented using the OpenAPI specification 3.0.1 and can be found at (https://try.useboomerang.io/flow/apis/docs/api). Open API specifications are used to generate client stubs for a variety of languages, one such open source project to facilitate this is the open API-generator project. (https://github.com/OpenAPITools/openapi-generator).

## Versioning

To make it easier to evolve and extend are APIs we support a versioned API, such as `/apis/v1`, and is done at the API level rather than a particular context to ensure we have a consistent view and behavior for the life of a version.

## SDKs

We currently generate a Java client based on the OpenAPI specification.

_In the future we will also support NodeJS and Go._

## Limits

Our API endpoints have limits on the number of requests that can be made with the following defaults that can be edited in the Ingress definition.

| Type                      | Description                                                       | Default |
| ------------------------- | ----------------------------------------------------------------- | ------- |
| Connections               | Number of concurrent connections allowed from a single IP address | 3       |
| Requests Per Minute (rpm) | Number of requests accepted from a given IP each minute           | 100     |

_Reference_: https://kubernetes.github.io/ingress-nginx/user-guide/nginx-configuration/annotations/#rate-limiting
