# Lotto Draw History API

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A REST API providing historical draw results for Polish Lotto and Lotto Plus games. The API supports JSON and CSV formats and includes automated updates for new draws.

## Features

- **Historical Data Endpoint**: Fetch all historical results via `/api/historical-draw-results`.
- **Multiple Formats**: Set `Accept: application/json` (default) or `Accept: application/octet-stream` for CSV.
- **Auto-Update**: New results are added automatically 30 minutes after each draw (Tue/Thu/Fri at 22:30 CET/CEST).
- **Initialization Tools**: Python scripts to populate the database with historical data.

## Getting Started

### Prerequisites
- .NET 9 SDK
- Azure account with active subscription
- Python (for initialization scripts)
- Lotto API key from [Lotto Developers Portal](https://developers.lotto.pl/)

## Deployment via GitHub Actions

1. **Initialize Azure Resources**:
   ```bash
   ./azure-init.sh
   ```

   Follow prompts to create resources. This generates `azure-credentials.json`.

2. **Configure Secrets** in your GitHub repository:
   - `LOTTO_API_KEY`: Your Lotto.pl API key
   - `AZURE_CREDENTIALS`: Contents of `azure-credentials.json`
   - `AZURE_RESOURCE_GROUP`: Your Azure resource group name

3. **Trigger Deployment Workflow**:

   Push to main branch or manually run "Build and Deploy Azure Resources" action
   
## Post-Deployment Setup

1. **Prepare Python Environment**:
   ```bash
   cd tools
   python -m venv .venv
   source .venv/bin/activate  # Linux/MacOS
   pip install -r requirements.txt
   ```

2. **Configure Environment**:
   Copy `.env.template` to `.env`
   Fill values:
   ```env
   LOTTO_API_KEY="<your-api-key>"
   USER_AGENT="Your Name/Contact Info"
   STORAGE_CONNECTION_STRING="<from Azure Portal>"
   ```

3. **Initialize Historical Data**:
   ```bash
   # Fetch data from Lotto.pl API (default start date: 2000-01-01)
   python fetch_to_csv.py -d 2000-01-01 -f data.csv

   # Upload to Azure Storage
   python csv_to_storage.py -f
   ```

## Future Improvements

APIM with some of the Lotto API endpoints integrated.
