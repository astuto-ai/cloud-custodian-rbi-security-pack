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

Each policy execution create 3 files in the output directory. 
  - custodian-run.log
  - metadata.json
  - resources.json


### 1. `custodian-run.log`
The `custodian-run.log` file is a log file that captures the detailed execution process of the policy. This file typically includes:

- **Start and End Time:** When the policy execution started and finished.
- **Policy Information:** Details about the policy being executed, including its name and parameters.
- **Execution Steps:** A step-by-step record of actions taken by the policy, such as resource filtering, actions performed, and any errors encountered.
- **Summary:** A summary of the execution, including the number of resources processed and any issues or notable events during the run.

```yaml
2024-05-20 14:48:20,823 - custodian.policy - INFO - policy:cloudtrail-enabled resource:aws.cloudtrail region:ap-south-1 count:1 time:2.11
```
### 2. `metadata.json`
The `metadata.json` file contains metadata about the policy execution. This file usually includes:

- **Policy Details:** The name, description, and configuration of the policy.
- **Execution Context:** Information about the environment in which the policy was run, such as the account, region, and execution timestamp.
- **Resource Information:** High-level information about the resources targeted by the policy, such as the resource types and counts.
- **Execution Metrics:** Metrics and statistics about the policy run, such as the duration of the run and resource counts before and after filtering.

```json
{
  "policy": {
    "name": "cloudtrail-enabled",
    "resource": "aws.cloudtrail",
    "filters": [
      {
        "type": "status",
        "key": "IsLogging",
        "value": false
      }
    ]
  },
  "version": "0.9.35",
  "execution": {
    "id": "f963b85d-aefb-4589-b95b-b16d106e1822",
    "start": 1716196698.6789637,
    "end_time": 1716196700.8230755,
    "duration": 2.1441118717193604
  },
  "config": {
    "region": "ap-south-1",
    "regions": [
      "ap-south-1"
    ],
    "cache": "~/.cache/cloud-custodian.cache",
    "profile": null,
    "account_id": "473332792234",
    "assume_role": null,
    "external_id": null,
    "log_group": null,
    "tracer": null,
    "metrics_enabled": null,
    "metrics": null,
    "output_dir": ".",
    "cache_period": 15,
    "dryrun": true,
    "authorization_file": null,
    "subparser": "run",
    "config": null,
    "configs": [
      "cloudtrail-enabled.yml"
    ],
    "policy_filters": [],
    "resource_types": [],
    "verbose": null,
    "quiet": null,
    "debug": false,
    "skip_validation": false,
    "command": "c7n.commands.run",
    "vars": null
  },
  "sys-stats": {
    "snapshot_time": 2.152089834213257,
    "num_handles": 5,
    "cpu_user": 0.25,
    "cpu_system": 0.484375,
    "num_ctx_switches_voluntary": 1073,
    "read_count": 438873,
    "write_count": 37,
    "write_bytes": 63890,
    "read_bytes": 1865395,
    "rss": 8634368,
    "vms": 7438336
  },
  "api-stats": {
    "cloudtrail.DescribeTrails": 1,
    "tagging.GetResources": 1,
    "cloudtrail.GetTrailStatus": 3
  },
  "metrics": [
    {
      "MetricName": "ResourceCount",
      "Timestamp": "2024-05-20T14:48:20.823075",
      "Value": 1,
      "Unit": "Count"
    },
    {
      "MetricName": "ResourceTime",
      "Timestamp": "2024-05-20T14:48:20.823075",
      "Value": 2.1129376888275146,
      "Unit": "Seconds"
    }
  ]
}
```

### 3. `resources.json`
The `resources.json` file contains detailed information about the resources that were processed by the policy. This file typically includes:

- **Resource Data:** Comprehensive data for each resource that matched the policy filters, including resource IDs, attributes, and tags.
- **Policy Actions:** Details about any actions taken on each resource by the policy, such as notifications sent, tags added, or resources stopped/terminated.
- **Resource State:** The state of each resource before and after the policy execution, which helps in understanding the impact of the policy actions.

These files together provide a complete picture of the policy execution, from logging and metadata to detailed resource information. They are essential for auditing, debugging, and understanding the impact of the policies enforced by Cloud Custodian.

```json
[
  {
    "Name": "cost-trail",
    "S3BucketName": "aws-cloudtrail-logs-473332792234-f692c55d",
    "IncludeGlobalServiceEvents": true,
    "IsMultiRegionTrail": true,
    "HomeRegion": "ap-south-1",
    "TrailARN": "arn:aws:cloudtrail:ap-south-1:473332792234:trail/cost-trail",
    "LogFileValidationEnabled": true,
    "KmsKeyId": "arn:aws:kms:ap-south-1:473332792234:key/512ee8bc-865e-4b5b-baac-01b0e24ff019",
    "HasCustomEventSelectors": true,
    "HasInsightSelectors": false,
    "IsOrganizationTrail": false,
    "Tags": [],
    "c7n:TrailStatus": {
      "IsLogging": false,
      "LatestDeliveryTime": "2024-05-20T14:47:26.853000+05:30",
      "StartLoggingTime": "2024-05-17T17:42:45.180000+05:30",
      "StopLoggingTime": "2024-05-20T14:48:05.208000+05:30",
      "LatestDigestDeliveryTime": "2024-05-20T14:32:50.270000+05:30",
      "LatestDeliveryAttemptTime": "2024-05-20T09:17:26Z",
      "LatestNotificationAttemptTime": "",
      "LatestNotificationAttemptSucceeded": "",
      "LatestDeliveryAttemptSucceeded": "2024-05-20T09:17:26Z",
      "TimeLoggingStarted": "2024-05-17T12:12:45Z",
      "TimeLoggingStopped": "2024-05-20T09:18:05Z"
    }
  }
]
```

## License

This project is licensed under the Apache License 2.0. See the [LICENSE](LICENSE) file for details.

## References

- (Cloud Custodian)[https://cloudcustodian.io/]
- (Auto Remediation with Cloud Custodian)[https://aws.amazon.com/blogs/opensource/compliance-as-code-and-auto-remediation-with-cloud-custodian/]
- (Custodian GitHub)[https://github.com/cloud-custodian/cloud-custodian]
- (Custodian AWS Examples)[https://cloudcustodian.io/docs/aws/examples/index.html]

