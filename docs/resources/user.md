---
# generated by https://github.com/hashicorp/terraform-plugin-docs
page_title: "opensearch_user Resource - terraform-provider-opensearch"
subcategory: ""
description: |-
  Provides an OpenSearch security user. Please refer to the OpenSearch Access Control documentation for details.
---

# opensearch_user (Resource)

Provides an OpenSearch security user. Please refer to the OpenSearch Access Control documentation for details.

## Example Usage

```terraform
# Create a user
resource "opensearch_user" "mapper" {
  username    = "app-reasdder"
  password    = "SuperSekret123!"
  description = "a reader role for our app"
}

# And a full user, role and role mapping example:
resource "opensearch_role" "reader" {
  role_name   = "app_reader"
  description = "App Reader Role"

  index_permissions {
    index_patterns  = ["app-*"]
    allowed_actions = ["get", "read", "search"]
  }
}

resource "opensearch_user" "reader" {
  username = "app-reader"
  password = var.password
}

resource "opensearch_roles_mapping" "reader" {
  role_name   = opensearch_role.reader.id
  description = "App Reader Role"
  users       = [opensearch_user.reader.id]
}
```

<!-- schema generated by tfplugindocs -->
## Schema

### Required

- `username` (String) The name of the security user.

### Optional

- `attributes` (Map of String) A map of arbitrary key value string pairs stored alongside of users.
- `backend_roles` (Set of String) A list of backend roles.
- `description` (String) Description of the user.
- `password` (String, Sensitive) The plain text password for the user, cannot be specified with `password_hash`. Some implementations may enforce a password policy. Invalid passwords may cause a non-descriptive HTTP 400 Bad Request error. For AWS Opensearch domains "password must be at least 8 characters long and contain at least one uppercase letter, one lowercase letter, one digit, and one special character".
- `password_hash` (String, Sensitive) The pre-hashed password for the user, cannot be specified with `password`.

### Read-Only

- `id` (String) The ID of this resource.

## Import

Import is supported using the following syntax:

```shell
terraform import opensearch_user.reader app_reader
```