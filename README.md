# GitHub Traffic Automation

Please consider giving this project a ⭐ if you find it helpful!

## What it does
It fetches your GitHub traffic data every day and appends it to `data/traffic_history.json`. It automatically runs via GitHub Actions, letting you save historical traffic data permanently (GitHub normally only retains this for 14 days).

## Setup
1. Create a GitHub Personal Access Token (PAT) with `repo` permissions.
2. Go to your repository settings > Secrets and variables > Actions.
3. Add a new repository secret named `TRAFFIC_TOKEN` and paste your PAT.
4. The GitHub Action will automatically run daily at 00:00 UTC to update your data.

## Companion Repository
You can visualize this data using the dashboard: [github-traffic-dashboard](https://github.com/ameyac11/github-traffic-dashboard)

## License
Apache 2.0
