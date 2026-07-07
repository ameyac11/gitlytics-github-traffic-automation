<div align="center">

<img src="./assets/logo.png" alt="Gitlytics Logo" width="150" />

# Gitlytics Traffic Sync

[![GitHub Actions](https://api.gitlytics.dev/api/badge/tech.svg?slug=githubactions&style=icon_label_value&label=GH%20Actions&label_color=%23555555&variant=plastic&value=Automated&value_color=%232088FF)](https://github.com/features/actions)
[![Python](https://api.gitlytics.dev/api/badge/tech.svg?slug=python&style=icon_label_value&label=Python&label_color=%23555555&variant=plastic&value=3.11%2B&value_color=%233776AB)](https://www.python.org/)
[![PyPI](https://api.gitlytics.dev/api/badge/pypi/gitlytics.svg?label=Powered%20by)](https://pypi.org/project/gitlytics/)
[![Live Dashboard](https://api.gitlytics.dev/api/badge/tech.svg?slug=livedemo&style=icon_label_value&label=Live%20Dashboard&label_color=%23555555&variant=plastic&value=dashboard.gitlytics.dev&value_color=%233FB950)](https://dashboard.gitlytics.dev)
[![Live Docs](https://api.gitlytics.dev/api/badge/tech.svg?slug=docs&style=icon_label_value&label=Live%20Docs&label_color=%23555555&variant=plastic&value=docs.gitlytics.dev&value_color=%233FB950)](https://docs.gitlytics.dev)

**Never lose your GitHub repository traffic data again!** 📈

Please consider giving this project a ⭐ if you find it helpful!

</div>

---

<div align="center">
  <img src="https://raw.githubusercontent.com/ameyac11/gitlytics/main/assets/gitlytics_thumbnail_1.png" width="100%" />
</div>

---

## 🔗 The Gitlytics Ecosystem

The full Gitlytics ecosystem spans across a few repositories. If you are looking for the core Python API or the web dashboard, check out the links below:

- 🐍 **[Gitlytics (Core Library)](https://github.com/ameyac11/gitlytics)**: The core Python library, REST API backend, and CLI tools.
- **[Gitlytics Web Ecosystem](https://github.com/ameyac11/gitlytics-deployement)**: The production landing page, React Dashboard, and React Documentation site.

---

## 🧐 Why do we need automation?

> [!WARNING]
> **The 14-Day Data Loss:** By default, GitHub only retains repository traffic data (views and clones) for the **latest 14 days**. After two weeks, your valuable historical data is permanently deleted and gone forever. 

If you want to track your repository's growth, analyze long-term trends, or just keep a permanent record of your project's popularity, you need a way to back up this data regularly. 

**This automation solves that problem perfectly.** Instead of running every day and wasting GitHub Action minutes, it smartly fetches your GitHub traffic data **every 13 days**. Since GitHub keeps 14 days of history, this creates a perfect 1-day overlap as a safety buffer—meaning 0 wasted minutes and absolutely no data missed!

---

## ✨ Features & How it Works

The repository is structured into three main components to ensure a clean and automated data extraction process:

### ⚙️ 1. The Automation Engine (`.github/workflows/`)
This folder contains the `fetch_traffic.yml` workflow file. It uses **GitHub Actions** to automatically trigger the data extraction script **every 13th day** at **17:00 UTC (10:30 PM IST)**. It runs completely in the background without any manual intervention, perfectly optimized to save CI/CD minutes.

### 🧠 2. The Core Logic (`gitlytics` package)
The workflow automatically installs and runs the official [**gitlytics** PyPI package](https://pypi.org/project/gitlytics/) to handle the heavy lifting:
- Securely authenticates with the GitHub API using your Personal Access Token.
- Fetches all available traffic data (Views, Clones, Stars, Forks, Unique Visitors).
- Intelligently merges new data with your historical data without any duplicates.

### 📂 3. The Data Storage (`data/`)
Instead of one massive file, the data is smartly organized into **monthly CSV files** (e.g., `traffic_2024-01.csv`). This makes it incredibly easy to read, export, and analyze your data over time.

---

## ⚡ Setup Guide

Choose one of the two options below to set up your traffic tracking:

### Option A: Reusable GitHub Action (Recommended)
Add traffic tracking directly to your existing repository.

#### Step 1: Create a Fine-Grained Personal Access Token (PAT)
1. Go to GitHub **Settings** > **Developer settings** > **Personal access tokens** > **Fine-grained tokens**.
2. Click **Generate new token**.
3. Select the repository you want to monitor.
4. Under **Repository permissions**, set **Administration** to **Read-only**.
5. Generate and copy the token.

#### Step 2: Add the Token to Secrets
1. In the repository you want to monitor, navigate to **Settings** > **Secrets and variables** > **Actions**.
2. Create a new repository secret named `TRAFFIC_TOKEN` and paste your token.

#### Step 3: Create the Workflow File
In your repository, create `.github/workflows/traffic.yml` and paste the following content:

```yaml
name: Gitlytics Traffic Sync

on:
  schedule:
    - cron: '0 17 */13 * *'
  workflow_dispatch:

jobs:
  backup:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Sync Traffic Data
        uses: ameyac11/gitlytics-action@v1
        with:
          traffic_token: ${{ secrets.TRAFFIC_TOKEN }}
```

---

### Option B: Dedicated Traffic Vault (Template Repository)
Keep all your repository traffic databases consolidated in a single, private vault repository.

#### Step 1: Use this Template
Click **Use this template** at the top right of this repository page and select **Create a new repository**.
> [!IMPORTANT]
> Make sure to set the new repository to **Private**!

#### Step 2: Create a Personal Access Token (PAT)
Generate a classic GitHub PAT with `repo` permissions to allow the sync process to fetch your repositories.

#### Step 3: Add the Token to Secrets
1. Go to your new private vault repository's settings.
2. Navigate to **Secrets and variables** > **Actions**.
3. Create a new repository secret named `TRAFFIC_TOKEN` and paste your PAT.

---

### 🚀 First Run Tip
Because the cron schedule runs every 13 days, the folders/CSV files will not appear immediately.
To populate your initial traffic data right away:
1. Go to the **Actions** tab in your repository.
2. Select the **Fetch GitHub Traffic** workflow.
3. Click the **Run workflow** button.

---

## 📊 Companion Traffic Dashboard

Raw CSV files are great, but visualizing them is even better! 

We have built a beautiful production **React Dashboard** to seamlessly visualize the data generated by this automation.

👉 **[View the Live Dashboard at dashboard.gitlytics.dev](https://dashboard.gitlytics.dev)**
👉 **[Read the Full Documentation](https://docs.gitlytics.dev)**

**How to use it:**
1. **CSV Upload:** Directly upload your monthly CSV files generated by this automation into the dashboard to visualize your accumulated historical data.
2. **Real-time API Fetch:** If you want on-demand, real-time traffic stats, you can connect your GitHub account directly using your Personal Access Token (PAT).

---

## 📜 License

This project is licensed under the Apache 2.0 License. See the [LICENSE](LICENSE) file for more details.

<div align="center">
  <i>Built with ❤️ for the Open Source Community.</i>
</div>
