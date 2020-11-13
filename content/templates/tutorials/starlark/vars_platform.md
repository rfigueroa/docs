---
title: "Platform Vars"
linkTitle: "Platform Vars"
description: >
  Example Starlark template with platform vars.
---

{{% alert color="note" %}}
We recommend reviewing [Starlark Spec](https://github.com/bazelbuild/starlark/blob/master/spec.md) before attempting to create a template.
{{% /alert %}}

## Overview

From [Platform vars](/docs/templates/tutorials/starlark/#platform-variables):

Platform variables can be referenced with the following syntax:

`ctx['vela']['<resource>']['<name>']`

## Sample

Let's take a look at using a function within a template:

```star
def main(ctx):
    return {
        'version': '1',
        'steps': [
            step(ctx["vela"]["repo"]["full_name"]),
        ],
    }

def step(full_name):
    return {
        "name": "echo %s" % full_name,
        "image": "alpine:latest",
        'commands': [
            "echo %s" % full_name
        ]
    }

```

The caller of this template could look like:

```yaml
version: "1"
templates:
  - name: sample
    source: github.com/<org>/<repo>/path/to/file/<template>.star
    format: starlark
    type: github

steps:
  - name: build
    template:  
      name: sample
      vars: 
```

Which means the compiled pipeline for execution on a worker is:

```yaml
version: 1
steps:
  - name: sample_echo octocat/hello-world
    image: alpine:latest
    commands:
      - echo octocat/hello-world
```
