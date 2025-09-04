# Reference: Blueprint Specification

A `blueprint.cx.yaml` file is the executable heart of a blueprint package. It is a declarative YAML document that defines how the Contextually runtime engine should interact with an external service.

This document serves as the technical reference for all valid keys.

---

## Top-Level Keys

The following keys are the top-level attributes of a `blueprint.cx.yaml` file.

| Key                      | Type   | Required | Description                                                                                                       |
| ------------------------ | ------ | -------- | ----------------------------------------------------------------------------------------------------------------- |
| `id`                     | String | Yes      | A unique identifier for the blueprint, e.g., `blueprint:system-mssql`.                                            |
| `name`                   | String | Yes      | The human-readable name of the API, e.g., "Microsoft SQL Server".                                                 |
| `version`                | String | Yes      | The semantic version of this blueprint package, e.g., `0.2.1`. This must match the release tag.                   |
| `connector_provider_key` | String | Yes      | The key for the runtime strategy to use, e.g., `rest-declarative`, `sql-mssql`.                                   |
| `supported_auth_methods` | List   | No       | A list of authentication methods this blueprint supports. This contract drives the `cx connection create` wizard. |
| `browse_config`          | Object | Yes      | The core configuration for API interaction, including the base URL and action templates.                          |
| `test_connection_config` | Object | No       | An optional dictionary defining how to test a connection to this service.                                         |
| `oauth_config`           | Object | No       | Configuration for the `oauth2-declarative` provider, such as the `token_url`.                                     |

---

## `supported_auth_methods`

This section is a list of objects, where each object defines a complete, self-contained authentication method that a blueprint supports. This contract is what drives the interactive `cx connection create` command.

### SupportedAuthMethod Object

| Key            | Type   | Required | Description                                                                     |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------- |
| `type`         | String | Yes      | A unique identifier for this method within the blueprint (e.g., `credentials`). |
| `display_name` | String | Yes      | A human-friendly name shown to the user (e.g., "Username & Password").          |
| `fields`       | List   | Yes      | A list of `AuthField` objects that must be collected from the user.             |

### AuthField Object

| Key           | Type                 | Required | Description                                                                            |
| ------------- | -------------------- | -------- | -------------------------------------------------------------------------------------- |
| `name`        | String               | Yes      | The key for this field (e.g., `server`, `api_key`).                                    |
| `label`       | String               | Yes      | The human-friendly prompt for this field (e.g., "Server Address").                     |
| `type`        | `detail` \| `secret` | Yes      | `detail` is stored in `.conn.yaml`, `secret` is stored in `.secret.env`.               |
| `is_password` | Boolean              | No       | If `true`, the interactive CLI prompt will mask the user's input. Defaults to `false`. |

#### Example

```yaml
supported_auth_methods:
  - type: credentials
    display_name: "Username & Password"
    fields:
      - name: server
        label: "Server Address (hostname or IP)"
        type: detail
      - name: database
        label: "Database Name"
        type: detail
      - name: username
        label: "Username"
        type: secret
      - name: password
        label: "Password"
        type: secret
        is_password: true
```

---

## `browse_config`

This is the most important section of the blueprint for REST APIs. It defines the API's base URL and all the available actions.

| Key                 | Type   | Required | Description                                                                           |
| ------------------- | ------ | -------- | ------------------------------------------------------------------------------------- |
| `base_url_template` | String | Yes      | The base URL for all API calls, e.g., `https://api.example.com/v2`.                   |
| `action_templates`  | Object | Yes      | A dictionary where each key is a user-facing action name mapping to an action object. |

### `action_templates`

Each key under `action_templates` is a unique, user-facing verb (often the `operationId` from an OpenAPI spec). The value is an object defining how to execute that action.

| Key                   | Type   | Required       | Description                                                                                                                                              |
| --------------------- | ------ | -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `http_method`         | String | Yes (for REST) | The HTTP method for the request (e.g., `GET`, `POST`, `PUT`, `DELETE`).                                                                                  |
| `api_endpoint`        | String | Yes (for REST) | The URL path for the action, relative to `base_url_template`. May contain Jinja2 variables from the flow's `context`, e.g., `/pets/{{ context.petId }}`. |
| `payload_constructor` | Object | No             | For `POST` or `PUT` requests, this block defines the Pydantic model used to construct and validate the request body from the flow's `context`.           |
| `run`                 | Object | No             | For non-REST actions (like SQL), this block explicitly defines the action to execute. e.g., `run: { action: "run_sql_query" }`.                          |

### `payload_constructor` (v0.2.1+)

This block is the heart of the declarative payload engine. It tells the engine how to build and validate the JSON body for a request.

| Key      | Type   | Required | Description                                                                                                                                                                                                                               |
| -------- | ------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `_model` | String | Yes      | The fully-qualified name of the Pydantic model in `schemas.py` that represents the final API payload (e.g., `schemas.SendGridMailPayload`). The engine will use this to validate the `context` from the flow step before making the call. |

!!! success "Schema-Driven Transformation"
The `cx` engine is intelligent. It uses the target Pydantic model (`_model`) to automatically transform the data provided in a flow's `context`. For example, if a model field is `to: List[EmailObject]` and the user provides a simple list of strings, the engine will automatically convert it to the required `[{'email': '...'}]` structure. This keeps flow files simple and blueprints clean.

#### REST API Example

```yaml
browse_config:
  base_url_template: https://api.example.com/v1
  action_templates:
    # A GET action with a dynamic path parameter from the context.
    getUser:
      http_method: GET
      api_endpoint: /users/{{ context.userId }}

    # A POST action with a request body.
    # The flow's 'context' block must provide data that can be
    # validated against the 'schemas.User' Pydantic model.
    createUser:
      http_method: POST
      api_endpoint: /users
      payload_constructor:
        _model: schemas.User
```

#### SQL Example

```yaml
browse_config:
  action_templates:
    query:
      run:
        action: "run_sql_query"
      description: "Executes a SQL query against the connected database."
```
