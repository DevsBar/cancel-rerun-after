# ReRun After - GitHub Application
ReRun After is a **GitHub application** designed to enhance your CI/CD pipeline by offering to rerun your workflow; you can **easily rerun entire workflows or just the failed jobs within up to 24 days delay**.

## Features
### Delayed Rerun Options
ReRun Workflow allows you to **schedule your reruns up to 24 days in advance**. 

### Rerun Failed Jobs
ReRun After can choose to run the entire workflow or just failed jobs, giving you flexibility in your decision-making.

### GitHub Runner Optimization
Avoid tying up GitHub runners unnecessarily. ReRun Workflow ensures that you use runners only when needed, **reducing the costs** associated with GitHub Actions usage.

### Easy to Use
You can install this GitHub App on your repositories and use the GitHub action to rerun your workflow.

## How to Install
Follow these simple steps to install ReRun After in your GitHub repository:
1. Navigate to the GitHub Marketplace and search for "Rerun After" or go to this [Link](https://github.com/apps/rerun-after)
2. Click On Install and choose the plan you need it
3. Choose the repositories where you want to apply the app or select all repositories for universal access.

## Usage
After installation, integrate ReRun Workflow into your GitHub Actions with this simple action setup:

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
          uses: DevsBar/rerun-after@V1.3
          with:
            workflow-id: ${{ github.run_id }}
            time: 3600
            rerun-all-jobs: true
            failed-workflow: false
```

## Canceling the rerun request
You can use another Github action to simply cancel the rerun request
Below demonstrate using the cancel requst

```yaml
name: Cancel Workflow Rerun
on: [workflow_dispatch]
jobs:
  Deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
    steps:
        - name: Rerun the workflow after 1 hour
          uses: DevsBar/cancel-rerun-after@V1.3
          with:
            workflow-id: ${{ github.run_id }}
```

This configuration snippet sets up an action to delay reruns and specifies whether to rerun all jobs or only those that have failed.

### Parameters
| Key | Description | Required | Default Value |
| --- | --- | --- | --- |
| Workflow-id | The workflow ID of the workflow that needs to be rerun; current workflow id `${{ github.run_id }}` | `true` | `-` |
| time | The delay time in seconds, minimum `10`, maximum `2500000` | `false` | `120` |
| rerun-all-jobs | Rerun all the jobs even if it is successfully done | `false` | `false` |
| rerun-all-jobs | Make the workflow failed after this step | `false` | `false` |


## Benefits
- Cost Efficiency: Reduces the financial impact of using GitHub runners by optimizing the usage time.
- Flexibility: Offers up to 24 days to reschedule failed jobs or full workflows, providing ample time for issue resolutions.
- Focus on Stability: Concentrates efforts on fixing failed jobs, enhancing the overall stability and reliability of your deployments.