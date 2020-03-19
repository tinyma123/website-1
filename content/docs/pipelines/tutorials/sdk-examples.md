+++
title = "Using the Kubeflow Pipelines SDK"
description = "How to use the Kubeflow Pipelines API to upload a local file to create a new pipeline version"
weight = 1
+++

This guide provides examples that demonstrate how to use the Kubeflow Pipelines SDK.

## Before you start

To follow the example, you must have installed Kubeflow Pipelines SDK version 0.2.5 or higher. Use the following instructions to install the Kubeflow Pipelines SDK and check the SDK version.

* Install the [Kubeflow Pipelines SDK](/docs/pipelines/sdk/install-sdk/)
* Run the following command to check the version of the SDK
```
pip list | grep kfp
```
The response should be something like this:
```
kfp                      0.2.5
kfp-server-api           0.2.5
```

## Examples

Use the following examples to learn more about the Kubeflow Pipelines SDK.

### Using the SDK to create a pipeline and a pipeline version

The following example demonstrates how to use the Kubeflow Pipelines SDK to create a pipeline and a pipeline version.

In the example, we first use KFP API client to create a pipeline from a local file. When the pipeline is created, a default pipeline version is created under it automatically. Then, we again use KFP API client to create a new pipeline version from a local file, and the new version will be under the pipeline that just get created.

```python
import kfp
import os

host = <host>
pipeline_file_path = <path to pipeline file>
pipeline_name = <pipeline name>
pipeline_version_file_path = <path to pipeline version file>
pipeline_version_name = <pipeline version name>


if __name__ == '__main__':
  client = kfp.Client(host)
  pipeline_file = os.path.join(pipeline_file_path)
  pipeline = client.pipeline_uploads.upload_pipeline(pipeline_file, name=pipeline_name)
  pipeline_version_file = os.path.join(pipeline_version_file_path)
  pipeline_version = client.pipeline_uploads.upload_pipeline_version(pipeline_version_file, name=pipeline_version_name, pipelineid=pipeline.id)
```

* **host**: Your Kubeflow Pipelines cluster's host name.
* **pipeline file path**: The path to the directory where your pipeline YAML is stored.
* **pipeline name**: Your pipeline's file name.
* **pipeline version file path**: The path to the directory where your pipeline version YAML is stored.
* **pipeline version name**: Your pipeline version's file name.

Note that the pipeline names need to be unique across your KFP instance; while pipeline version names need to be unique under each pipeline.

### Using the SDK to create a pipeline version under an existing pipeline

If you want to create a new pipeline version under an existing pipeline, you only need to find out the pipeline id of the existing pipeline and use it with the method upload_pipeline_version. To find out the pipeline id, you can go to KFP Pipeline Details page of the selected pipeline. On that page, the pipeline id is listed in the summary card, as shown below.

<img src="/docs/images/sdk-examples-snapshot-1.png"
alt="Pipeline ID in Summary Card"
class="mt-3 mb-3 border border-info rounded">