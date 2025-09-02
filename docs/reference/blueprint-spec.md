# Reference: Blueprint Specification

A `blueprint.cx.yaml` file is the executable heart of a blueprint package. It is a declarative YAML document that defines how the Contextually runtime engine should interact with an external service.

This document serves as the technical reference for all valid top-level and nested keys.

---

## Top-Level Keys

| Key                      | Type   | Required | Description                                                                                                |
| ------------------------ | ------ | -------- | ---------------------------------------------------------------------------------------------------------- |
| `id`                     | String | Yes      | A unique identifier for the blueprint, e.g., `blueprint:swagger-petstore`.                                 |
| `name`                   | String | Yes      | The human-readable name of the API, e.g., "Swagger Petstore".                                              |
| `version`                | String | Yes      | The semantic version of this blueprint package, e.g., `1.0.7`.                                             |
| `connector_provider_key` | String | Yes      | The key for the runtime strategy to use, e.g., `rest-declarative`, `oauth2-declarative`, `sql-postgresql`. |
| `auth_config`            | Object | No       | A dictionary defining the authentication method. Defaults to `type: none`.                                 |
| `browse_config`          | Object | Yes      | The core configuration for API interaction, including the base URL and action templates.                   |
| `test_connection_config` | Object | No       | An optional dictionary defining how to test a connection to this service.                                  |

---

## `browse_config`

This is the most important section of the blueprint. It defines the API's base URL and all the available actions.

| Key                 | Type   | Required | Description                                                                  |
| ------------------- | ------ | -------- | ---------------------------------------------------------------------------- |
| `base_url_template` | String | Yes      | The base URL for all API calls, e.g., `https://api.example.com/v2`.          |
| `action_templates`  | Object | Yes      | A dictionary where each key is an `operationId` mapping to an action object. |

### `action_templates`

Each key under `action_templates` is a unique `operationId` from the source specification (e.g., `getPetById`). The value is an object defining how to execute that action.

| Key                   | Type   | Required | Description                                                                                                                                               |
| --------------------- | ------ | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `http_method`         | String | Yes      | The HTTP method to use for the request (e.g., `GET`, `POST`, `PUT`, `DELETE`).                                                                            |
| `api_endpoint`        | String | Yes      | The URL path for the action, relative to `base_url_template`. May contain Jinja2 variables, e.g., `/pets/{{ petId }}`.                                    |
| `parameters_model`    | String | No       | The fully-qualified name of the Pydantic model in `schemas.py` used to validate path, query, and header parameters. e.g., `schemas.GetPetByIdParameters`. |
| `payload_constructor` | Object | No       | An object defining the Pydantic model used to construct the JSON body for `POST` or `PUT` requests. e.g., `_constructor: schemas.Pet`.                    |

### Example

Here is an excerpt from a complete `blueprint.cx.yaml` showing these concepts together.

```yaml
id: blueprint:swagger-petstore
name: Swagger Petstore
version: 1.0.7
connector_provider_key: rest-declarative
auth_config:
  type: none
browse_config:
  base_url_template: https://petstore.swagger.io/v2
  action_templates:
    # An action with path parameters, validated by a Pydantic model
    getPetById:
      http_method: GET
      api_endpoint: /pet/{{ petId }}
      parameters_model: schemas.GetpetbyidParameters

    # An action with a request body, constructed from a Pydantic model
    addPet:
      http_method: POST
      api_endpoint: /pet
      payload_constructor:
        _constructor: schemas.Pet
```
