# 🔄 MyApp Repository Restore

Restores your GitHub repository from secure backups stored in MyApp File Security with end-to-end encryption and OTP verification.

## ⚠️ Note
This is designed for empty repositories — it will overwrite all history

---

## 🚀 Quick Start

```yaml
name: Restore Repository
permissions: write-all

on:
  workflow_dispatch:
    inputs:
      otp_code:
        description: 'OTP CODE - Use it as blank for first run. Get OTP code via email and run again with OTP code. OTP code expires after 90 seconds'
        required: false

jobs:
  restore:
    runs-on: ubuntu-latest
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@latest
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          encryption_key: ${{ secrets.ENCRYPTION_KEY }}
          otp_code: ${{ github.event.inputs.OTP_CODE }}
          record_id: 'string'
```

##  🚀 How to Use in a Blank Repository
Follow these steps to set up and run the restore action successfully:

1️⃣ Create a New Blank Repository
  - Go to GitHub – New Repository
  - Create a new empty repository


2️⃣ Create Required Folders
  - If working locally:

```bash
mkdir -p .github/workflows
```
  - Or if using the GitHub web UI:
  Click "Add file" > "Create new file"
  - Name it: .github/workflows/restore.yml

3️⃣ Add the Workflow File
  - Paste the following content into restore.yml:

```yaml
name: Restore Repository

on:
  workflow_dispatch:
    inputs:
      record_id:
        description: 'Record ID'
        required: true

jobs:
  restore:
    runs-on: ubuntu-latest
    permissions: write-all
    steps:
      - name: Restore Repository
        uses: berkayy-atas/marketplace-test-workflow-restore@v1.0.17
        with:
          activation_code: ${{ secrets.ACTIVATION_CODE }}
          encryption_key: ${{ secrets.ENCRYPTION_KEY }}
          restore_github_token: ${{ secrets.RESTORE_GITHUB_TOKEN }}
          record_id: ${{ github.event.inputs.RECORD_ID }}
```
4️⃣ Add Required Secrets
  - Go to: Repository → Settings → Secrets and variables → Actions
  - Click "New repository secret" and add the following:

Secret Name	Description
  - ACTIVATION_CODE:	Your API activation token from File Security
  - RESTORE_GITHUB_TOKEN:	A personal access token (PAT) with repo and workflow access (you can claim your token: "Github Settings -> Developer Settings -> Personal access tokens (classic)") [Scopes: repo, workflow, admin:org, write:discussion] 
  - ENCRYPTION_KEY: A key to open the shielded file


5️⃣ Run the Workflow
  - Go to the Actions tab in your GitHub repository
  -Select Restore Repository
  - Click "Run workflow" (top right)
  - Fill in FILE_RECORD_ID

6️⃣ Enter OTP When Prompted
  - The workflow will pause and wait for you to provide the OTP
  - Check your email for the OTP code
  - Paste it into the GitHub UI field when prompted
  - The workflow will resume and complete the restore

✅ What This Action Does
  - Securely retrieves an activation token and sends an OTP
  - Downloads and extracts your backup
  - Pushes the full mirror to your GitHub repository

