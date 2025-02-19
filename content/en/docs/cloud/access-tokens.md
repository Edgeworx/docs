---
title: "Access tokens"
weight: 800
aliases:
  - /edgeworx/edgeworx-cloud/access-tokens
---

Edgeworx Cloud supports basic HTTP authentication through access tokens.

Simply pass the token as username in
the [HTTP Basic authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) header.
If you are doing the base64 encoding yourself, don't forget to append a single `:` at the end of
your token before encoding.

## Usage

- Inside a URL:

```text
https://<access_token>@api.edgeworx.io/v1/...
```

- As an HTTP header (base64 encoded)

```text
window.fetch(
    "https://api.edgeworx.io/v1/account/penny.lim@edge-ample.io",
    { headers:
        { Authorization: "Basic " + window.btoa("<access_token>:")}
    }
)
```

- As a login option in edgectl or for a specific command

```bash
edgectl login --token <access_token>
edgectl --token <access_token> account get <your_email>
```

### Personal Access Token

Personal Access Tokens have the same privilege as the user they represent and have a lifecycle
connected to the user account.

![Personal Access Tokens](</images/access_token_modal.png>)

### Project Access Token

[Project](../more/terminology#project) Access Tokens have full access to the project they represent and have a lifecycle connected
to their project. Those are the token used in the [node register script](../more/terminology#node-install-script).

![Project Access Token](</images/project_access_token_modal.png>)

### Third Party Access Tokens

You can create third party access tokens both at a project level and at an account level.

3rd party access token have a label and can be revoked at any time. Those are useful for using
edgectl in a CI/CD environment or share access to a specific project to another account.
