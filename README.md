# AWS RBI Compliance Policies using Cloud Custodian

This repository contains AWS-specific policies designed to ensure compliance with the Reserve Bank of India (RBI) guidelines using Cloud Custodian. The policies are open-sourced under the Apache 2.0 license.

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Defining Policies](#defining-policies)
  - [Validating Policies](#validating-policies)
  - [Executing Policies](#executing-policies)
- [License](#license)

## Overview

Cloud Custodian is a rules engine for managing AWS environments, allowing you to define policies to automate governance and security. This repository contains a set of policies tailored to meet RBI compliance requirements for AWS resources.

## Prerequisites

Before using these policies, ensure you have the following:

- An AWS account with the necessary permissions to create and manage resources.
- Python 3.6 or higher installed on your local machine.
- Cloud Custodian installed. If not, you can install it using pip:

  ```bash
  pip install c7n
  ```

## Installation

1. Clone this repository to your local machine:

   ```bash
   git clone https://github.com/astuto-ai/astuto-cloud-custodian.git
   ```

2. Navigate to the repository directory:

   ```bash
   cd astuto-cloud-custodian
   ```

## Usage

### Defining Policies

Policies are defined in YAML format and are located in the `policies` directory. Each policy file contains one or more policies that specify rules and actions for different AWS resources.

Here is an example policy file (`example-policy.yml`):

```yaml
policies:
  - name: cloudtrail-enabled
    resource: aws.cloudtrail
    filters:
     - type: status
       key: IsLogging
       value: False
```

### Validating Policies

Before executing policies, it is crucial to validate them to ensure they are correctly defined. Use the following command to validate a policy:

```bash
custodian validate -c RBI/AWS/cloudtrail-enabled.yml
```

If the policy is valid, you will see a confirmation message. If there are errors, they will be displayed for you to correct.

### Executing Policies

To execute a policy, use the following command:

```bash
custodian run -c RBI/AWS/cloudtrail-enabled.yml -s output
```

This command runs the specified policy and saves the output in the `output` directory. Review the output logs to verify the policy execution results.

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## References

- (Cloud Custodian)[https://cloudcustodian.io/]
- (Auto Remediation with Cloud Custodian)[https://aws.amazon.com/blogs/opensource/compliance-as-code-and-auto-remediation-with-cloud-custodian/]
- (Custodian GitHub)[https://github.com/cloud-custodian/cloud-custodian]
- (Custodian AWS Examples)[https://cloudcustodian.io/docs/aws/examples/index.html]

