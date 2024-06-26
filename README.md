# ReRun After - GitHub Application

ReRun After is a **GitHub application** designed to enhance your CI/CD pipeline by allowing you to rerun workflows or just the failed jobs within a delay of up to 24 days. This application offers flexibility and efficiency, ensuring that your deployments are reliable and cost-effective.

## Features

### Delayed Rerun Options
ReRun After allows you to **schedule your reruns up to 24 days in advance**.

### Rerun Failed Jobs
ReRun After can rerun the entire workflow or just the failed jobs, providing flexibility in your decision-making.

### Overwrite Your Rerun Request
You can easily replace your rerun request before the scheduled time.

### Cancel Rerun Request
You can cancel your rerun request before the scheduled time.

### GitHub Runner Optimization
Avoid tying up GitHub runners unnecessarily. ReRun After ensures that runners are used only when needed, **reducing the costs** associated with GitHub Actions usage.

### Easy to Use
Install the GitHub App on your repositories and use the GitHub action to rerun your workflow.

## How to Install
Follow these simple steps to install ReRun After in your GitHub repository:
1. Navigate to the GitHub Marketplace and search for "Rerun After" or go to this [link](https://github.com/apps/rerun-after).
2. Click on "Install" and choose the plan you need.
3. Select the repositories where you want to apply the app or choose all repositories for universal access.

## Usage
After installation, integrate ReRun After into your GitHub Actions with this simple action setup:

```yaml
name: Delayed Workflow Rerun
on: [workflow_dispatch]
jobs:
  Deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
        - name: Rerun the workflow after 1 hour
          uses: DevsBar/rerun-after@V1.4
          with:
            workflow-id: ${{ github.run_id }}
            time: 3600
            rerun-all-jobs: true
            failed-workflow: false
```

This configuration snippet sets up an action to delay reruns and specifies whether to rerun all jobs or only those that have failed.

***Find more examples at the end of the readme.***

## Canceling the rerun request
Use another GitHub action to cancel the rerun request. Below is an example:

```yaml
name: Demo to cancel rerun request
on:
  workflow_dispatch: 
    inputs:
      workflow_id:
        description: 'Workflow ID'
        required: true
jobs:
  Deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
        - name: Cancel rerun request
          uses: DevsBar/cancel-rerun-after@V1.4
          with:
            workflow-id: ${{ inputs.workflow_id }}
```

### Parameters
| Key | Description | Required | Default Value |
| --- | --- | --- | --- |
| Workflow-id | The workflow id of the workflow that needs to be rerun; current workflow id is accessible with `${{ github.run_id }}` | `true` | `-` |
| time | The delay time in seconds, minimum `10`, maximum `2500000` | `false` | `120` |
| rerun-all-jobs | Rerun all the jobs even if it is successfully done | `false` | `false` |
| rerun-all-jobs | Make the workflow failed after this step | `false` | `false` |


## Benefits
- Cost Efficiency: Reduces the financial impact of using GitHub runners by optimizing usage time.
- Flexibility: Offers up to 24 days to reschedule failed jobs or full workflows, providing ample time for issue resolutions.
- Focus on Stability: Concentrates efforts on fixing failed jobs, enhancing the overall stability and reliability of your deployments.

## More Example

```yaml
name: Check if the api is ready otherwise rerun the workflow after 1 hour
on:
  push:
    branches:
      - main

jobs:
  Deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
      - shell: bash
        run: npm i node-fetch@2

      - id: check-api
        name: Check if the api is ready
        uses: actions/github-script@v7
        with:
          script: |
            const fetch = require('node-fetch');
            const response = await fetch('http://localhost:3000');

            if (response.status === 200) {
              core.setOutput('rerun', 'false');
            } else {
              core.setOutput('rerun', 'true');
            }

      - name: Rerun the workflow after 1 hour
        uses: DevsBar/rerun-after@V1.4
        if: steps.check-api.outputs.rerun == 'true'
        with:
          workflow-id: ${{ github.run_id }}
          time: 3600
          rerun-all-jobs: true
          failed-workflow: true
      
      - name: Doing something
        run: echo "Doing something"
```