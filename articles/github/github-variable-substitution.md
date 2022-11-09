--- 
title: Use variable substitution with GitHub Actions for Azure
description: GitHub Action for substituting variables in parameterized files
author: juliakm
ms.author: jukullam 
ms.topic: conceptual
ms.service: azure
ms.date: 01/25/2021
ms.custom: github-actions-azure
---

# Use variable substitution with GitHub Actions

Learn how to use [variable substitution action](https://github.com/microsoft/variable-substitution) to replace values in XML, JSON and YAML based configuration and parameter files.

Variable substitution lets you insert values, including [GitHub secrets](https://docs.github.com/en/actions/reference/encrypted-secrets), into files in your repository during the workflow run. For example, you could insert an API login and password into a JSON file during the workflow run.

Variable substitution only works for keys predefined in the object hierarchy. You cannot create new keys with variable substitution. In addition, only variables defined as [environment variables](https://docs.github.com/en/actions/reference/environment-variables) in the workflow or system variables that are already available can be used for substitution.

## Prerequisites

- A GitHub account. If you don't have one, sign up for [free](https://github.com/join).  

## Use the variable substitution action

This example walks through replacing values in `employee.json` using the [variable substitution action](https://github.com/microsoft/variable-substitution).

1. Create `employee.json` at the root level of your repository.

    ```json
    {
        "first-name": "Toni",
        "last-name": "Cranz",
        "username": "",
        "password": "",
        "url": ""
    }
    ```

1. Open your GitHub repository and go to **Settings**.

    :::image type="content" source="media/github-repo-settings.png" alt-text="Select Settings in the navigation":::

1. Select **Security > Secrets and variables > Actions**.

1. Select **New repository secret**.

1. Add a new secret `PASSWORD` with the value `5v{W<$2B<GR2=t4#` (or a password you select). Save your secret. 

1. Go to **Actions** and select **set up a workflow yourself**.

1. Add a workflow file. The username value in your json file will be replaced with `tcranz`. The password will be replaced with your GitHub secret. The url field will be populated with a URL that includes the GitHub variable `github.repository`.

    ```yaml
    on: [push]
    name: variable substitution in json

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - uses: actions/checkout@v2
        - uses: microsoft/variable-substitution@v1 
        with:
            files: 'employee.json'
        env:
            username: tcranz
            password: ${{ secrets.PASSWORD }}
            url: https://github.com/${{github.repository}}

    ```

7. Go to **Actions** to see your workflow run. Open the variable substitution action. You should see that each variable was replaced.

    ```text
    SubstitutingValueonKeyWithString username tcranz
    SubstitutingValueonKeyWithString password ***
    SubstitutingValueonKeyWithString url https://github.com/account/variable-sub
    Successfully updated file: employee.json
    ```

## Clean up resources

Delete your GitHub repository when it is no longer needed.

## Next steps

> [!div class="nextstepaction"]
> [Deploy to Azure Web Apps using GitHub Actions](/azure/app-service/deploy-github-actions)
