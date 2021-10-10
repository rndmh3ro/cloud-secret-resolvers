[![codecov](https://codecov.io/gh/kubeopsskills/cloud-secret-resolvers/branch/main/graph/badge.svg?token=t65R7COoaz)](https://codecov.io/gh/kubeopsskills/cloud-secret-resolvers)
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-3-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->
# Cloud Secret Resolvers (CSR)

Cloud Secret Resolvers is a set of tools to help your applications (on Kubernetes) to retrieve any credentials from cloud managed vaults without the needed to write additional boilerplate code in your applications!.

<!-- TOC -->

- [Cloud Secret Resolvers (CSR)](#cloud-secret-resolvers-csr)
  - [Installation](#installation)
  - [Using on Kubernetes](#using-on-kubernetes)
  - [How it works](#how-it-works)
  - [Dev tools](#dev-tools)
  - [Contributors ✨](#contributors-)

<!-- /TOC -->

## Installation

Cloud Secret Resolvers is available on Linux, ARM, macOS and Windows platforms.
- Binaries for Linux, ARM, Windows and Mac are available as tarballs in the [release](https://github.com/kubeopsskills/cloud-secret-resolvers/releases) page

## Using on Kubernetes

- AWS
  
  - Prerequisites:
    - Enabled the OIDC provider on your [EKS](https://aws.amazon.com/th/eks/) cluster (https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html)
    - Your application Kubernetes pod has a service account with the following privillege:
       [policy.json](assets/policy.json)
  - Update your application entrypoint as follows:
    ```bash
    #!/bin/bash
    eval $(csr)
    node ... # your application runtime command
    ```
  - Update your application Kubernetes config maps as follows:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: [your config map name]
      namespace: [your config map namespace name]
    data:
    ...
    CLOUD_TYPE: "aws"
    AWS_REGION: "[your AWS region name]"
    AWS_SECRET_NAME: "[your AWS secret name]"
    ```

- Azure
  - Prerequisites:
    1. Install az cli with [link](https://docs.microsoft.com/cli/azure/install-azure-cli)
    2. Go to access policy of your azure key vault and add GET secret permissions for users
    3. Login with `az login` with user from step B.

  - Update your application entrypoint as follows:
    ```bash
    #!/bin/bash
    eval $(csr)
    node ... # your application runtime command
    ```
  - Update your application Kubernetes config maps as follows:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: [your config map name]
      namespace: [your config map namespace name]
    data:
    ...
    CLOUD_TYPE: "azure"
    AZ_REGION: "[your Azure region name]"
    AZ_SUBSCRIPTION_ID: "[your Azure subscription id]"
    AZ_VAULT_NAME: "[your Azure key vault name]"
    ```
  
- Google Cloud
  - Prerequisites:
    - Add Secret Manager Secret Accessor permissions to your secret with "client_email" from Google cloud Credentials Json file
  
  - Update your application entrypoint as follows:
    ```bash
    #!/bin/bash
    eval $(csr)
    node ... # your application runtime command
    ```
  - Update your application Kubernetes config maps as follows:
    ```yaml
    apiVersion: v1
    kind: ConfigMap
    metadata:
      name: [your config map name]
      namespace: [your config map namespace name]
    data:
    ...
    CLOUD_TYPE: "gcloud"
    GOOGLE_PROJECT_ID: "[your Google cloud project Id]" 
    GOOGLE_APPLICATION_CREDENTIALS: "[your Google cloud Credentials Json file path]"
    ```

## How it works
The architecture looks like below.

Internally, the `CSR` find local environment variables in the Kubernetes Pod Container which have Cloud Vault key placeholders for example: export db_username=${db_username}, then the `CSR` will extract db_username as a key and ${db_username} as a value. Finally, the `CSR` will use ${db_username} to match cloud vault key, retrieve cloud vault value, and map the value with db_username local environment.

![Diagram](https://github.com/kubeopsskills/cloud-secret-resolvers/blob/main/assets/diagram.png)

## Dev tools

> install make for using command with `brew install make`

- `make run` for running app
- `make test` for testing
- `make test-coverage` for export test coverage
- `make all` for building app for all OS
- `make clean` for cleaning build app

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://naijab.com"><img src="https://avatars.githubusercontent.com/u/20009757?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Nattapon Pondongnok</b></sub></a><br /><a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=naijab" title="Code">💻</a> <a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=naijab" title="Tests">⚠️</a></td>
    <td align="center"><a href="https://kubeops.guru"><img src="https://avatars.githubusercontent.com/u/18477492?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Saritrat Jirakulphondchai</b></sub></a><br /><a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=Sikiryl" title="Code">💻</a> <a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=Sikiryl" title="Tests">⚠️</a> <a href="https://github.com/kubeopsskills/cloud-secret-resolvers/pulls?q=is%3Apr+reviewed-by%3ASikiryl" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="https://www.kubeops.guru"><img src="https://avatars.githubusercontent.com/u/4091492?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sirinat Paphatsirinatthi</b></sub></a><br /><a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=dmakeroam" title="Code">💻</a> <a href="https://github.com/kubeopsskills/cloud-secret-resolvers/commits?author=dmakeroam" title="Tests">⚠️</a> <a href="https://github.com/kubeopsskills/cloud-secret-resolvers/pulls?q=is%3Apr+reviewed-by%3Admakeroam" title="Reviewed Pull Requests">👀</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!