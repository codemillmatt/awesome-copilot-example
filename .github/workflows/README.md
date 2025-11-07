# GitHub Actions Workflows

## Traffic Reporting Workflow

The `traffic-reporting.yml` workflow automatically fetches and reports repository
traffic statistics to an external API.

### Features

- Fetches GitHub repository traffic data (page views and unique views)
- Retrieves data for the last 14 days
- Sends data to a configurable external API endpoint
- Runs on-demand via manual trigger
- Can be configured to run on a schedule

### Configuration

#### Required Secret

Set the following repository secret in GitHub:

- `TRAFFIC_REPORT_URL`: The URL of the external API endpoint that will
  receive the traffic data

To add the secret:

1. Go to your repository on GitHub
2. Click on **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `TRAFFIC_REPORT_URL`
5. Value: Your API endpoint URL
6. Click **Add secret**

### Usage

#### Manual Trigger

1. Go to the **Actions** tab in your repository
2. Select **Traffic Reporting** workflow from the left sidebar
3. Click **Run workflow** button
4. Select the branch (usually `main`)
5. Click **Run workflow**

#### Schedule (Future)

To enable scheduled runs, uncomment the schedule section in the workflow file:

```yaml
schedule:
  - cron: '0 0 * * *'  # Runs daily at midnight UTC
```

### Data Format

The workflow sends a POST request with JSON data containing:

- `count`: Total number of views in the last 14 days
- `uniques`: Total number of unique visitors in the last 14 days
- `views`: Array of daily traffic data, each containing:
  - `timestamp`: Date of the record
  - `count`: Number of views on that date
  - `uniques`: Number of unique visitors on that date

### Troubleshooting

If the `TRAFFIC_REPORT_URL` secret is not set, the workflow will:

- Display a warning message
- Show the traffic data that would have been sent
- Complete successfully without sending data

This allows you to test the workflow before configuring the external API.
