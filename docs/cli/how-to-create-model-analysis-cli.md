# How to create a Model Analysis Job in AzureML CLI

This sample will walk you through creating a simple Model analysis dashboard with a single component attatched.

## Installing Responsible AI Components

## Setting up the CLI and useful CLI commands

Login from CLI:
```CLI
az login --tenant <your_tenant_name_such_us_microsoft.onmicrosoft.com>
```
Check defaults set:
```CLI
az configure
```
Set by default Resource Group:
```CLI
az configure --defaults group=<your_resource_group_name> location=<your_azure_region>
```
Set by default AML Workspace:
```CLI
az configure --defaults workspace=<your_workspace_name>
```

## Understanding Model Analysis Job Specification


## Initiate Model Analysis Job

The .YAML config below will initiate your model analysis. There are a few parameters that will need to be ready in advance.

```YAML
$schema: http://azureml/sdk-2-0/CommandComponent.json
name: AzureMLModelAnalysis
display_name: Model Analysis with azureml-responsibleai
version: VERSION_REPLACEMENT_STRING
type: command_component
inputs:
  title:
    type: string
  task_type:
    type: string # [classification, regression]
    enum: ['classification', 'regression']
  model_info_path:
    type: path # To model_info.json
  train_dataset:
    type: path # Must be Parquet
  test_dataset:
    type: path # Must be Parquet
  target_column_name:
    type: string
  X_column_names:
    type: string # List[str]
  datastore_name:
    type: string
  maximum_rows_for_test_dataset:
    type: integer
    default: 5000
  categorical_column_names:
    type: string # Optional[List[str]]
outputs:
  model_analysis_info:
    type: path
code:
  local_path: ./rai_analyse/
environment: azureml:AML-RAI-Environment:VERSION_REPLACEMENT_STRING
command: >-
  python create_model_analysis.py
  --title '${{inputs.title}}'
  --task_type ${{inputs.task_type}}
  --model_info_path ${{inputs.model_info_path}}
  --train_dataset ${{inputs.train_dataset}}
  --test_dataset ${{inputs.test_dataset}}
  --target_column_name ${{inputs.target_column_name}}
  --X_column_names '${{inputs.X_column_names}}'
  --datastore_name ${{inputs.datastore_name}}
  --maximum_rows_for_test_dataset ${{inputs.maximum_rows_for_test_dataset}}
  --categorical_column_names '${{inputs.categorical_column_names}}'
  --output_path ${{outputs.model_analysis_info}}
```

2. Once your Model Analysis is initiated, you will be able to add any compatible component to your analysis which will be rendered for you in a single dashboard. Each of these components have their own .YAML configurations, for this example we will be using x. [Explore other components here]()